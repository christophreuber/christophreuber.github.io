# How to deal with uncertainty in timeseries forecasts

If we do forecasts then what we usually aim at is accuracy of the prediction. However, this is only one part of the story. You do not only want to make sure that your models are accurate but you also want to know how much you can trust the prediction.

Imagine you have a model that predicts customer requests or the demand for a service. Then, it is not only important to know the predicted amount of the request, but also the amount of uncertainty for this in order to safely plan your resources. 

It makes a difference if your model predicts 100 requests per day with a significant probability that it might also be 1000, compared to a prediction of 100 where you are almost certain that the requests will not exceed 120. In both cases your resource planning might look different. 

For this reason, it is quite interesting to have some reliable means to estimate the uncertainty of your predictions. 

The Bayesian view on statistics provides a powerful approach to assess this. In the context of regression models, this leads to the [Gaussian process models](www.gaussianprocess.org/gpml/) which in a sense provide a generalization of distributions to function space. 

As such, their predictions always consist of a full distribution with mean and confidence intervals. It is important to note that the latter actually come for free as they are an intrinsic part of the model, whereas most other models have to do special tricks in order to provide something similar, as for example [dropout for Neural Networks ensembles](https://arxiv.org/abs/1506.02142) (however, [some approaches employ the Bayesian view as well](https://eng.uber.com/neural-networks-uncertainty-estimation/)). 

So let us have a look at a quick example of how Gaussian process models can be applied and how they compare to an [ARIMA approach](https://www.digitalocean.com/community/tutorials/a-guide-to-time-series-forecasting-with-arima-in-python-3). 

As test data we take a [data set from the statsmodel library](www.statsmodels.org/stable/datasets/generated/sunspots.html) that describes the sun activity between the years 1700 and 2008.


