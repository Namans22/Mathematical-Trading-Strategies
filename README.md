# Mathematical-Trading-Strategies
Official repo for submission of assignments in Mathematical Trading Strategies
import yfinance as yf
import pandas as pd
import numpy as np

# International Indices
indices = ['^RUT', '^GSPC', '^BSESN', '^NSEI', '^DJI']

#International Equities
equities = ['DIS', 'NFLX', 'V', 'WMT', 'TSLA']

# Combine indices and equities into one list
symbols = indices + equities

# Download historical data since "2010-01-01"
data = yf.download(symbols, start='2010-01-01')['Adj Close']

# Calculate Daily Returns
returns = data.pct_change()
last_daily_returns = returns.iloc[-1]

# Calculate Cumulative Returns
cumulative_returns = (1 + returns).cumprod()
final_cumulative_returns = cumulative_returns.iloc[-1]

# Calculate Max Drawdowns
rolling_max = cumulative_returns.cummax()
drawdowns = (cumulative_returns / rolling_max) - 1
max_drawdowns = drawdowns.min()

# Calculate Volatility
volatility = returns.std()

# Calculate Daily Average Returns and Risk-free Rate (assumed as 0 for simplicity)
daily_avg_returns = returns.mean()
risk_free_rate = 0.0

# Calculate Sharpe Ratio
sharpe_ratio = (daily_avg_returns - risk_free_rate) / volatility

# Calculate Sortino Ratio
downside_returns = returns[returns < 0]
downside_risk = downside_returns.std()
sortino_ratio = (daily_avg_returns - risk_free_rate) / downside_risk

# Print the results
print("Daily Returns:\n", last_daily_returns)
print("\nCumulative Returns:\n", final_cumulative_returns)
print("\nMax Drawdowns:\n", max_drawdowns)
print("\nVolatility:\n", volatility)
print("\nSharpe Ratio:\n", sharpe_ratio)
print("\nSortino Ratio:\n", sortino_ratio)
