Name : Vamshi Krishna Emmadi
ENDOVIA QR TASK


Mail : vamshikgpian@gmail.com

Results:

The final equity is 589080
So, Profit=Final Equity−Initial Capital=589,080−200,000=₹389,080


Metrics CSV:
refer to metrics_csv
Total return in the above picture is the profit percent, so the total equity is 200000*(1+total_return), ( please refer  the code in case )

Equity curve:

refer to equity curve.png

Drawdown:

refer to drawdown.png




Option Strategy Quantitative Backtest
Implementation of a quant research framework for calendar-year 2023 options backtesting using spot signals and machine learning. The workflow covers data prep, technical indicators, composite signals, option selection logic, ML modeling, and a full backtesting/metrics pipeline.

Environment Setup & Dependencies:
Requirements:

Python 3.8+
Packages:
pandas
numpy
matplotlib
scikit-learn
xgboost
Tqdm





Indicators, Model, & Backtest Parameters:
Indicators
MACD: Measures momentum/trend via EMA differences

RSI: Measures overbought/oversold levels

Composite Signal: Weighted vote: 'in-house' signal, MACD, RSI (default weights: 0.4/0.3/0.3)

Model
XGBoost Classifier (tree_method='gpu_hist' 0 — i used for GPU)

Features: price + signal/indicators

Target: Next step up/down

Backtest Parameters
Initial capital: ₹200,000

Option entry: Sell ATM PUT on buy signal, sell ATM CALL on sell signal

Nearest expiry only
Stop-Loss: 1.5% on option MTM loss
Take-Profit: 3% on option MTM gain
Forced exit: 15:15 IST
Ignoring margin/slippage


My Approach:

The method used a weighted composite of both in-house and technical signals (MACD, RSI) to make options selling more accurate in terms of direction. Machine learning (XGBoost) made trade triggers even better by using past spot and signal data to guess the short-term direction of the market. 

I did backtesting with realistic execution, utilizing stop-loss, take-profit, and daily forced exit, with minute-level spot and options data. The last system put a lot of emphasis on good risk management and realistic trading logistics.

Important points:

The composite signal did much better than any one indicator, cutting down on whipsaw transactions and raising net profits.

XGBoost categorization improved correctness by a small but noticeable amount compared to only using static rules.

To get good returns while keeping drawdowns low, it was important to use conservative position sizing, dynamic exits, and rigorous stop-loss/take-profit rules.

Iterative testing with filtered data windows was very important for quickly making prototypes and checking them before doing full-scale backtests. This made sure that the code was both efficient and reliable.

The overall approach made a good profit(almost triple the beginning capital) and had strong risk metrics. 


Good Signs from results::
Upward, steady equity growth
Low, recoverable drawdowns
Positive Sharpe ratio
Realistic trade logs (not overfitting)


strategy-backtest/
├── data/
├── results/
├── indicators.py
├── signal_engine.py
├── model.py
├── backtest.py
├── utils.py
├── requirements.txt
├── README.md




THANK YOU FOR YOUR TIME!

