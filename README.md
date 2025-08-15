Cryptocurrency Temporal & Volatility Pipeline

Project: Cryptocurrency Temporal & Volatility Crypto Data Analysis

Overview

This repository runs a pipeline that ingests hourly cryptocurrency price and volume data, computes intraday patterns, Price Spikes analysis, event impact analysis and exports plots for downstream reporting and presentation.

Data

    Source file used in this run: integrated_crypto_data.csv
    Expected columns: timestamp, coin, price, volume, high, low, open, market_cap, circulating_supply, coingecko_id
    Timestamp timezone: assumed UTC unless otherwise noted

Pipeline steps

    Load and clean data
        Parse timestamps to pandas datetime
        Sort by coin and timestamp
        Coerce numeric columns (price, volume, market_cap, circulating_supply) to numeric types
        Drop rows with critical missing values (price, timestamp, coin)

    Feature engineering
        Hour of day, day of week
        Hourly returns: return_t = price_t / price_t-1 - 1
        Log returns where relevant

    Intraday patterns / Hourly Trends (Question 1)
        Compute average daily volume to identify top 5 & bottom 5 coins
        Compute best trades for top 5 * bottom 5 coins
        Create comprehensive visualizations & plots

    Stable coins vs. Volatile price movements (Question 2)
        Compute daily price range & volatility for identification of stable & volatile coins
        Compute volatility comparison
        Compute price movement patterns
        Compute daily volatility patterns
        Compute volatility distribution

    Price Spikes Analysis (Question 3)
        Compute volume analysis
        Compute spike detection
        Compute hourly spikes
        Compute spike magnitutdes
        Compute spike direction
        Compute time categories

Outputs produced in the recent run

How to reproduce locally

    Requirements: Python 3.8+, pandas, numpy, matplotlib, seaborn
    Suggested install: pip install pandas numpy matplotlib seaborn
    Run the main pipeline script: python run_pipeline.py --input integrated_crypto_data.csv --outdir outputs

Contact

    Assistant: Nduati Githu(nduatihump@gmail.com)
