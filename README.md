## Using candlestick charts to identify network anomalies in time series data

This simple script analyzes nomalies and outliers inside a time series, using candlestick charts.
The forecasting algorithm has been based on (double) exponential moving average.
Monitoring candlestick values (open, close, and volume) allows us to find anomalies even in
noisy datasets.


### How are candles created and what do open, close and volume values mean in network monitoring?

In order to create candles, data are fetched from a provided RRD archive; then the resulting time series is 
split into temporal windows, each one representing a candle (window size can be set inside the config.json).
The bigger the window, the thicker the candle. For example, if RRD step is ```300s``` (5min) and candle thickness is set
to ```3```, each candle will represent 15 minutes of data; so, for each candle we obtain that:

- ```open``` value is the first point of the selected window;
- ```close``` value is window's last point;
- ```max (min)``` is the maximum (minimum) value registered.
- ```volume = abs(close-open)``` and represents candle's body (or height)

Using candlestick charts instead of _raw_ time series let us make better predictions in noisy data.

### How does the algorithm work?

To forecast anomalies, simple and double exponential smoothing functions have been implemented.
Each value of the candle is forecasted by an ewm function and confidence bands are created. Data are stored inside
a pandas dataframe. It is possible to tune algorithm sensitivity inside ```config.json``` file. An anomaly is detected if more than or equal
```threshold``` params are out of forecasted confidence bands for a single candle; 
furthermore, anomaly is confirmed if one of the actual params is out of the 95th percentile 
(you may have to set ```USE-QUANTILE``` flag to true in the configuration file)

At the end of the analysis, found anomalies details are printed and plotted to a plotly web app in order
to be visualized by the user.

### Screenshots
Candlestick chart with anomalies markers:

![Candlestick chart with anomalies markers](assets/detected_anomalies.png)

Data forecasting example chart:

![Data forecasting example chart](assets/chart_example.png)

### Installation instructions

This project requires python 3.9 installed on your local machine and the package virtualenv.
Once you have installed python, open a terminal (on Linux/MacOS) window and type the following commands
in order to install dependencies.

```bash
    $ virtualenv timeseries-anomaly-venv
    $ source timeseries-anomaly-venv/bin/activate
    $ pip3 install -r requirements.txt  
```

Now, you can edit the ```config.json``` with all the parameters you need for your dataset.

To run the script type:
```bash
    $ python3 main.py
```

