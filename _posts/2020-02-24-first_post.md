# How to deal with uncertainty in timeseries forecasts

If we do forecasts then what we usually aim at is accuracy of the prediction. However, this is only one part of the story. You do not only want to make sure that your models are accurate but you also want to know how much you can trust the prediction.

Imagine you have a model that predicts customer requests or the demand for a service. Then, it is not only important to know the predicted amount of the request, but also the amount of uncertainty for this in order to safely plan your resources. 

It makes a difference if your model predicts 100 requests per day with a significant probability that it might also be 1000, compared to a prediction of 100 where you are almost certain that the requests will not exceed 120. In both cases your resource planning might look different. 

For this reason, it is quite interesting to have some reliable means to estimate the uncertainty of your predictions. 

The Bayesian view on statistics provides a powerful approach to assess this. 

Wheather forecasts long deal with the fact that their predictions are subject to uncertainty. What they do is that they use [model ensembles](https://www.ecmwf.int), that means not a single model, but a set of models with only slight modifications. These so-called perturbations can be incorporated either [in the starting conditions or in the integration steps of the model](https://www.ecmwf.int/en/research/modelling-and-prediction/quantifying-forecast-uncertainty). 

Basically, you can do the same with your data-driven machine learning model. Neural networks are one example which are often used in model ensembles. Here, the perturbation is created by having slightly different conditions or hyperparameters for the model training. For example using

