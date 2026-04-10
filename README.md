# 🌫️ Air Quality Prediction

> Predict AQI values and classify air quality categories across Indian cities using machine learning — with feature engineering, SMOTE balancing, hyperparameter tuning, ensemble methods, and SHAP explainability.

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-latest-orange?logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-latest-red)
![LightGBM](https://img.shields.io/badge/LightGBM-latest-green)
![License](https://img.shields.io/badge/license-MIT-blue)

---
## Overview

This project tackles air quality prediction as two parallel problems:

| Task | Target | Metric |
|---|---|---|
| **Regression** | AQI (continuous, 0–600+) | R² score |
| **Classification** | AQI Bucket (Good → Severe) | Accuracy |

Data covers five Indian cities — **Delhi, Kolkata, Mumbai, Chandigarh, and Bengaluru** — with daily pollutant readings across 3,349 records.

Key techniques applied:
- **Feature engineering** — 5 interaction features derived from raw pollutants
- **SMOTE** — oversampling to fix severe class imbalance (Severe: 61 vs Moderate: 1,375)
- **GridSearchCV** — 108-combination hyperparameter search for XGBoost
- **Soft Voting Ensemble** — RF + Tuned XGBoost + LightGBM combined
- **SHAP** — TreeExplainer to identify per-feature impact on each prediction

---

## Dataset

**File:** `air_quality_data.csv`

| Column | Type | Description |
|---|---|---|
| `City` | string | One of 5 Indian cities |
| `Date` | date | Daily reading |
| `PM2.5` | float | Fine particulate matter (µg/m³) |
| `PM10` | float | Coarse particulate matter (µg/m³) |
| `NO2` | float | Nitrogen dioxide (µg/m³) |
| `SO2` | float | Sulfur dioxide (µg/m³) |
| `O3` | float | Ozone (µg/m³) |
| `AQI` | float | Air Quality Index (target for regression) |
| `AQI_Bucket` | string | Category label (target for classification) |

**Class distribution (before SMOTE):**

```
Moderate      1375
Satisfactory  1128
Good           319
Poor           308
Very Poor      158
Severe          61
```

Missing values (6 per column) are filled with column means; missing `AQI_Bucket` labels are derived from AQI thresholds.

---

## Project Structure

```
air_quality/
│
├── air_quality_data.csv          # Raw dataset
├── air_quality_final.ipynb       # Main notebook
├── requirements.txt              # Dependencies
└── README.md
```

---

## Pipeline

```
Raw Data (3349 rows, 9 cols)
        │
        ▼
  Data Cleaning
  (mean imputation, bucket fill, drop City/Date)
        │
        ▼
  Feature Engineering
  (5 raw → 10 total features)
        │
        ├──────────────────────────────┐
        ▼                              ▼
  REGRESSION                    CLASSIFICATION
  Target: AQI (continuous)      Target: AQI_Bucket (6 classes)
        │                              │
        │                         SMOTE balancing
        │                         (3349 → 8250 samples)
        │                              │
        ▼                              ▼
  Cross-Validation               GridSearchCV (XGBoost)
  (Linear Reg, RF,               + Soft Voting Ensemble
   XGBoost, LightGBM,            (RF + XGBoost + LightGBM)
   Stacking)
        │                              │
        ▼                              ▼
  Best Model Evaluation         Best Model Evaluation
  + SHAP Explainability         + Confusion Matrix
```

### Engineered Features

| Feature | Formula | Rationale |
|---|---|---|
| `PM_total` | PM2.5 + PM10 | Total particulate load |
| `gas_total` | NO2 + SO2 + O3 | Total gas pollutants |
| `PM_gas_ratio` | PM_total / (gas_total + 1) | Particulate vs gas balance |
| `PM2.5_PM10_ratio` | PM2.5 / (PM10 + 1) | Fine vs coarse particle ratio |
| `NO2_O3_ratio` | NO2 / (O3 + 1) | Photochemical activity proxy |

---

## Models & Results

### Regression (AQI prediction)

| Model | Mean R² |
|---|---|
| XGBoost | ~0.994 |
| LightGBM | — |
| Random Forest | — |
| Linear Regression | — |
| Stacking Ensemble | — |
| **Baseline (mean)** | **–0.005** |

> Best model achieves R² ≈ 0.9942 with MAE ≈ 4.3 on training data. A proper train/test split is recommended for unbiased estimates — see [Known Issues](#known-issues--fixes).

### Classification (AQI category)

| Model | CV Accuracy | Std Dev |
|---|---|---|
| **Voting Ensemble** | **95.67%** | ±0.70% |
| XGBoost (Tuned) | 95.66% | ±0.66% |
| LightGBM | 95.60% | ±0.60% |
| Random Forest | 95.41% | ±0.85% |
| XGBoost | 95.03% | ±0.71% |
| **Baseline (majority)** | **41.06%** | — |

Cross-validation was run on SMOTE-balanced data with `StratifiedKFold(n_splits=5)`.

**GridSearchCV best parameters (XGBoost):**
```
colsample_bytree : 0.8
learning_rate    : 0.1
max_depth        : 7
n_estimators     : 300
subsample        : 0.8
Best CV accuracy : 95.66%
```

---

## SHAP Explainability

SHAP (SHapley Additive exPlanations) was used to explain the best regression model's predictions.

**Top features by mean absolute SHAP value:**

| Rank | Feature | Impact |
|---|---|---|
| 1 | PM2.5 | Highest — strong positive driver |
| 2 | PM_total | High — engineered feature validates well |
| 3 | PM10 | Moderate positive |
| 4 | PM_gas_ratio | Moderate |
| 5 | SO2 | Low positive |

Key insight: **PM2.5 and PM_total together dominate AQI prediction**, confirming that particulate matter (not gas pollutants like O3 or NO2) is the primary air quality driver in this dataset. The engineered `PM_total` feature adds genuine predictive signal beyond PM2.5 alone.

---

## Installation

```bash
# Clone the repository
git clone https://github.com/your-username/air-quality-prediction.git
cd air-quality-prediction

# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

**`requirements.txt`**
```
pandas
numpy
matplotlib
seaborn
scikit-learn
imbalanced-learn
xgboost
lightgbm
shap
jupyter
```

---

## Usage

```bash
# Launch the notebook
jupyter notebook air_quality_final.ipynb
```

Run cells in order. The notebook is self-contained — data loading, cleaning, feature engineering, training, and evaluation all run sequentially.

---

## Known Issues & Fixes

### 1. `KeyError: 'Mean'` in Final Summary cell
The summary cell references `clf_results['Mean']` but the column is stored as `'Mean Accuracy'`.

**Fix:**
```python
# Change this:
best_acc = clf_results[...]['Mean'].max()

# To this:
best_acc = clf_results[~clf_results['Model'].str.contains('Baseline')]['Mean Accuracy'].max()
```

### 2. `NameError: StackingRegressor` not imported
`StackingRegressor` is used in the regression model definitions but missing from imports.

**Fix:** Add to the imports cell:
```python
from sklearn.ensemble import StackingRegressor
```

### 3. Evaluation on training data (data leakage)
The confusion matrix and classification report are generated by predicting on the same data the model was trained on, producing an artificially perfect 1.00 score across all classes.

## Future Improvements

- Add a proper 80/20 train/test split for unbiased evaluation
- Apply SMOTE inside cross-validation folds using `Pipeline` + `imblearn`
- Include temporal features (month, season) to capture pollution seasonality
- Add city as a one-hot encoded feature instead of dropping it
- Deploy best model as a REST API (FastAPI or Flask)
- Build an interactive dashboard for real-time AQI prediction

---

## License

This project is licensed under the MIT License.
