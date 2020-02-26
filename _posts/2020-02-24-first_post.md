# A short comparison of an ARIMA model with a Gaussian process for time series forecasts

If we do forecasts then what we usually aim at is accuracy of the prediction. However, this is only one part of the story. You do not only want to make sure that your models are accurate but you also want to know how much you can trust the prediction.

Imagine you have a model that predicts customer requests or the demand for a service. Then, it is not only important to know the predicted amount of the request, but also the amount of uncertainty for this in order to safely plan your resources. 

It makes a difference if your model predicts 100 requests per day with a significant probability that it might also be 1000, compared to a prediction of 100 where you are almost certain that the requests will not exceed 120. In both cases your resource planning might look different. 

For this reason, it is quite interesting to have some reliable means to estimate the uncertainty of your predictions. 

The Bayesian view on statistics provides a powerful approach to assess this. In the context of regression models, this leads to the [Gaussian process models](www.gaussianprocess.org/gpml/) which in a sense provide a generalization of distributions to function space. 

As such, their predictions always consist of a full distribution with mean and confidence intervals. It is important to note that the latter actually come for free as they are an intrinsic part of the model, whereas most other models have to do special tricks in order to provide something similar, as for example [dropout for Neural Networks ensembles](https://arxiv.org/abs/1506.02142) (however, [some approaches employ the Bayesian view as well](https://eng.uber.com/neural-networks-uncertainty-estimation/)). 

So let us have a look at a quick example of how Gaussian process models can be applied and how they compare to an [ARIMA approach](https://www.digitalocean.com/community/tutorials/a-guide-to-time-series-forecasting-with-arima-in-python-3). 

### The data

As test data we take a [data set from the statsmodel library](www.statsmodels.org/stable/datasets/generated/sunspots.html) that describes the sun activity between the years 1700 and 2008.

![](/images/Data.png "The data used in this example (originally taken from [http://www.ngdc.noaa.gov/stp/solar/solarda3.html](http://www.ngdc.noaa.gov/stp/solar/solarda3.html))")

### The ARIMA model

First, we have a look at the ARIMA model. We include a seasonal part to the standard ARIMA model, since obviously the data has strong seasonal variations (the ups and downs with a more or less constant frequency). This means, that in total we have 7 free parameters for the model. 

In order to determine the best set of 7 parameters, we simply do a grid search on a limited set of possible parameter variations. Although, this is not very efficient, it is a very simple way to make sure to find a globally optimal solution.

Let us have a look at the model results below. 

![](/images/ARIMA_1_0_1__1_2_1_11__prediction.png "Results from the SARIMA model")

It turns out, that the ARIMA model does a pretty good job in forecasting. It is actually almost as good as for the known training data. 

Another thing, which becomes apparent is the fact that the validation data is lying completely within the confidence region of the prediction. Yet, the confidence region is probably too large and even exceeds into the negative value range. While the latter can easily be fixed by a meaningful transformation of the data (e.g. power transform), the generally quite large  confidence region will remain mostly unchanged by a transformation.

Let us now compare to a Gaussian process model. 

### The Gaussian process model

The behavior of the Gaussian process (GP) model is mainly defined by the so-called covariance function or kernel. The kernel describes how the behavior for different time stamps correlates with each other.  Since we have a very complex time dependent behavior, we have to create composed kernel function that captures all the different properties of the data. Therefore, we have a standard radial basis function kernel for the long-range effects, as well as two exponential sine squared kernels that allow for the periodicity in the function, and a noise term. 

The training of the model is done via multiple runs of a LBFGSB minimizer with different starting conditions. 

![](/images/GP_prediction.png "Results from the Gaussian process model")

Compared to the ARIMA model, the GP model is inferior when it comes to forecasts for unknown future data, while being superior in capturing the known behavior. In a sense, this is understandable, because in the end the GP model kind of interpolates the known data and as such is notoriously bad at extrapolating. Moreover, the model predictions (if you do not incorporate a mean function) will always tend to zero (or to the mean of the data, if the output data is normalized); you only need to be far enough from the data.  

In this respect, the obtained results for the GP model are actually quite OK. However, the forecasts are still not good enough to be able to compete with the ARIMA model, especially also when looking at the confidence regions, which do not completely enclose the validation data. 
