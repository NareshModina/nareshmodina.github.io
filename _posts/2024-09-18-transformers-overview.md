---
title: Overview of transformers library
date: 2024-09-18 16:00:00 +/-TTTT0200
categories: [AIML]
tags: [transformers pipeling, hugging face]     # TAG names should always be lowercase
author: naresh
---

The basic setup involves the following steps

(Raw data)  ---> Tokenizer (input ids) ---> Model (logits) ---> Post processing ---> prediction

## Tokenizer
 Transformers can not process text input, so they must be converted to numbers called token ids
 - Tokens: splitting the input into words, sub words, symbols.
 - Map such token to integers
 - other inputs such as attention masks, etc.
 [AutoTokenizer](https://huggingface.co/docs/transformers/main/en/model_doc/auto#transformers.AutoTokenizer){:target="_blank"} is a library that contains various tokenizers used in different models. A tokenizer's algorithm and vocabulary can be loaded using from_pretrained("modelname") method. Similarly, tokenizer can be saved using save_pretrained() method. Besides, tokenizers can be loaded directly from a specific model such as BertTokenizer

 ```Python 
    from Transformers import AutoTokenizer
    checkpoint = "albert"
    tokenizer = AutoTokenizer.from_pretrained(checkpoint)
```

```Python 
    from transformers import BertTokenizer
    from transformers import AutoTokenizer
    ## encoding
    checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
    tokenizer1 = BertTokenizer.from_pretrained("bert-base-cased")
    tokenizer2 = AutoTokenizer.from_pretrained(checkpoint)
    tokens1 = tokenizer1("This is a test sentence for tokenization", padding=True, truncation=True, return_tensors="pt")
    tokens2 = tokenizer2.tokenize("This is a test sentence for tokenization")
    print(tokens1)
    print(tokens2)
    ids = tokenizer.convert_tokens_to_ids(tokens)
    print(ids)
    # decoding
    string_decoded = tokenizer.decode([2023, 2003, 1037, 3231, 2741, 6651, 2005, 19204, 3989])
    print(string_decoded)
```

## Model 
Similar to Tokenizer, a model can be loaded using a specific modelname or instantiated using [AutoModel](https://huggingface.co/docs/transformers/model_doc/auto#transformers.AutoModel){:target="_blank"} class.

```Python 
    from transformers import BertConfig, BertModel
    config = BertConfig()
    model = BertModel(config)
    print(config)
```

Alternative loading method

```Python 
    from transformers import AutoModel
    Model = AutoModel.from_pretrained("google-bert/bert-base-uncased")
    config = Model.config
    print(config)
```

> **Gated Models** 
> ```Python 
>    from transformers import AutoModel
>    Model = AutoModel.from_pretrained("google-bert/bert-base-uncased")
>    config = Model.config
>    print(config)