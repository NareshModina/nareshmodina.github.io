---
title: "Overview of the Transformers Library"
date: 2026-04-14 09:00:00 +0200
categories: [AIML]
tags: [transformers, huggingface, nlp, tokenizer]
author: naresh
---

The Hugging Face `transformers` library provides a unified API for loading, running, and fine-tuning pretrained models. The basic pipeline looks like this:

```
(Raw text) → Tokenizer (input ids) → Model (logits) → Post-processing → Prediction
```

## Tokenizer

Transformers cannot process raw text — input must first be converted to integers called token ids. A tokenizer handles three things: splitting input into tokens (words, subwords, or symbols), mapping those tokens to integers, and producing auxiliary inputs like attention masks.

[AutoTokenizer](https://huggingface.co/docs/transformers/main/en/model_doc/auto#transformers.AutoTokenizer){:target="_blank"} selects the correct tokenizer automatically based on the model checkpoint. You can load and save tokenizers using `from_pretrained()` and `save_pretrained()`.

```python
from transformers import AutoTokenizer, BertTokenizer

# Load via AutoTokenizer
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

# Load a specific tokenizer directly
bert_tokenizer = BertTokenizer.from_pretrained("bert-base-cased")

# Encoding
tokens = tokenizer("This is a test sentence", padding=True, truncation=True, return_tensors="pt")
print(tokens)

# Tokenize to strings
token_strings = tokenizer.tokenize("This is a test sentence")
ids = tokenizer.convert_tokens_to_ids(token_strings)
print(ids)

# Decoding
decoded = tokenizer.decode([2023, 2003, 1037, 3231, 6651])
print(decoded)
```

## Model

Similar to tokenizers, models can be loaded via a specific class or through [AutoModel](https://huggingface.co/docs/transformers/model_doc/auto#transformers.AutoModel){:target="_blank"}, which infers the correct architecture from the checkpoint name.

```python
from transformers import BertConfig, BertModel

# Instantiate from config (randomly initialised weights)
config = BertConfig()
model = BertModel(config)
print(config)
```

```python
from transformers import AutoModel

# Load pretrained weights
model = AutoModel.from_pretrained("google-bert/bert-base-uncased")
print(model.config)
```

> **Gated models**
> Some models on the Hub require accepting a licence before access is granted (e.g. Llama, Mistral). Pass your Hugging Face token explicitly:
> ```python
> from transformers import AutoModel
> model = AutoModel.from_pretrained("meta-llama/Llama-2-7b-hf", token="hf_your_token_here")
> ```
> You can also set `HF_TOKEN` as an environment variable and the library will pick it up automatically.
