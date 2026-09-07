# Pelabuhan Ratu Wave Height Forecasting

This notebook compares Autoformer and Transformer architectures for significant wave-height (`Hs`) forecasting in a Pelabuhan Ratu case study. The loaded tab-separated file contains 58,657 timestamped records from 2014-01-01 00:00 to 2024-01-15 00:00, with `Hs`, peak period (`Tp`), and direction (`Dir`) among the recorded fields. The model input is reduced to `Hs`.

The notebook experiments with several historical training spans and forecast horizons, including 24, 48, and 96 hours, and also contains 14-day forecasting sections. It implements Autoformer components directly in PyTorch and separately implements Transformer experiments.

Autoformer was introduced by Wu et al. (2021) as a forecasting architecture built around progressive series decomposition and an Auto-Correlation mechanism. The notebook uses those ideas, but this repository should not claim equivalence to the authors' reference implementation without a code-level comparison. The original paper is available from NeurIPS: https://proceedings.neurips.cc/paper/2021/hash/bcc0d400288793e8bdcd7c19a8ac0c2b-Abstract.html

## Data handling

The notebook reads:

```text
TOTAL_WAVE_PR-C1_HsTpDir.txt
```

and assigns these columns:

```text
Year, Month, Day, Hour, Minute, Second, Hs, Tp, Dir
```

Rows with missing values are dropped. `MinMaxScaler` is then fitted to the complete `Hs` series before later train/validation/test procedures appear in the notebook.

That ordering is a methodological problem. A scaler fitted before a temporal split can use future minimum and maximum values when transforming earlier observations. The recorded model metrics therefore come from a pipeline with potential temporal leakage.

The notebook also contains an absolute local path:

```text
/Users/mac/Desktop/.../TOTAL_WAVE_PR-C1_HsTpDir.txt
```

This should be replaced before publication because it exposes a private filesystem layout and prevents another user from running the notebook without editing the path.

## Experimental structure

The notebook includes:

- Autoformer experiments;
- Transformer experiments;
- multiple training-history lengths;
- 24-, 48-, and 96-hour settings;
- hyperparameter search code;
- early stopping;
- checkpoint loading and saving;
- RMSE and MAPE reporting;
- forecast visualizations.

The saved outputs contain several "Best" metrics for different experiment blocks. They should not be collapsed into one project-wide performance number because they refer to different configurations and forecasting setups.

The notebook also imports and uses several modeling utilities beyond Autoformer and Transformer, including XGBoost in later code. That means the file is broader than its top markdown title suggests.

## What can be claimed

The notebook supports the statement that Autoformer- and Transformer-based forecasting experiments were run on a univariate `Hs` series derived from 58,657 timestamped records.

It does not establish:

- that either architecture is generally superior;
- that the reported metrics are unbiased estimates of future performance;
- that the implementation exactly reproduces the Autoformer paper;
- that the model is suitable for operational maritime forecasting.

The absence of an untouched final evaluation design and the scaler ordering limit the strength of performance claims.

## Running the notebook

Install the dependencies:

```bash
pip install -r requirements.txt
```

Update the dataset path in the notebook, then run the cells in order.

## References

- Wu, H., Xu, J., Wang, J., & Long, M. (2021). *Autoformer: Decomposition Transformers with Auto-Correlation for Long-Term Series Forecasting*. NeurIPS 34. https://proceedings.neurips.cc/paper/2021/hash/bcc0d400288793e8bdcd7c19a8ac0c2b-Abstract.html
