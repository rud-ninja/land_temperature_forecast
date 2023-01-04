# Title: Forecasting land temperatures from only historical temperature data (univariate) using ARIMA model

### Objectives:
1. Exploratory data analysis
2. Long term forecast (2 years) and short term forecast (6 months)
3. Comment on the difference in results for long term and short term forecasts

#### Language used:
Python

#### Libraries used:
Pandas, NumPy, Matplotlib, seasonal_decompose, acf & pacf (auto-correlation function), ARIMA

#### The dataset:
The dataset consists of recorded average temperatures over a monthly period across several major cities of the world from 1800 to 2014. Check dataset [here](https://github.com/rud-ninja/land_temperature_forecast/blob/main/GlobalLandTemperaturesByMajorCity.csv).

#### Data Imputation:
There were a few missing values for recorded average temperature. These values have been imputed using forward fill (ffill) function in pandas.



## Figures and discussions


### Data exploration

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/worldwide_temperature.png)

**Fig 1: A Look at the temperature distribution over all the major cities in the dataset**

We can see a skewed distribution leaning more towards warmer land temperatures. The vertical lines show the quartile ranges. For the remainder of the task, we have selected 3 cities lying on different latitudes for comparison of local temperature characteristics and will attempt to forecast their land temperatures for long and short periods. The cities selected are *Calcutta* that experiences tropical climate, *London* that experiences temperate climate and *Rio De Janeiro* that also experiences tropical climate but lies in the southern hemisphere.

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/citywise_temperatures.png)

**Fig 2: Temperature distribution across the cities represented as histogram and boxplot**

The histograms for Calcutta and London show a wide range of temperatures and a distinct difference in temperatures during summer and winter (evident from the two peaks) whereas the histogram for Rio De Janeiro is just like normal distribution with a single peak and a small spread (standard deviation). Along with that, we can see that Calcutta experiences more warmer temperatures than colder and London experiences almost equal amount of warm and cold climate. Rio De Janeiro remains warm almost all year round.

Speaking of climate, an interesting thing to observe will be the gradual rise in land temperature over the years. Below is a trend plot with a period of 10 years where we can see the gradual rise in temperature, which becomes more noticeable after 1920 and keeps rising. The rate of the rise appears similar for the two tropical cities while it is a bit lower for the temperate city.

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/10year_trend.png)

**Fig 3: Trend of average land temperature since 1900 over a period of 10 years**

Zooming into the above graph, we can visualise the actual change in temperature with time for the 3 cities. The seasonal nature is most consistent for Calcutta, followed by London and then Rio De Janeiro, for which there are wider variations even within the seasonal changes. This could be the reason why we saw a single peak for the temperature distribution of Rio De Janeiro. This could also mean that the temperature time series of Rio De Janeiro is stationary. In actuality, it is very close to stationary but is not. On performing Augmented Dickey Fuller test, the p-value obtained for the original series was *0.0940642* and that for the 1st difference stage was *0.0113305*

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/temperature_vs_time.png)

**Fig 4: A closer look at the temperature changes with time across the 3 cities**

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/monthly_seasonality.png)

**Fig 5: Monthly seasonality of land temperature values**




### Forecasts
The first thing to consider when using ARIMA model for time series forecasts is the stationarity of the series. We make use of autocorrelation and partial autocorrelation plots along with p value tests like Augmented Dickey Fuller test to determine whether a time series is stationary. If they are not, they have to be made stationary first before models can be trained on them.

![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/autocorrelation_london.png)

**Fig 6: Plots of autocorrelation and partial aurocorrelation functions that demonstrate correlation between past and future values in the time series as a function of the time gap (or lag) between them. The above plot is for the city of London. The same has been performed for the remaining cities (not shown here)**

The above figure was followed by an AD fuller test whose resutls were as follows:

*ADF Statistic for a decade for city of London: -1.2288251956924272
p-value: 0.661074580011341
Critical Values:
	1%: -3.486055829282407
	5%: -2.8859430324074076
	10%: -2.5797850694444446*
  
The p-value is greater than 0.05 which is our considered significance level. This indicates that the series is not stationary. To make it stationary, we perform first differenciation, which essentially means calculating the difference between subsequent temperature values. The results are as follows.

*ADF Statistic of 1st difference: -9.27663324265689
p-value: 1.2837472335018562e-15
Critical Values:
	1%: -3.486055829282407
	5%: -2.8859430324074076
	10%: -2.5797850694444446*
  
Now the p-value is less than 0.05 which means the series is now stationary and the ARIMA model can be trained on it.



## Long term forecasts
For the forecasts, we have sliced the chunk of data for the decade starting at the year 2000. We will compare the efficacy of ARIMA model to forecast based on univariate data for long and short durations. For long term forecast we have chosen the last 2 years of our data chunk as the test set. The test set for short term forecasts are the last year of the decade and the remaining for training.

City: Calcutta,		MSE: 1.53
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/lt_cal.png)

City: London,		MSE: 2.56
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/lt_lon.png)

City: Rio De Janeiro,		MSE: 1.27
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/lt_rdj.png)

**Fig 7: Long term land temperature forecast for the 3 cities. The plots on the left display the training and test size while the plots on the right display the actual vs predicted values and the 95% confidence interval for the same**



## Short term forecasts

City: Calcutta,		MSE: 1.32
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/st_cal.png)

City: London,		MSE: 3.99
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/st_lon.png)

City: Rio De Janeiro,		MSE: 1.46
![](https://github.com/rud-ninja/land_temperature_forecast/blob/main/new_images/st_rdj.png)

**Fig 8: Short term temperature forecast**
