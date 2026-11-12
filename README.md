# Explainable CNN-LSTM forecasting of retail petrol prices: Pakistan and Saudi Arabia

Code, data and results for the manuscript

> *Forecasting fuel prices under geopolitical uncertainty: A SHAP-explainable CNN-LSTM approach for oil-importing and oil-exporting economies (Pakistan and Saudi Arabia)* — Khalil Ullah et al.

A single notebook reproduces every number, table and figure in the manuscript: one-week-ahead forecasts of weekly retail petrol prices (January 2013 – April 2026) with a CNN-LSTM and four other neural networks (LSTM, Bi-LSTM, CNN-GRU, CNN-LSTM-Attention), ARIMA, ARIMAX, SVR and naive benchmarks, evaluated with a chronological hold-out split and a rolling-origin design, Diebold–Mariano tests, event windows, ablations, Kernel SHAP and Grad-CAM.

## Repository contents

| Path | Content |
|---|---|
| `Petrol_Price_predictions_R2.ipynb` | Full analysis pipeline (S1 File of the manuscript) |
| `data/raw/Petrol Price dataset updated_2026-1.csv` | Pakistan input file: week-end date, USD/PKR rate, World Uncertainty Index, WTI crude price, retail petrol price (PKR/litre) |
| `data/raw/Saudi_PetrolFinalComplete.csv` | Saudi Arabia input file: week-end date, USD/SAR rate, World Uncertainty Index, WTI crude price, retail gasoline price (SAR/litre) |
| `data/processed/PK_weekly_dataset.csv`, `SA_weekly_dataset.csv` | Weekly-aligned modelling datasets written by the notebook (crude oil lagged one month before August 2023) |
| `results/R2_key_numbers.csv` | Every number quoted in the manuscript, keyed by ID |
| `results/Petrol_Price_Results_R2.xlsx` | All result tables (hold-out, rolling-origin folds, DM tests, look-back and random search, SHAP, diagnostics) |
| `results/tables/` | LaTeX tables used in the manuscript |
| `results/figures/` | Figures used in the manuscript and supporting information |
| `results/models/` | Trained hold-out models (seed 0), Keras format |
| `results/environment.json` | Software versions and run flags of the reported run |

## Reproducing the results

```bash
pip install -r requirements.txt
export PETROL_DATA_DIR=data/raw/     # folder containing the two raw CSV files
export PETROL_OUT_ROOT=.             # outputs are written to ./R2_outputs
jupyter nbconvert --to notebook --execute Petrol_Price_predictions_R2.ipynb
```

Set `PETROL_FAST=1` for a quick smoke test with reduced seeds and search budget (outputs go to `R2_outputs_FAST`). The notebook also runs on Google Colab, where it mounts Google Drive and checkpoints intermediate results. The reported run used Python 3.12 on Windows 11 without a GPU (`results/environment.json`); exact neural-network results can differ slightly across hardware and library versions.

Key settings in the notebook: `TARGET_MODE = 'diff'` (networks receive weekly changes and forecast the next weekly price change; `'level'` reproduces the level-representation sensitivity analysis) and `CRUDE_LAG_FIX = True` (previous month's WTI settlement for weeks before August 2023, avoiding look-ahead).

## Data sources and terms of use

| Variable | Source |
|---|---|
| Retail petrol price, Pakistan | Pakistan Bureau of Statistics via the Karandaaz data portal (https://portal.karandaaz.com.pk/dataset/petrol-avg/1388) |
| Retail gasoline price, Saudi Arabia | Trading Economics (https://tradingeconomics.com/saudi-arabia/gasoline-prices), Octane 95 |
| USD/PKR, USD/SAR | https://www.exchangerates.org.uk (spot history, value on the week-end date) |
| Crude oil | WTI crude oil, NYMEX front-month futures settlement prices |
| World Uncertainty Index | Ahir, Bloom & Furceri; Pakistan: https://worlduncertaintyindex.com (monthly); Saudi Arabia: FRED series WUISAU (quarterly) |

All series were collected manually on 23 April 2026 from publicly accessible pages, without automated scraping. The World Uncertainty Index and the Karandaaz/PBS price data are openly reusable with attribution. The exchange-rate, Saudi gasoline-price and WTI series come from websites that permit personal and non-commercial use but do not grant a general redistribution licence; they are included here only as a weekly-aligned compilation, with attribution, to reproduce the reported results for non-commercial academic research. Openly available equivalents are: State Bank of Pakistan exchange rates for USD/PKR, the official peg of 3.75 SAR per USD, Saudi Aramco's gasoline price announcements and the U.S. Energy Information Administration's WTI prices.

## Citation

If you use this code or data, please cite the article (citation details will be added on publication).
