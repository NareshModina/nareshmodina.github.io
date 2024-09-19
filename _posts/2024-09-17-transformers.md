---
title: Transformers
date: 2024-09-17 17:00:00 +/-TTTT0200
categories: [AIML]
tags: [transformers, hugging face]     # TAG names should always be lowercase
author: naresh
---

## Generic Transformer Models

- Encoder models : these models posses bidirectional attention, these are often referred as auto encoding models. The training is performed a perturbed (by masking words) sentence and reconstructing the initial sentence. These are suitable for sentence classification, named entity recognition (ner), Q&A.
    - [BERT](https://huggingface.co/docs/transformers/model_doc/albert)
    - [ALBERT](https://huggingface.co/docs/transformers/model_doc/bert)
    - [RoBERTa](https://huggingface.co/docs/transformers/model_doc/roberta)

- Decoder models: These models are often referred as auto-regressive models. The training process involves predicting next word. It is trivial to see that these are used for text generation.
    - [GPT - 2](https://huggingface.co/docs/transformers/model_doc/gpt2)
    - [CTRL](https://huggingface.co/docs/transformers/model_doc/ctrl)
    - [TransformerXL](https://huggingface.co/docs/transformers/model_doc/transfo-xl)

- Encoder-decoder models: These are some times called as sequence to sequence models, these are best suited for summarization, generative Q&A, translation.
    - [BART](https://huggingface.co/docs/transformers/model_doc/bart)
    - [Marian](https://huggingface.co/docs/transformers/model_doc/marian)
    - [T5](https://huggingface.co/docs/transformers/model_doc/t5)

- Bias and limitations
    - The original data may contain best and worst of what's available on the internet
    - These inherent biases won't disappear even with fine-tuning as the original model may contain the bias
    - Training a new model requires huge computational capacity, which is not possible for all.

    [source1](https://user.phil.hhu.de/~cwurm/wp-content/uploads/2020/01/7181-attention-is-all-you-need.pdf){:target="_blank"} - [source2](https://huggingface.co/learn/nlp-course/chapter1/7){:target="_blank"}