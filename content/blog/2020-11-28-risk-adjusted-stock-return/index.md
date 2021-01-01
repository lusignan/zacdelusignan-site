---
title: Risk-adjusted Stock Return
author: Zac de Lusignan
date: '2020-11-28'
slug: []
categories: ['Finance']
tags: ['Python']
description: ''
featured: ''
featuredalt: ''
featuredpath: ''
linktitle: ''
type: post
---

## Introduction

The Sharpe Ratio is a way of evaluating the value of an investment after subtracting for its risk. In this project we'll use it to compare four stocks: Alphabet, Amazon, Apple, and Tesla. We'll be using the S&P 500 to measure the overall volatility of the market.

## Getting started

To start, we'll load in the data which was pulled from Yahoo Finance. To account for the recent recession the data begins with the start of the current business cycle in early March.

 ````
#Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

#Read in data
stock_data = pd.read_csv('https://raw.githubusercontent.com/lusignan/Sharpe-Ratio/main/stock_data.csv', 
    parse_dates=['Date'],
    index_col='Date'
    ).dropna()
benchmark_data = pd.read_csv('https://raw.githubusercontent.com/lusignan/Sharpe-Ratio/main/stock_data.csv', 
    parse_dates=['Date'],
    index_col='Date'
    ).dropna()
````


````
#Visualize stock data
stock_data.plot(title='Stock Data', subplots=True, linewidth = 1.5)
````
![stockdataaaat2020](/img/main/stockdataaaat2020.png)
````
#Visualize S&P 500 data
benchmark_data.plot(linewidth = 1.5)
````
![sp5002020](/img/main/sp5002020.png)
````
#Calculate daily stock returns
stock_returns = stock_data.pct_change()

#Plot daily returns
stock_returns.plot(linewidth = 1.5)
````
![stockreturnsaaat](/img/main/stockreturnsaaat.png)
````
#Calculate daily S&P 500 returns
sp_returns = benchmark_data['S&P 500'].pct_change()

#Plot the daily returns
sp_returns.plot(linewidth = 1.5)
````
![spreturns](/img/main/spreturns.png)
````
#Calculate the difference in daily returns
excess_returns = stock_returns.sub(sp_returns, axis=0)

#Plot the excess_returns
excess_returns.plot(linewidth = 1.5)
````
![excessreturns](/img/main/excessreturns.png)
````
#Calculate the mean of excess_returns 
avg_excess_return = excess_returns.mean()

#Plot the average excess returns
avg_excess_return.plot.bar(title='Mean of the Return Difference');
````
![meanreturndiff](/img/main/meanreturndiff.png)

````
#Calculate the standard deviations
sd_excess_return = excess_returns.std()

#Plot the standard deviations
sd_excess_return.plot.bar(title='Standard Deviation of the Return Difference');
````
![stddvreturndiff](/img/main/stddvreturndiff.png)

````
#Calculate the daily sharpe ratio
daily_sharpe_ratio = avg_excess_return.div(sd_excess_return)

#Annualize the sharpe ratio
annual_factor = np.sqrt(252)
annual_sharpe_ratio = daily_sharpe_ratio.mul(annual_factor)

#Plot the annualized sharpe ratio 
annual_sharpe_ratio.plot.bar(title='Annualized Sharpe Ratio: Stocks vs S&P 500');
````
![stocksvssp500](/img/main/stocksvssp500.png)

## Conclusion

After accounting for market volatility, Tesla is the best of the four stock options to buy followed by Apple.