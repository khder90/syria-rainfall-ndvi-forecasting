# Long-Term Joint Forecasting of Rainfall and an NDVI-Derived Vegetation Anomaly in Syria Using Time-Varying Multivariate Systems

This repository contains the data and reproducible computational workflow for the manuscript:

**“Long-Term Joint Forecasting of Rainfall and an NDVI-Derived Vegetation Anomaly in Syria Using Time-Varying Multivariate Systems.”**

## Study target

The vegetation variable is an **NDVI-derived vegetation anomaly expressed as a percentage of the long-term normal**, where the normal condition is centred at 100.

It is not raw NDVI and it is not the conventional min–max Vegetation Condition Index.

The rainfall variable is the monthly one-month rolling rainfall indicator expressed in millimetres.

## Data

The primary sample contains 282 complete monthly observations from July 2002 through December 2025.

Two preliminary or forecast records from 2026 are retained only in the audit worksheet and are excluded from model estimation and evaluation.

The repository includes national data, regional vegetation summaries, hydrological-memory variables, source metadata, data-quality checks, and the common model-ready sample.

## Forecasting models

The controlled comparison includes:

- XGBoost
- Multi-output XceptionTime
- Bayesian time-varying-parameter VAR with smooth time-varying log-volatility, reported as BTVP-VAR-SV

All three models use the same preceding 12 monthly observations of rainfall and vegetation, corresponding to a common 12 × 2 lag information set.

The label BTVP-VAR-SV is retained for consistency with the manuscript. In this implementation, stochastic volatility denotes a smooth Bayesian time-varying marginal log-volatility process represented through cubic B-spline basis functions. It is not a conventional latent AR(1) stochastic-volatility state equation.

## Evaluation design

The analysis uses:

- Training-only scaling
- Chronological training, testing, and validation samples
- One-step holdout evaluation
- Three expanding-window rolling-origin folds
- Forecast horizons of 1, 3, 6, and 12 months
- RMSE, MAE, MASE, Theil’s U2, and forecast bias
- Continuous ranked probability score
- Negative log score
- Empirical 80% and 95% interval coverage
- Mean interval scores
- Diebold–Mariano forecast-comparison tests
- Structural-break and predictive-precedence robustness checks

All reported forecast metrics are calculated after predictions and predictive simulations are transformed back to their original units.

## Main interpretation

BTVP-VAR-SV produced the most accurate one-step vegetation forecasts and achieved the lowest CRPS in three of the four holdout-target comparisons.

XGBoost achieved the lower CRPS for validation rainfall, with a value of 3.481 compared with 4.050 for BTVP-VAR-SV, and remained competitive for rainfall point prediction.

The probabilistic ranking is interpreted only among the uncertainty constructions implemented in this study. XGBoost and XceptionTime use residual-bootstrap uncertainty, whereas BTVP-VAR-SV uses posterior predictive simulation.

The predictive-precedence p-values are interpreted as exploratory evidence because several lag lengths, directions, subsamples, and conditional specifications are examined.

The multiple-break analysis evaluates one to five piecewise-mean breaks. Because the vegetation series selects the upper search boundary, the exact number of breaks is treated as sensitivity evidence rather than a definitive structural count.

## Long-term projections

Following held-out model comparison, BTVP-VAR-SV is re-estimated using all observed data through December 2025.

The long-term forecast ensemble contains 2,000 recursive posterior predictive simulations from January 2026 through December 2030.

The simulations use the same spline-based time-varying coefficient and marginal log-volatility representation used during estimation. The spline basis is defined over the complete observed-and-forecast domain.

These forecasts provide a scenario-free statistical baseline. They are not explicit climate, conflict, land-use, or policy scenarios.

## Repository files

- `DATA.xlsx` contains the analysis data, audit information, variable dictionary, regional summaries, and source metadata.
- `Syria Rainfall NDVI Analysis.ipynb` contains the full reproducible workflow.

## Reproduction

1. Download or clone the repository.
2. Keep `DATA.xlsx` in the same directory as the notebook.
3. Open the notebook using Jupyter Notebook or JupyterLab.
4. For a new environment, set `INSTALL_EXACT_ENVIRONMENT = True`.
5. Run the environment-installation cell once.
6. Restart the kernel.
7. Return `INSTALL_EXACT_ENVIRONMENT` to `False`.
8. Run the notebook cells sequentially.

Publication mode estimates the main Bayesian model, reduced-draw Bayesian rolling-validation models, and a full-sample long-term model. Execution can require several hours on a CPU.

## Author

Khder Alakkari

Department of Statistics and Programming, Faculty of Economics, Latakia University, Latakia, Syria

Department of Financial and Banking Sciences, Faculty of Economics, Tartous University, Tartous, Syria
