---
title: Why LoRA Isn't Enough for Domain Pretraining
date: 2026-03-31 09:00:00 +0200
categories: [AIML]
tags: [llm, finetuning, lora, telecom, nlp, pretraining]
author: naresh
---

When adapting a large language model to a new domain, LoRA is usually the first tool researchers reach for. It is fast, memory-efficient, and works remarkably well for instruction fine-tuning. But when the goal is domain pretraining on a large specialised corpus — not just teaching a model to follow instructions, but fundamentally shifting its knowledge distribution — LoRA has a ceiling. This post documents what we found when we hit that ceiling while building [tele-SLMs](https://github.com/NareshModina/tele-SLMs), a project to adapt sub-2B parameter language models to 3GPP and ETSI telecommunications standards.

## What LoRA Does (and Does Not Do)

LoRA (Low-Rank Adaptation) freezes the original model weights and injects small trainable matrices into the attention layers [[1]](#references). Instead of updating all parameters, it trains a low-rank decomposition `ΔW = BA` where `B` and `A` are much smaller matrices. For a model with billions of parameters, this means training less than 1% of the total weights — dramatically reducing memory and compute requirements.

This approach works extremely well for supervised fine-tuning (SFT) tasks: instruction following, classification, summarisation, and chat alignment. The model's general knowledge stays intact while the adapter steers its behaviour toward the target task. For these use cases, LoRA is genuinely the right tool.

The question is whether the same approach holds when the goal is not behavioural alignment but *knowledge injection* — forcing the model to deeply absorb a large, specialised corpus it has seen only superficially during pretraining.

## The Pilot Experiment

As a proof of concept before committing to full fine-tuning, we ran a two-phase LoRA pilot on `Qwen2.5-1.5B` using [Tele-Data](https://huggingface.co/datasets/AliMaatouk/Tele-Data) [[2]](#references), a telecommunications corpus of approximately 2.5 billion tokens spanning 3GPP standards, arXiv papers, Wikipedia articles, and web content.

**Phase 1 — Continual pretraining** with LoRA (r=64, α=128) on ~1.26B tokens, trained for 2 epochs across 33,442 steps on 3× NVIDIA L40S GPUs.

**Phase 2 — Instruction fine-tuning** with LoRA SFT on Alpaca (52k examples).

The training loss for Phase 1 dropped from 1.76 to 1.48 — a reasonable reduction. Perplexity on Tele-Eval [[2]](#references) improved from 8.90 to 8.35, a modest 6% improvement. But the Token F1 on the benchmark actually regressed slightly, from 31.20% to 30.08%.

The perplexity improvement confirmed that the model's distribution had shifted toward the telecom domain. The F1 regression was a format mismatch — the pretrained model produced verbose, standards-style responses that overlapped poorly with the terse gold answers in Tele-Eval — not a loss of domain knowledge. But the perplexity gain was smaller than expected for a corpus of this size and two full epochs of training. Something was limiting the model's ability to absorb the domain signal.

## Why LoRA Saturates

The capacity argument is straightforward. LoRA constrains weight updates to a low-rank subspace. For task-specific fine-tuning, where the model already has the relevant knowledge and simply needs its behaviour redirected, this constraint is acceptable. But for continual pretraining on a large corpus, the model needs to update a much broader set of representations — clause structure, technical terminology, cross-document reasoning patterns, equation formatting — across all layers simultaneously. A rank-64 adapter, representing roughly 1.3% of total parameters in a 1.5B model, is a narrow bottleneck for this kind of wholesale distribution shift.

The Tele-LLMs paper [[2]](#references) provides direct empirical evidence of this limitation. In their comparison of LoRA versus full fine-tuning on both Gemma-2B and LLaMA-3-8B, they observed that for smaller models, LoRA initially injects telecom knowledge but quickly saturates due to limited capacity. For LLaMA-3-8B, the gradient norm with LoRA remained extremely low throughout training, causing the training loss to barely change at all. The adapter had effectively stopped learning. Full fine-tuning showed no such saturation — loss continued to decrease steadily across both epochs.

## Switching to Full Fine-Tuning

For the main tele-SLMs pipeline, we switched to full fine-tuning for the pretraining stage and kept LoRA only for the subsequent SFT step. This separation is deliberate:

- **Full FT for pretraining** — all weights updated freely, no rank bottleneck, maximum capacity for domain knowledge absorption
- **LoRA for SFT** — base weights frozen after pretraining, adapter adds instruction-following behaviour on top without overwriting domain knowledge

This hybrid approach avoids the capacity problem while still protecting the domain knowledge during the instruction alignment phase. Starting from a base model (not an instruct variant) means there is no instruction-following capability to destroy during full FT pretraining — the model has a single job at that stage: learn the domain.

We rebuilt the training corpus from scratch, replacing Tele-Data with [TeleSpec-Data](https://huggingface.co/datasets/nareshmodina/TeleSpec-Data) — a standards-only dataset of 38,302 documents (15,054 3GPP + 23,248 ETSI) with zero arXiv content. After token packing into 2048-token blocks, this produced 914,321 training examples and 1.87 billion tokens — 24× more training examples than naive truncation would have produced from the same documents.

Running the full pipeline on `SmolLM2-135M`, the training loss dropped from 1.498 to 1.141 over approximately 14,000 steps — a 0.357 reduction compared to the pilot's 0.32 reduction on a model 11× larger. More tellingly, the answer perplexity on standards-derived Tele-Eval questions improved by **31%** when comparing pretrain+SFT against SFT-only (12.92 → 8.85). The LoRA pilot, by contrast, achieved only a 6% perplexity improvement over the base model. The difference in signal strength between low-rank adaptation and full fine-tuning is stark.

## When to Use LoRA vs Full Fine-Tuning

This is not an argument against LoRA in general — it is an argument for matching the method to the task.

| Task | Recommended approach |
|---|---|
| Instruction fine-tuning / chat alignment | LoRA |
| Task-specific fine-tuning on small datasets | LoRA |
| Domain pretraining on large corpora (>500M tokens) | Full fine-tuning |
| Domain pretraining followed by instruction alignment | Full FT → LoRA SFT |

The intuition is simple: LoRA modifies *behaviour*, full fine-tuning modifies *knowledge*. When the goal is deep domain adaptation, only full fine-tuning has sufficient capacity.

## Conclusion

LoRA is one of the most useful tools in the fine-tuning toolkit, but it has a fundamental capacity limit that becomes apparent when the pretraining corpus is large and the required domain shift is substantial. For telecom standards adaptation — a domain with highly specialised terminology, dense technical structure, and very different token distributions from general web text — full fine-tuning is necessary to meaningfully shift the model's knowledge distribution.

The practical recommendation: use full fine-tuning for pretraining on domain corpora, and reserve LoRA for the SFT stage where its efficiency advantages are most relevant and its capacity limitations matter least.

The code, dataset, and trained models for this project are available at [github.com/NareshModina/tele-SLMs](https://github.com/NareshModina/tele-SLMs).

---

## References

1. Hu, E. J., et al. (2022). [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685). *ICLR 2022*.
2. Maatouk, A., Chirino Ampudia, K., Ying, R., & Tassiulas, L. (2024). [Tele-LLMs: A Series of Specialized Large Language Models for Telecommunications](https://arxiv.org/abs/2409.05314). *arXiv:2409.05314*.
