# Kronos Architecture Reference

Source: `shiyu-coder/Kronos` — financial foundation model for K-line forecasting.

## Core Components
- `KronosTokenizer`: hierarchical discrete tokenizer for OHLCV
- `KronosPredictor`: autoregressive Transformer predictor
- `predict()`: single-series forecast
- `predict_batch()`: GPU-parallel multi-series forecast

## IO Contract
```
Input: DataFrame[open, high, low, close, volume, amount] + timestamps
Output: DataFrame[open, high, low, close, volume, amount] forecast
```

## Constraints
- `max_context = 512` for small/base models
- GPU recommended for batch; CPU falls back to sequential
- Normalization range learned from training data; must match inference distribution

## Why This Pattern For Sam
Local-first, no API keys, batch-ready, deterministic IO contract.
