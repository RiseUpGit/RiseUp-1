---
name: sam-crypto-forecaster
description: >
  Local OHLCV → discrete tokens → forecast → trade-signal pipeline for Sam.
  Pattern derived from shiyu-coder/Kronos tokenizer/predictor architecture.
  Use when building crypto/trading workflows that need local-first ML inference.
---

# Sam Crypto Forecaster

## Context
Kronos (`shiyu-coder/Kronos`) proved the tokenizer→predictor pattern for financial
time series. This skill adapts that architecture for crypto OHLCV data on Sam.

## What We Borrow From Kronos
- Hierarchical discrete tokenizer for continuous OHLCV data
- Autoregressive Transformer predictor on token sequences
- Normalize → tokenize → predict → inverse-normalize pipeline
- `predict_batch()` pattern for parallel multi-asset predictions
- 512-token context baseline; extend for crypto tick data

## What We Do NOT Import
- Trained stock/ETF weights (A-share bias)
- Qlib dependency (overkill for raw crypto)
- Stock-specific backtesting without slippage/fees model

## Skill Trigger
- User asks for crypto forecasting, trading signals, or market analysis
- Sam needs local inference on OHLCV data without cloud APIs

## Minimal Build Steps
1. Collect OHLCV data (Binance/CCXT/local CSV)
2. Normalize per asset: z-score or min-max across lookback window
3. Tokenize with hierarchical discretizer (open/high/low/close/volume bands)
4. Predict next N tokens → inverse normalize → OHLCV forecast
5. Translate forecast into signal: trend direction, volatility bands, support/resistance

## Output Format
- Forecast DataFrame with predicted OHLCV for next `pred_len` periods
- Signal side: long / short / flat with confidence band
- Batch output: one prediction per monitored pair

## Future Additions
- Live WebSocket feed from exchange
- Position sizing + risk model
- Backtest harness with fee/slippage assumptions
