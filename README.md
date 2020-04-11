# trade-stat-logger
Want to be able to easily log trades of any type of security and measure performance and analyze several factors, such as risk or return distribution? Then check out trade-stat-logger.
## features (SimpleLogger)
- log trades: long and short
- get a statistical summary: 
  - net profit: self-explanatory
  - drawdown: vertical distance between peak and trough of net profits
  - volatility of returns: measures deviation of returns distribution
  - kurtosis: measures tails of returns distribution
  - probability of winning on a trade: self-explanatory
  - Kelly Criterion: estimate of optimal portfolio allocation
  - And more
- visualize performance:
  - visualize distribution of returns
  - analyze movement of gains or losses after every trade or over a period of time
## install
```
pip install trade-stat-logger
```
## Classes and methods
### SimpleLogger Methods:
```
SimpleLogger(datetime_support=False)
```
Constructs a new SimpleLogger object. Set datetime_support=True if you want to log the time at which the trade executed. Ignore future dt key word arguments if you wish to not support datetime logging.
***
```
log(security, shares, share_price, dt=None)
```
Log trades with this method. Shares is the number of shares you wish to purchase, and share_price is the price of a share. If dt is left as None, it will log current time, else set dt to a datetime object to log a custom time.
***
```
log_cp(security, share_price, dt=None)
```
Clear a position of given security at given share_price and log it. If dt is left as None, it will log current time, else set dt to a datetime object to log a custom time.
***
```
get_positions()
```
Returns a dict of current security holdings in form of {security: {'shares': # shares, 'avg_share_price': average share price1}, another_security: {'shares': # shares, 'avg_share_price': average share price1}, ...}, where the key corresponding to a security is the string passed through the log() method.
***
```
clear_all_positions(get_price_func, closure_date)
```
Clears all security holdings, which is advised before calling methods that measure performance of trades. get_price_func should be a function such that get_price_func(security, closure_date) == price of security. For example:
```
logger = SimpleLogger()
...log trades...
def get_price(security, date):
    return api.get_price(security, date)
logger.clear_all_positions(get_price, some_date)
```
However, if you logged the tickers of stocks, you can use an easier alternative: `clear_stock_positions(closure_date)` will clear all stock positions
***
```
get_summary_statistics()
```
Returns a dictionary of various statistics in the form of {statistic1_name: value, statistics2_name: value,...}.
```
graph_statistics(time_axis=False, time_strformat='%m/%d/%Y', show_window=True)
```
Graphs the distribution of trade returns, net profit at the nth trade, and puts the summary statistics in a table. If you would like to have the x-axis for the net profit graph be time, set time_axis=True, and optionally add a custom time format through time_strformat='%Y/%o/%u/%r format'
## Future features
- more time support for SimpleLogger, including time analysis on returns
- I will add a ComplexLogger, which will be built upon SimpleLogger, and it will support initial portfolio size, ROI, and many more.
- KellyLogger, which will dynamically return Kelly Criterion scores based on trade logs
