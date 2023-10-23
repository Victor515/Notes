[TOC]

At a high level, ML problem framing consists of two distinct steps:

1. Determining whether ML is the right approach for solving a problem.
2. **Framing the problem in ML terms.**



# Understand the Problem

- **State the goal** for the product you are developing or refactoring.
- Determine whether the goal is best solved using ML.
- Verify you have the data required to train a model.



## Clear Use Case for ML

If you don't have a non-ML solution implemented, try solving the problem manually using a [**heuristic**](https://developers.google.com/machine-learning/glossary#heuristic). The non-ML solution is the benchmark you'll use to determine whether ML is a good use case for your problem.

- **Quality**. How much **better** do you think an ML solution can be? If you think an ML solution might be only a small improvement, that might indicate the current solution is the best one.
- **Cost and maintenance**. How expensive is the ML solution in both the short- and long-term?



## Data

Desired characteristic:

- Abundant
- Consistent and reliable
- Trusted
- Available
- Correct
- Representative

### Predictive Power

Some features will have more predictive power than others. 

Be aware that a feature's predictive power can change because the context or domain changes.

You can manually explore a feature's predictive power by **removing and adding it while training a model**. You can automate finding a feature's predictive power by using algorithms



## Predictions vs. actions

your product should take action from the model's output.



# Framing an ML Problem

- Define the ideal outcome and the model's goal.
- Identify the model's output.
- Define success metrics.



## Define the ideal outcome and the model's goal

This should be the same statement defined when "Understand your problem"



### Choose the right kind of model

A [**classification model**](https://developers.google.com/machine-learning/glossary#classification-model) predicts what category the input data belongs to, for example, whether an input should be classified as A, B, or C.

A [**regression model**](https://developers.google.com/machine-learning/glossary#regression-model) predicts where to place the input data on a number line.

Model output -> Actions



Case study: video cache prediction

Observation: when training the regression model, you realize that it produces the same loss for a prediction of 28 and 32 for videos that have 30 views. In this scenario, a classification model would produce the correct behavior because a classification model would produce a higher loss for a prediction of 28 than 32.

**Regression models are unaware of product-defined thresholds.**



Learning:

* **Predict the decision**. When possible, predict the decision your app will take.
* **Understand the problem's constraints**. If your app takes different actions based on different thresholds, determine if those thresholds are fixed or dynamic.
  * **Dynamic thresholds**: If thresholds are dynamic, use a regression model and set the thresholds limits in your app's code. This lets you easily update the thresholds while still having the model make reasonable predictions.
  * **Fixed thresholds**: If thresholds are fixed, use a classification model and label your datasets based on the threshold limits.



## Identify the model's output

Classification Model

![A classification flowchart.](https://developers.google.com/static/machine-learning/problem-framing/images/classification-flowchart.png)

Regression Model:

![A regression flowchart.](https://developers.google.com/static/machine-learning/problem-framing/images/regression-flowchart.png)



Sometimes we don't have a one-to-one relationship between **ideal outcome** and the **label**. Therefore, we need a proxy label.



### Proxy Labels

Proxy labels are necessary when you can't directly measure what you want to predict.

No proxy label can be a perfect substitute for your ideal outcome. All will have potential problems. Pick the one that has the least problems for your use case.



# Define the success metrics

* Success metrics define what you care about, like engagement or helping users take appropriate action
* Success metrics **differ** from the model's evaluation metrics, like [**accuracy**](https://developers.google.com/machine-learning/glossary#accuracy), [**precision**](https://developers.google.com/machine-learning/glossary#precision), [**recall**](https://developers.google.com/machine-learning/glossary#recall), or [**AUC**](https://developers.google.com/machine-learning/glossary#auc-area-under-the-roc-curve).

When deciding to improve the model, re-evaluate if the increase in resources, like engineering time and compute costs, justify the predicted improvement of the model.



# Implement a model

Most of the work in ML is **on the data side**, so getting a full pipeline running for a complex model is harder than iterating on the model itself

**Start simple**. Simple models provide a good baseline, even if you don't end up launching them.



## Train your own model versus using a pre-trained model

Pre-trained models only really work when the label and features match your dataset exactly

Commonly, ML practitioners use matching subsections of inputs from a pre-trained model for **fine-tuning or transfer learning**.



## Monitoring

### Model Deployment

get an alert that your automated deployment has failed.

### Training-serving skew

If any of the incoming features used for inference have values that fall outside the distribution range of the data used in training, you'll want to be alerted

Conversely, the serving system should alert you if the model is making predictions that are outside the distribution range that was seen during training.

### Inference server

monitor the RPC server itself and get an alert if it stops providing inferences.