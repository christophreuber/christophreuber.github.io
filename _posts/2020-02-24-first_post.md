# How to deal with uncertainty in timeseries forecasts

Whether forecasts long deal with the fact that their predictions are subject to uncertainty. What they do is that they use [model ensembles](https://www.ecmwf.int), that means not a single model, but a set of models with only slight modifications. These so-called perturbations can be incorporated either [in the starting conditions or in the integration steps of the model](https://www.ecmwf.int/en/research/modelling-and-prediction/quantifying-forecast-uncertainty). 

Basically, you can do the same with your data-driven machine learning model. Neural networks are one example which are often used in model ensembles. Here, the perturbation is created by having slightly different conditions or hyperparameters for the model training. For example using

