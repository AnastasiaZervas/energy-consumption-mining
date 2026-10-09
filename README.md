# Household Energy Consumption Mining

Data-mining study of the UCI *Individual Household Electric Power Consumption* dataset: about four years of minute-level readings from one household. Two notebooks cover preprocessing and exploratory analysis, then classification, regression, clustering, association rules and time-series forecasting.

## What is in the notebooks

**`notebooks/data_preprocessing.ipynb`** — loading and cleaning the raw readings, feature engineering (calendar features, lags, rolling statistics), a chronological train/validation/test split, scaling, and exploratory analysis.

**`notebooks/modeling.ipynb`**

| Task | Approach |
|---|---|
| Classification of high-consumption days | Season-adjusted target (a day counts as high when it is more than 15% above its seasonal mean); Logistic Regression, Random Forest and XGBoost |
| Regression | Next-day total consumption with Linear Regression, Random Forest and XGBoost |
| Clustering | K-Means on 24-dimensional hourly profiles, normalised per day so clusters reflect the shape of the day; k = 4 chosen for interpretability (silhouette ≈ 0.104, Davies–Bouldin ≈ 2.223) |
| Association rules | Apriori on discretised daily features, mined on the training set and checked on validation and test |
| Forecasting | Prophet, evaluated on validation and then retrained for the test period |

Features that would leak the target are listed and excluded explicitly, and every threshold or quantile is computed on the training split only.

## Findings

- All three classifiers pass the 85% accuracy target with similar scores on train, validation and test.
- Lag features (previous day, previous week) and rolling statistics carry most of the predictive signal.
- The strongest association rules link high afternoon and peak-hour usage to high daily consumption, with lift above 3.5.

## Running it

```bash
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

The dataset is not included because of its size. Download it from the [UCI repository](https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption) and place `household_power_consumption.txt` in `data/`. Run `data_preprocessing.ipynb` first, then `modeling.ipynb`.

## Repository layout

```
notebooks/   Preprocessing/EDA and modeling notebooks
models/      Saved classifiers, feature lists and decision thresholds
data/        Dataset location (not committed)
results/     Generated outputs (not committed)
docs/        Assignment brief and setup notes (in Greek)
```

## Context

Coursework for the Data Mining course, Department of Informatics and Telematics, Harokopio University of Athens.
