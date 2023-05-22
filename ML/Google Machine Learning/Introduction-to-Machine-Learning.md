[TOC]

# What is Machine Learning

ML is the process of **[training](https://developers.google.com/machine-learning/glossary#training)** a piece of software, called a [**model**](https://developers.google.com/machine-learning/glossary#model), to make useful [**predictions**](https://developers.google.com/machine-learning/glossary#prediction) from data

Traditional Approach vs ML approach



## Types of ML Systems

- Supervised Learning
- Unsupervised Learning
- Reinforcement Learning



## Supervised Learning

ML systems are “supervised” in the sense that a human gives the ML system **data with the known correct results**.



### Regression

A [regression model](https://developers.google.com/machine-learning/glossary#regression-model) predicts a **numeric value**. For example, a weather model that predicts the amount of rain, in inches or millimeters, is a regression model.



### Classification

[Classification models](https://developers.google.com/machine-learning/glossary#classification-model) predict the likelihood that something belongs to a category.

classification models output a value that states whether or not something belongs to a particular category

- Binary Classification
- Multiclass classification



## Unsupervised Learning

[Unsupervised learning](https://developers.google.com/machine-learning/glossary#unsupervised-machine-learning) models make predictions by being given data that does not contain any correct answers. A commonly used unsupervised learning model employs a technique called [**clustering**](https://developers.google.com/machine-learning/glossary#clustering). The model finds data points that demarcate natural groupings.



Clustering vs Categorization

The categories aren't defined by you



## Reinforcement Learning

[Reinforcement learning](https://developers.google.com/machine-learning/glossary#reinforcement-learning-rl) models make predictions by getting [rewards](https://developers.google.com/machine-learning/glossary#reward) or penalties based on actions performed within an environment. A reinforcement learning system generates a [policy](https://developers.google.com/machine-learning/glossary#policy) that **defines the best strategy** for getting the most rewards.



# Supervised Learning

The dominant ML system (at Google)



## Foundational Concepts:

- Data
- Model
- Training
- Evaluating
- Inference



## Data

Datasets are made up of individual [examples](https://developers.google.com/machine-learning/glossary#example) that contain [features](https://developers.google.com/machine-learning/glossary#feature) and a [**label**](https://developers.google.com/machine-learning/glossary#label).

what is label? The "answer" or "result" portion of an example



## Dataset Characteristics

Size:  indicates the number of examples

Diversity:  indicates the range those examples cover

A large dataset doesn't guarantee sufficient diversity



Another characteristic: Number of features

Datasets with more features can help a model discover additional patterns and make better predictions. However, datasets with more features **don't *always*** produce models that make better predictions because some features might have no causal relationship to the label.



## Model

A model is the **complex collection of numbers** that define the mathematical **relationship** from specific input feature patterns to specific output label values.

Model discovers the patterns through *training* 



## Training

A model is given a dataset with *labeled* examples

The model finds the best solution by comparing its predicted value to the label's actual value.

Loss --  the difference between the predicted and actual values

During training, ML practitioners can make subtle adjustments to the configurations and features the model uses to make predictions.



## Evaluating

When we evaluate a model, we use a labeled dataset, but **we only give the model the dataset's features**. We then compare the model's predictions to the label's true values.



## Inference

Def: Use the model to make predictions, called [inferences](https://developers.google.com/machine-learning/glossary#inference)