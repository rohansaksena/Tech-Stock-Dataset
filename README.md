# Stock Analysis For Tech Companies
This is a very Basic Data analysis project focusing on dealing with retrieval of stock data from net and then performing the exploratory data analysis of stock prices for certain popular tech. companies. We will get the data using pandas datareader and will get the stock information for the followings Amazon, Microsoft, Google and Apple from Jan 1st 2011 to Jan 1st 2021.

## Data Source 
We will gather our data directly from Yahoo finance using pandas datareader! We will get the stock data from Jan 1st 2011 to Jan 1st 2021 for each of the tech companies mentioned above.

## Project Outcomes:

### *The Imports*
```
import pandas as pd
from pandas_datareader import data, wb
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import datetime as dt
import cufflinks as cf
```

*EDA*
Let's explore the data a bit!*

### Set the Datetime Parameters
```
start = dt.datetime(2010,1,1)
end = dt.datetime(2020,12,31)
```

### Fetch the stock data for each of the tech. companies
```
tickers = ['AAPL','GOOG','MSFT','AMZN']
for tick in tickers:
    globals()[tick] = data.DataReader(tick, 'yahoo', start, end) #Method 1
    
AAPL = data.DataReader('AAPL', 'yahoo', start, end)
GOOG = data.DataReader('GOOG', 'yahoo', start, end)
MSFT = data.DataReader('MSFT', 'yahoo', start, end)
AMZN = data.DataReader('AMZN', 'yahoo', start, end) #Method 2
```

### Concatenate the stock dataframes together into a single data frame called tech_stocks
```
tech_stocks = pd.concat([AAPL,GOOG,MSFT,AMZN],keys=tickers,axis=1)
```

### Set the Column Names
```
tech_stocks.columns.names=['Tech Com','Stock Price']
tech_stocks.head(2)
```

### What is the max close price for each tech stock throughout the time period ?
```
tech_stocks.xs(key='Close',axis=1,level='Stock Price').max()
Tech Com
AAPL     136.690002
GOOG    1827.989990
MSFT     231.649994
AMZN    3531.449951
dtype: float64
```

### Create a new DataFrame to store the returns
```
returns = pd.DataFrame()
```

### Calculate the returns for each of the stocks and store them in the above created DataFrame
```
for tick in tickers :
    returns[tick + " Returns"] = tech_stocks[tick]['Close'].pct_change()
returns.head()
```

### Using returns DataFrame figure out the date when banks had there worst and best stocks
```
returns.idxmax()
returns.idxmin()
```

### Look at the standard deviation of the returns
```
returns.std()
```

### Plot the closing trend for each of the tech stock
```
tech_stocks.xs(level='Stock Price',axis=1,key='Adj Close').plot(figsize=(15,5))
```

### Plot the rolling 30 day average against the Close Price for Amazon's stock for the year 2016
```
AMZN['Close']['2016-01-01':'2016-12-31'].rolling(window=30).mean().plot()
```

### Create a heatmap of the correlation between the stocks Close Price.
```
sns.heatmap(tech_stocks.xs(level='Stock Price',key='Close',axis=1).corr(),annot=True)
```

### Use seaborn's clustermap to cluster the correlations together:
```
sns.clustermap(tech_stocks.xs(level='Stock Price',axis=1,key='Close').corr(),annot=True)
```

### Create a column daily return and plot a pct_change graph for Amazon in the year 2010
```
AMZN['Daily Return'] = AMZN['Adj Close'].pct_change()
AMZN['Daily Return'].loc['2010-01-01':'2010-12-31'].plot(figsize=(15,4),legend=True,linestyle='--',marker='o')
```

## Project Setup:
To clone this repository you need to have Python compiler installed on your system alongside pandas and seaborn libraries. I would rather suggest that you download jupyter notebook if you've not already.

To access all of the files I recommend you fork this repo and then clone it locally. Instructions on how to do this can be found here: https://help.github.com/en/github/getting-started-with-github/fork-a-repo

The other option is to click the green "clone or download" button and then click "Download ZIP". You then should extract all of the files to the location you want to edit your code.

Installing Jupyter Notebook: https://jupyter.readthedocs.io/en/latest/install.html <br>
Installing Pandas library: https://pandas.pydata.org/pandas-docs/stable/install.html
