
# Cryptocurrency Temporal & Volatility Pipeline

Project: Cryptocurrency Temporal & Volatility Crypto Data Analysis


Overview

This repository runs a pipeline that ingests hourly cryptocurrency price and volume data, computes intraday patterns, rolling volatility, event impact analysis, volume-volatility relationships, and drawdowns, and exports plots and CSVs for downstream reporting and presentation.

Data

- Source file used in this run: integrated_crypto_data.csv
- Expected columns: timestamp, coin, price, volume, high, low, open, market_cap, circulating_supply, coingecko_id
- Timestamp timezone: assumed UTC unless otherwise noted

Pipeline steps

1. Load and clean data
   - Parse timestamps to pandas datetime
   - Sort by coin and timestamp
   - Coerce numeric columns (price, volume, market_cap, circulating_supply) to numeric types
   - Drop rows with critical missing values (price, timestamp, coin)

2. Feature engineering
   - Hour of day, day of week
   - Hourly returns: return_t = price_t / price_t-1 - 1
   - Log returns where relevant

3. Intraday patterns (Question 1)
   - Aggregate average hourly returns and average volume by hour-of-day for each coin
   - Save per-coin plots and combined visualizations to outputs/plots/01_intraday_*.png
   - Save summary CSVs to outputs/csvs/intraday_summary_*.csv

4. Event impact analysis (Question 2)
   - If an event_time column is present, compute price/volume changes before and after events, and plot event-aligned windows
   - If event_time is missing, step is skipped with a warning. Outputs are saved under outputs/plots/02_event_impact_* if available

5. Rolling volatility (Question 3)
   - Compute rolling standard deviation of hourly returns over a 24-hour window (or configurable window)
   - Save CSV: outputs/csvs/rolling_volatility_hourly_24h.csv
   - Save plots per coin under outputs/plots/03_rolling_volatility_*

6. Volume-volatility relationship (Question 4)
   - Compute correlation between volume and absolute returns by coin and by hour buckets
   - Fit simple linear models where appropriate and save results to outputs/csvs/volume_volatility_relationship.csv
   - Save companion plots to outputs/plots/04_volume_volatility_*

7. Drawdowns (Question 5)
   - Compute peak-to-trough drawdowns per coin, duration, and recovery times
   - Save CSV: outputs/csvs/drawdowns_summary.csv and plots under outputs/plots/05_drawdowns_*

Errors and troubleshooting

- During the last run a KeyError occurred in the rolling volatility step when selecting columns for the final output. This usually indicates a rename/merge mismatch. To fix:
  - Inspect intermediate dataframe columns (df.columns)
  - Ensure the rolling series is renamed to rolling_vol and merged properly on coin + timestamp
  - Use reset_index() carefully to preserve timestamp as a column

- If event impact is skipped, check for an event_time column or provide an events CSV with columns: coin, event_time, event_type, description

Outputs produced in the recent run

- Many plots and CSVs were produced under outputs/plots/ and outputs/csvs/. Example paths created by the pipeline (these are representative and were generated during the run):
  - outputs/plots/01_intraday_BTC.png
  - outputs/plots/03_rolling_volatility_BTC.png
  - outputs/csvs/intraday_summary_top_coins.csv
  - outputs/csvs/drawdowns_summary.csv

Note: The pipeline logged a KeyError in the rolling volatility step which prevented creation of the rolling_volatility_hourly_24h.csv in that run. Choose to re-run the pipeline with a fix to regenerate the missing files.

How to reproduce locally

- Requirements: Python 3.8+, pandas, numpy, matplotlib, seaborn
- Suggested install: pip install pandas numpy matplotlib seaborn
- Run the main pipeline script: python run_pipeline.py --input integrated_crypto_data.csv --outdir outputs





Contact

- Assistant: Nduati Githu(nduatihump@gmail.com)
- For support: team@julius.ai

