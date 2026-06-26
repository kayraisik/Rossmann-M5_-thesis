# When Do Tree-Based Methods Win? — Rossmann Store Sales

Code and reproducibility materials for the master's thesis *"When Do Tree-Based
Methods Win? Dataset Characteristics as Determinants of Ensemble Performance"*
(Kayra Işık).

The thesis asks **which measurable properties of retail demand data explain when
tree-based ensemble methods (XGBoost, LightGBM) outperform simple and statistical
baselines**, using the Rossmann Store Sales dataset (1115 stores, 2013–2015). A
decomposition-based framework links the gain over a store-mean forecast to the
predictable within-store variation, the feature informativeness, and the training
data; the experiments vary those conditions and a simulation and a per-store
property regression test the resulting mechanism.

## Repository structure

```
.
├── code/
│   ├── rossmann_eda.Rmd             # Data description and exploratory figures (+ STL)
│   ├── rossmann_simulation.Rmd      # Controlled simulation of the gap = Var(delta) * rho^2 mechanism
│   ├── rossmann_stage1_models.Rmd   # Stage 1 — full 11-model comparison (no lags)
│   ├── rossmann_stages2_4.Rmd       # Stages 2–4 — subsetting, lag enrichment, rolling robustness
│   ├── rossmann_stage5.Rmd          # Stage 5 — incremental (nested) information sets
│   ├── rossmann_stage6.Rmd          # Stage 6 — extensions (recursive lags, DM tests, grid, scale, global vs local)
│   └── rossmann_data_properties.Rmd # Generalisability — per-store gain vs measurable data properties
├── data/
│   └── README.md                    # how to obtain the Rossmann data
├── .gitignore
└── LICENSE
```

Each `.Rmd` is **self-contained** — it defines the full pipeline (preprocessing,
models, metrics) on its own and runs one stage. There is no shared source file to
keep alongside it; you can knit any file independently. All stages use the same
preprocessing and the same `run_xgboost` / `run_lightgbm` with
temporal-validation early stopping, so the numbers are consistent across stages.

## Code → thesis mapping

| File | Part | What it does |
|------|------|--------------|
| `rossmann_eda.Rmd` | Data | Data description and exploratory figures (sales distribution, trend and seasonality, promotion and store-type effects, correlations, and a calendar-aligned STL decomposition). Exports figures to `eda_outputs/`. |
| `rossmann_simulation.Rmd` | Framework | A controlled data-generating process with `Var(delta)` and the recoverable share `rho^2` set by hand, confirming the gap `≈ Var(delta) * rho^2`. |
| `rossmann_stage1_models.Rmd` | Stage 1 | Full no-lag comparison of 11 specifications: two naive baselines, the statistical models (Auto-ARIMA, ETS, Theta), linear regression, CART, bagging (`mtry = p`), Random Forest (`mtry = p/3`), XGBoost, and LightGBM. |
| `rossmann_stages2_4.Rmd` | Stages 2–4 | Subsetting experiments (store count, training window, store type), lag/rolling-mean enrichment with feature-importance share, and rolling 42-day test-window robustness. |
| `rossmann_stage5.Rmd` | Stage 5 | Fixed-origin 42-day forecasts under four nested information sets (history → calendar → store attributes → promotions/holidays). |
| `rossmann_stage6.Rmd` | Stage 6 | Recursive fixed-origin lag forecasts, Diebold–Mariano tests, number-of-stores variability with standard errors, store-level framework test, hyperparameter grid, fixed-composition scale (to the full Type-A set), and global-vs-local comparison. |
| `rossmann_data_properties.Rmd` | Generalisability | Computes series-level properties per store and regresses the realised gain over Store-DOW on them, so the advantage is expressed in transferable data properties rather than Rossmann-specific settings. |

## Requirements

- **R** ≥ 4.2
- R packages: `tidyverse`, `lubridate`, `zoo`, `forecast`, `xgboost`,
  `lightgbm`, `ranger`, `rpart`, `broom`

```r
install.packages(c("tidyverse", "lubridate", "zoo", "forecast",
                   "xgboost", "lightgbm", "ranger", "rpart", "broom"))
```

## How to run

1. Download the data into the same folder as the `.Rmd` files (see `data/README.md`).
   Each file reads `train.csv` and `store.csv` from its working directory.
2. Open a file in RStudio and **Knit**, or from the console:
   `rmarkdown::render("code/rossmann_stage1_models.Rmd")`.
3. Each file sets a fixed seed and prints `sessionInfo()`. Stage 6 has
   `RUN_E1..E7` switches and the subsetting experiments have an `N_REPS` knob to
   trade runtime against precision.

> Note on reproducibility: XGBoost uses row/column subsampling without a fixed
> internal seed, so its RMSE can vary by ~1–2% between runs; this does not change
> any conclusion in the thesis.

## Data

The Rossmann Store Sales data is **not redistributed here** (Kaggle competition
terms). `data/README.md` explains how to download `train.csv` and `store.csv`.

## Citation

> Işık, K. *When Do Tree-Based Methods Win? Dataset Characteristics as
> Determinants of Ensemble Performance.* Master's thesis.
