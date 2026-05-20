# 🛡️ MediGuard AI — FAERS Pharmacovigilance Platform
An end-to-end machine learning platform that predicts **fatal drug adverse events** using the FDA Adverse Event Reporting System (FAERS), covering **528,000 reports from 2015 to 2026**.

---

## 📸 Demo

> **Live App:** [https://mediguardd.streamlit.app](https://mediguardd.streamlit.app)

---

## 🎯 Project Overview

MediGuard AI walks through an entire machine learning pipeline — from raw FAERS data to a live risk predictor — in 8 interactive pages:

| Page | Description |
|---|---|
| 🏠 Overview | Dataset summary, pipeline architecture, target distribution |
| 🔬 Data & EDA | 14 EDA steps: missing values, distributions, leakage detection |
| ⚙️ Preprocessing | 11-step pipeline: leakage removal, outlier capping, encoding |
| 🧬 Feature Engineering | 7 clinically-motivated features with fatal rate lift analysis |
| 🤖 Model Training | 5 classifiers: LR, Decision Tree, Random Forest, XGBoost, LightGBM |
| 📊 Model Comparison | Radar charts, ROC curves, feature importance |
| 🎯 Risk Predictor | Interactive patient profile → real-time risk score |
| 📋 Conclusions | Key findings, limitations, and future improvements |

---

## 📊 Results Summary

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| **Random Forest** ⭐ | 0.8125 | 0.3054 | 0.6457 | 0.4147 | **0.8414** |
| XGBoost | 0.9010 | 0.5251 | 0.3885 | 0.4466 | 0.8380 |
| LightGBM | 0.8981 | 0.5061 | 0.3755 | 0.4311 | 0.8212 |
| Decision Tree | 0.6396 | 0.1889 | 0.7603 | 0.3026 | 0.7703 |
| Logistic Regression | 0.6343 | 0.1630 | 0.6184 | 0.2580 | 0.6764 |

**Random Forest** was selected as the best model (ROC-AUC = 0.8414) for its optimal precision-recall balance in a 10.3% imbalanced setting.

---

## 🏗️ ML Pipeline

```
Raw FAERS Data (528K rows)
    ↓
1. EDA (leakage, missingness, cardinality)
    ↓
2. Drop leakage columns (7 cols) + redundant columns (9 cols)
    ↓
3. Fix masked NaN values
    ↓
4. Cap outliers at 99th percentile
    ↓
5. Feature Engineering (7 new features)
    ↓
6. Encoding (ordinal, label, frequency)
    ↓
7. Train/Test Split (80/20 stratified)
    ↓
8. StandardScaler (fit on train only)
    ↓
9. SMOTE (train set only → 50/50 balance)
    ↓
10. Train 5 classifiers → evaluate on real-distribution test set
```

---

## 🧬 Engineered Features

| Feature | Type | Fatal Rate Lift |
|---|---|---|
| `is_elderly_polypharmacy` | Binary | **+98.6%** |
| `is_covid_year` | Binary | +86% |
| `is_multi_reaction` | Binary | +59% |
| `is_polypharmacy_high` | Binary | +47% |
| `is_high_risk_route` | Binary | +46% |
| `reaction_drug_ratio` | Ratio | r=+0.08 |
| `is_us_report` | Binary | +5% |

---

## 🚀 Running Locally

### 1. Clone the repository
```bash
git clone https://github.com/samaabdelbary/mediguard-ai.git
cd mediguard-ai
```

### 2. Create a virtual environment (recommended)
```bash
python -m venv venv
source venv/bin/activate        # macOS/Linux
venv\Scripts\activate           # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the app
```bash
streamlit run mediguard_app.py
```

The app will open at `http://localhost:8501`.

> **Note:** The app uses simulated/aggregated data and does not require the raw FAERS CSV to run. The Risk Predictor page uses a rule-based approximation, not a live model file.

---

## 📁 Repository Structure

```
mediguard-ai/
├── mediguard_app.py                          # Main Streamlit application
├── Pharmacovigilance_ML_Pipeline.ipynb       # Full ML pipeline notebook
├── requirements.txt                          # Python dependencies
├── README.md                                 # This file
└── .gitignore                                # Files to exclude from Git
```

---

## 📦 Dataset
- **Source:** [FDA Drug Adverse Event Reports 2015–2026 — Kaggle](https://www.kaggle.com/datasets/kanchana1990/fda-drug-adverse-event-reports-2015-to-2026-faers)
- **Size:** 528,000 reports (2015–2026), 26 columns
- **Target Variable:** `is_fatal` (10.3% positive rate)
- **Key Columns:** `suspect_drug`, `primary_reaction`, `age_group`, `num_drugs`, `num_reactions`, `drug_route`, `patient_sex`, `year`

> The raw dataset is not included in this repo due to size. Download it from the FDA FAERS link above and update `DATA_PATH` in the notebook.

---

## ⚠️ Disclaimer

FAERS reports indicate **suspected associations** — they do not confirm causation. All predictions and risk scores in this app are for analytical and educational purposes only and **do not constitute medical advice**.

---

## 👥 Team
Ahmed Shehab · Sama Abdelbary · Mina Magdy · Shahd Yousery · Malak Abdelkader
