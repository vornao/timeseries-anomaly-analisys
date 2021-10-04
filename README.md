## Using candlestick charts to identify network anomalies in timeseries data

The aim of this project is to identify anomalies and outliers inside a time series, using candlestick charts.
The forecasting algorithm has been based on (double) exponential moving average.
Monitoring the three main candle values (open value, close value, and volume) allows us to find anomalies even in
noisy datasets.

### How does the algorithm work?

This project implements simple and double exponential smoothing functions to forecast time series values.
Each value of the candle is forecasted by the ewm function and confidence bands are created. Data are stored inside
a pandas dataframe. It is possible to tune the algorithm "sensitivity". An anomaly is detected if more or equal than
_threshold_ params are out of the forecasted confidence bands for a single candle; 
furthermore, the anomaly is confirmed if one of the actual params is out of the 95th percentile.

Tuning parameters could be specified inside the config.json file in the root directory.

At the end of the analysis, found anomalies details are printed to the terminal and plotted to a plotly web app in order
to be visualized by the user.

### Screenshots
Candlestick chart with anomalies markers:

![Candlestick chart with anomalies markers](assets/detected_anomalies.png)

Data forecasting example chart:

![Data forecasting example chart](assets/chart_example.png)