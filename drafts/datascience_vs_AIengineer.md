---
title: Data scientist vs AI engineer
date: 2023-06-19 17:00:00 +/-TTTT0200
categories: [Programming]
tags: [data scientist, ai engineer]     # TAG names should always be lowercase
author: naresh
---

## Data scientist
---
Use AI models for analysing the data
1. Use cases
    - Descriptive usecase: 
        - Exploratory Data Analysis (EDA) --> graph the data and run statistical inference

        - Clustering: Group similar data points based on similar charecteristics
    - Predictive usecase: Use ML to predict outcomes
        - Regression models
        - clsassification
2. Data
    - Structrued data: 100s to 100000s observations. This requires a lot of cleaning and data preporcessing.
3. Models: Choose from hundreds of different models and algorithms.
    - Different usecases requires different dataset and hence training different models
    - Such trained models have narrow scope
    - Less number of parameters to optimize, so compute power is relatively low, so is traing time (seconds to hours).
## AI Engineer
---
Generative AI is also referred as AI engieering
1. Usecases
    - Use foundation models to build AI systems
    - Prescriptive usecase: Choosing the best course of action such as decision optimization or recommendation engines
    - Generative usecase: Assistants, chatbots
2. Data
    - Unstructrued data: such as text, images or videos. Billions to trillion of tokens of text data
3. Model: Foundation Models
    - Capable of generalizing a lot of tasks without retraining for every single task
    - Due to this wide range of scope, generally they contain large number of parameters (millions to trillions), requires massive compute power, so the training time is often very large (days to months)


 