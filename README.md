# 📡 Telecom Customer Churn Analysis & Prediction

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Records](https://img.shields.io/badge/Records-7%2C043-informational)]()
[![Features](https://img.shields.io/badge/Features-37-blueviolet)]()

> End-to-end churn analysis for a California-based telecom provider — identifying why customers leave and predicting who is at risk, so retention teams can act before it's too late.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset Summary](#dataset-summary)
- [File Structure](#file-structure)
- [Data Dictionary](#data-dictionary)
- [Key Metrics at a Glance](#key-metrics-at-a-glance)
- [Installation](#installation)
- [Usage](#usage)
- [Analysis Workflow](#analysis-workflow)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Contributing](#contributing)
- [License](#license)

---

## 📌 Overview

This project analyses customer churn for a telecom company operating across California. Using a rich dataset of **7,043 customers** and **37 features**, it combines exploratory data analysis with machine learning to:

- Understand the root causes of churn (competitor pricing, dissatisfaction, service attitude)
- Profile high-risk customer segments by age, tenure, contract type, and internet service
- Build predictive models that flag at-risk customers before they cancel
- Surface actionable recommendations for marketing and retention teams

---

## 📊 Dataset Summary

| Attribute | Details |
|-----------|---------|
| **File** | `Telecom-Customer-Churn-Dataset.xlsx` |
| **Sheets** | `Data`, `Pivots`, `Dashboard` |
| **Total Customers** | 7,043 |
| **Total Features** | 37 columns |
| **Churn Rate** | **26.16%** (1,869 churned) |
| **New Joiners** | 243 |
| **Geography** | California, USA (multiple cities) |
| **Tenure Range** | 1 – 72 months |
| **Age Range** | 19 – 80 years |
| **Monthly Charge Range** | -$10.00 – $118.75 |

### Workbook Sheets

| Sheet | Description |
|-------|-------------|
| `Data` | Raw customer-level data (7,043 rows × 37 columns) |
| `Pivots` | Pre-aggregated pivot tables (churn by contract, internet type, age group, city, etc.) |
| `Dashboard` | High-level KPI summary (Total Customers, Total Churn, Churn Rate, New Joiners) |

---

## 🗂️ File Structure

```
telecom-churn-prediction/
│
├── data/
│   └── Telecom-Customer-Churn-Dataset.xlsx   # Source dataset (3 sheets)
│
├── notebooks/
│   ├── 01_EDA.ipynb                          # Exploratory Data Analysis
│   ├── 02_preprocessing.ipynb                # Cleaning & feature engineering
│   ├── 03_modeling.ipynb                     # Model training & evaluation
│   └── 04_insights.ipynb                     # Business insights
│
├── src/
│   ├── data_preprocessing.py                 # Cleaning pipeline
│   ├── eda.py                                # EDA helper functions
│   ├── model.py                              # Training & evaluation
│   └── predict.py                            # Inference pipeline
│
├── models/
│   ├── random_forest.pkl
│   └── xgboost_model.pkl
│
├── reports/
│   └── figures/                              # EDA plots & confusion matrices
│
├── requirements.txt
└── README.md
```

---

## 📖 Data Dictionary

### Customer Demographics

| Column | Type | Description |
|--------|------|-------------|
| `Customer ID` | str | Unique identifier (e.g., `0002-ORFBO`) |
| `Gender` | str | Male / Female |
| `Age` | int | Customer age (19–80) |
| `Age Group` | str | Binned: 0-20, 21-30, 31-40, 41-50, 51-60, Above 60 |
| `Married` | str | Yes / No |
| `Number of Dependents` | int | Dependents living with the customer |

### Location

| Column | Type | Description |
|--------|------|-------------|
| `City` | str | California city (e.g., Los Angeles, San Diego) |
| `Zip Code` | int | Postal code |
| `Latitude` | float | Geographic latitude |
| `Longitude` | float | Geographic longitude |

### Account & Tenure

| Column | Type | Description |
|--------|------|-------------|
| `Number of Referrals` | int | Referrals made by the customer |
| `Tenure in Months` | int | How long the customer has been with the company (1–72) |
| `Tenure Group` | str | Binned: 0-6 Months, 6-12 Months, 12-18 Months, 18-24 Months, Above 24 Months |
| `Offer` | str | Promotional offer applied: Offer A–E or None |
| `Contract` | str | Month-to-Month, One Year, Two Year |
| `Paperless Billing` | str | Yes / No |
| `Payment Method` | str | Bank Withdrawal, Credit Card, Mailed Check |

### Services Subscribed

| Column | Type | Description |
|--------|------|-------------|
| `Phone Service` | str | Yes / No |
| `Internet Service` | str | Yes / No |
| `Internet Type` | str | Fiber Optic, Cable, DSL, or None |
| `Online Security` | str | Yes / No (NaN if no internet) |
| `Online Backup` | str | Yes / No (NaN if no internet) |
| `Device Protection Plan` | str | Yes / No (NaN if no internet) |
| `Premium Tech Support` | str | Yes / No (NaN if no internet) |
| `Streaming TV` | str | Yes / No (NaN if no internet) |
| `Streaming Movies` | str | Yes / No (NaN if no internet) |
| `Streaming Music` | str | Yes / No (NaN if no internet) |
| `Unlimited Data` | str | Yes / No (NaN if no internet) |

### Billing & Revenue

| Column | Type | Description |
|--------|------|-------------|
| `Monthly Charge` | float | Current monthly bill ($) |
| `Total Charges` | float | Cumulative charges ($) |
| `Total Refunds` | float | Total refunds issued ($) |
| `Total Extra Data Charges` | int | Charges for extra data usage ($) |
| `Total Long Distance Charges` | float | Long-distance call charges ($) |
| `Total Revenue` | float | Total revenue from customer ($) |

### Target & Churn Labels

| Column | Type | Description |
|--------|------|-------------|
| `Customer Status` | str | **Target** — Stayed, Churned, Joined |
| `Churn Category` | str | Competitor, Dissatisfaction, Attitude, Price, Other, Not Churned |
| `Churn Reason` | str | Granular reason (e.g., "Competitor had better devices") |

> **Note:** 1,526 customers have `NaN` in internet-related columns — these are customers with no internet service subscription.

---

## 📈 Key Metrics at a Glance

### Customer Status Distribution

| Status | Count | % |
|--------|-------|---|
| Stayed | 4,720 | 67.0% |
| **Churned** | **1,869** | **26.5%** |
| Joined | 454 | 6.4% |

### Churn by Root Cause

| Category | Churned Customers |
|----------|-------------------|
| Competitor | 841 (45.0%) |
| Dissatisfaction | 321 (17.2%) |
| Attitude | 314 (16.8%) |
| Price | 211 (11.3%) |
| Other | 182 (9.7%) |

### Churn by Contract Type

| Contract | Churn Rate |
|----------|-----------|
| Month-to-Month | **44.7%** |
| One Year | 11.5% |
| Two Year | 2.8% |

### Churn by Internet Type

| Internet Type | Churn Rate |
|---------------|-----------|
| Fiber Optic | **39.8%** |
| Cable | 26.6% |
| DSL | 19.0% |
| None | 7.3% |

### Churn by Tenure Group

| Tenure | Churn Rate |
|--------|-----------|
| 0–6 Months | **52.4%** |
| 6–12 Months | 35.0% |
| 12–18 Months | 29.5% |
| 18–24 Months | 28.6% |
| Above 24 Months | 14.1% |

### Churn by Age Group

| Age Group | Churn Rate |
|-----------|-----------|
| Above 60 | **36.3%** |
| 51–60 | 25.6% |
| 41–50 | 23.0% |
| 31–40 | 22.5% |
| 21–30 | 22.6% |
| 0–20 | 14.9% |

---

## ⚙️ Installation

### Prerequisites

- Python 3.8+
- pip

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/telecom-churn-prediction.git
cd telecom-churn-prediction

# 2. Create and activate virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### requirements.txt

```txt
pandas>=1.5.0
numpy>=1.23.0
openpyxl>=3.0.0
scikit-learn>=1.1.0
xgboost>=1.7.0
imbalanced-learn>=0.9.0
matplotlib>=3.6.0
seaborn>=0.12.0
plotly>=5.11.0
shap>=0.41.0
streamlit>=1.15.0
joblib>=1.2.0
```

---

## 🚀 Usage

### Load the Dataset

```python
import pandas as pd

# Load the main customer data
df = pd.read_excel("data/Telecom-Customer-Churn-Dataset.xlsx", sheet_name="Data")

# Load pre-aggregated pivot summaries
pivots = pd.read_excel("data/Telecom-Customer-Churn-Dataset.xlsx", sheet_name="Pivots")

print(df.shape)   # (7043, 37)
print(df['Customer Status'].value_counts())
```

### Run EDA Notebook

```bash
jupyter notebook notebooks/01_EDA.ipynb
```

### Train Model

```bash
python src/model.py --train
```

### Predict Churn for New Customers

```bash
python src/predict.py --input data/new_customers.xlsx --output predictions.csv
```

---

## 🔬 Analysis Workflow

### 1. Data Preprocessing
- Handle nulls: internet-related columns are `NaN` for non-internet customers — fill with `"No Service"`
- Drop or impute `Offer` nulls (3,877 missing = customers with no active promotional offer)
- Encode categorical columns (One-Hot / Label Encoding)
- Scale numeric features: `Age`, `Tenure in Months`, `Monthly Charge`, `Total Revenue`

### 2. Exploratory Data Analysis
- Churn distribution and class imbalance assessment
- Correlation heatmap across numeric features
- Churn rate by contract type, internet type, tenure group, age group, city
- Top churn reasons (Competitor vs. Dissatisfaction vs. Price)
- Geographic churn map (latitude/longitude available)

### 3. Handling Class Imbalance
- Churned (26.5%) vs. Stayed (67%) — moderate imbalance
- Apply **SMOTE** on training split only
- Use `class_weight='balanced'` in tree-based models
- Evaluate on **F1-Score** and **ROC-AUC**, not just accuracy

### 4. Feature Engineering Suggestions
- `has_internet`: Binary flag (Internet Service = Yes)
- `services_count`: Count of active add-on services per customer
- `charge_per_month_ratio`: `Total Charges / Tenure in Months`
- `is_new_customer`: Flag for tenure ≤ 6 months
- `is_senior`: Flag for Age Group = "Above 60"

### 5. Model Training
Train and compare:
- Logistic Regression (baseline)
- Random Forest
- XGBoost / LightGBM
- Support Vector Machine

Use **Stratified 5-Fold Cross-Validation** to preserve churn ratio across folds.

---

## 💡 Key Findings

### Top Churn Reasons (Granular)
1. **Competitor had better devices** — 313 customers
2. **Competitor made a better offer** — 311 customers
3. **Attitude of support person** — 220 customers
4. **Competitor offered more data** — 117 customers
5. **Price too high** — 78 customers

### High-Risk Segments
- 📅 **Month-to-month** customers churn at **44.7%** — more than 4× the Two Year rate
- 🌐 **Fiber Optic** users have the highest churn (39.8%) despite being a premium service
- ⏳ **New customers (0–6 months)** churn at **52.4%** — the most critical retention window
- 👴 Customers **above 60** churn at **36.3%** — likely more price-sensitive or less tech-savvy
- 🏙️ **San Diego** leads in total churned customers (93), followed by Los Angeles (31)

### Business Recommendations
1. **Prioritise early engagement** — target months 1–6 with onboarding calls and loyalty offers
2. **Incentivise annual contracts** — even a small discount on One Year plans can cut churn by ~33pp
3. **Investigate Fiber Optic dissatisfaction** — high price + high churn signals a service quality issue
4. **Win back competitive losses** — 45% of churn is competitor-driven; respond with device upgrade programmes and data bundle improvements
5. **Improve support culture** — "Attitude of support person" is the #3 reason; training investment has direct churn impact
6. **Senior customer programme** — dedicated support or simplified plans for the 60+ segment

---

## 🛠️ Technologies Used

| Category | Tools |
|----------|-------|
| Language | Python 3.8+ |
| Data Processing | Pandas, NumPy, OpenPyXL |
| Visualisation | Matplotlib, Seaborn, Plotly |
| Machine Learning | Scikit-learn, XGBoost, LightGBM |
| Imbalanced Data | imbalanced-learn (SMOTE) |
| Explainability | SHAP |
| Notebook | Jupyter |
| Version Control | Git, GitHub |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit changes: `git commit -m "Add: description"`
4. Push: `git push origin feature/my-feature`
5. Open a Pull Request

Please ensure code follows PEP 8 and includes docstrings for all functions.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 📬 Contact

**Author:** Your Name
**Email:** your.email@example.com
**GitHub:** [github.com/yourusername](https://github.com/venkat2961)

---

> ⭐ If this project helped you, please give it a star!
