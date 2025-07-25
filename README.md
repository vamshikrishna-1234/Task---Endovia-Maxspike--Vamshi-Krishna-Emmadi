# Task---Endovia-Maxspike--Vamshi-Krishna-Emmadi

Name : Vamshi Krishna Emmadi

Mail : vamshikgpian@gmail.com


Option Strategy Quantitative Backtest
This repository implements a modular research framework for calendar-year 2023 options backtesting using spot signals and machine learning. The workflow covers data prep, technical indicators, composite signals, option selection logic, ML modeling, and a full backtesting/metrics pipeline.

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


Install all dependencies:

bash
pip install pandas numpy matplotlib scikit-learn xgboost tqdm
Optional: For indicator shortcuts, use ta, if not using custom code:

bash
pip install ta
Script Usage Examples
Typical workflow in notebook or Python script:

python
# 1. Load your data (adjust paths as needed)
spot_df = pd.read_csv('data/spot_with_signals_2023.csv', parse_dates=['datetime'])
options_df = pd.read_parquet('data/options_data_2023.parquet')

# 2. (Optional) Reduce data for prototyping
# Example: Use only two months for fast testing
options_df['datetime'] = pd.to_datetime(options_df['datetime'])
options_df = options_df[options_df['datetime'].between('2023-01-01','2023-02-28')].copy()

# 3. Build indicators
spot_df = macd(spot_df)
spot_df['rsi'] = rsi(spot_df['close'])

# 4. Generate signals
spot_df['macd_signal'] = ((spot_df['macd'] > spot_df['macd_signal']).astype(int)
                          - (spot_df['macd'] < spot_df['macd_signal']).astype(int))
spot_df['rsi_signal'] = spot_df['rsi'].apply(lambda x: 1 if x < 30 else (-1 if x > 70 else 0))

# 5. Composite signal
weights = {'signal':0.4, 'macd':0.3, 'rsi':0.3}
spot_df['composite_signal'] = spot_df.apply(lambda row: composite_signal(row, weights, 0.5), axis=1)

# 6. Train ML model
spot_df['signal_numeric'] = spot_df['signal'].map({'BUY': 1, 'SELL': -1}).fillna(0).astype(int)
feature_cols = ['macd', 'rsi', 'close', 'signal_numeric']
model, _, _, _, _ = train_spot_model(spot_df.dropna(), feature_cols, 'target')

# 7. Run backtest
equity_df, trades_df, metrics_df = run_backtest(spot_df, options_df)

# 8. Save results
equity_df.to_csv('results/equity_df.csv', index=False)
trades_df.to_csv('results/trades.csv', index=False)
metrics_df.to_csv('results/metrics.csv', index=False)
plot_equity_curve(equity_df['equity'], 'results/equity_curve.png')
plot_drawdown(equity_df['equity'], 'results/drawdown.png')




Indicators, Model, & Backtest Parameters:
Indicators
MACD: Measures momentum/trend via EMA differences

RSI: Measures overbought/oversold levels

Composite Signal: Weighted vote: 'in-house' signal, MACD, RSI (default weights: 0.4/0.3/0.3)

Model
XGBoost Classifier (tree_method='gpu_hist' optional for GPU)

Features: price + signal/indicators

Target: Next step up/down

Backtest Parameters
Initial capital: ₹200,000

Option entry: Sell ATM PUT on buy signal, sell ATM CALL on sell signal

Nearest expiry only

One position at a time

Stop-Loss: 1.5% on option MTM loss

Take-Profit: 3% on option MTM gain

Forced exit: 15:15 IST

Ignore margin/slippage

 Results:



equity_curve.png: Your simulated portfolio value over time

drawdown.png: The worst-to-date loss at each point (downside risk)

metrics.csv: Key performance metrics (Sharpe, max drawdown, total return)

trades.csv: All trade details including entries, exits, and P&L

Good Signs from results::
Upward, steady equity growth

Low, recoverable drawdowns

Positive Sharpe ratio

Realistic trade logs (not overfitting)


Documentation: This README.md file (setup, usage, strategy, interpretation)

Tree:


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

