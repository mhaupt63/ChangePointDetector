# ChangePointDetector
 This module takes a time series and returns:  (a) the piecewise underlaying linear trend, and (b) the times where there is a change in the underlying trend
 
 We use a Kalman Filter to find the piecewise underlying linear trend, with a state space representation of seasonality and an underlying linear trend.  We initialise the parameters of the Kalman filter using a Least Squares estimator out of "statsmodels'
 
 For the change detector on the underlying trend we use another Kalman filter, this time a single period autoregression.  We then consider the Malhalanobis distance between that filter output, using a Gumbel distribution to decide where the increase in distance likely indicates a change in trend.  This implements the approach described by Lee & Roberts at
  https://www.robots.ox.ac.uk/~sjrob/Pubs/LeeRoberts_EVT.pdf



In order to get a Kalman filter of a time series, plus change points in the trend, do the following:

1. Prepare your time series as data plus Panda dates
2. Create  the necessary Kalman representation by creating a "session" object by calling the ChangePoint class, e.g.:
	Session=ChangePointDetector.ChangePointDetector(data,dates)
3. Determine the changepoints by running the ChangePointDetectorFunction on your "session", e.g.
	Results=Session.ChangePointDetectorFunction()
4. This will return a "Results" object that contains the following:
	- ChangePoints.  This is a list of 0s and 1s the length of the data, where 1s represent changepoints
	- Prediction.  This is the Kalman smoothed actuals, plus a 3 period forecast.  Note no forecast will be made if there is a changepoint in the last 3 		dates
	- PredictionVariance
	- ExtendedDates.  These are the original dates plus 3 exta for the forecast (if a forecast has been made)
	- Trend.  This is the linear change factor
	- TrendVariance.  Variance of the trend
