# 🏦 Bank Customer Churn Analysis

A full end-to-end data science project analyzing why bank customers leave — and building tools to predict and prevent it.

---

## 📌 Project Overview

Customer churn is one of the most costly problems in banking. This project uses a dataset of **10,000 bank customers** to understand churn behavior, predict which customers are at risk, and surface actionable business insights.

**Overall churn rate: 20.4%** — roughly 1 in 5 customers left the bank.

---

## 📂 Dataset

| Field | Details |
|---|---|
| **File** | `churn_bank_customers.xlsx` |
| **Rows** | 10,000 customers |
| **Columns** | 14 features |
| **Missing Values** | None |
| **Target Variable** | `Exited` (1 = churned, 0 = retained) |

### Features
| Column | Type | Description |
|---|---|---|
| CreditScore | Numeric | Customer credit score |
| Geography | Categorical | France, Germany, Spain |
| Gender | Categorical | Male / Female |
| Age | Numeric | Customer age |
| Tenure | Numeric | Years with the bank |
| Balance | Numeric | Account balance |
| NumOfProducts | Numeric | Number of bank products held |
| HasCrCard | Binary | Has credit card (1/0) |
| IsActiveMember | Binary | Active member (1/0) |
| EstimatedSalary | Numeric | Estimated annual salary |
| Exited | Binary | **Target** — churned (1) or retained (0) |

---

## 🗂️ Project Modules

| # | Module | Status |
|---|---|---|
| 1 | Exploratory Data Analysis | ✅ Complete |
| 2 | Churn Prediction (ML) | ✅ Complete |
| 3 | Customer Segmentation | 🔄 In Progress |
| 4 | Visualization & Dashboard | 🔄 In Progress |
| 5 | Business Insights Report | 🔄 In Progress |

---

## 📊 Module 1 — Exploratory Data Analysis

**File:** `1_exploratory_data_analysis/eda.py`

**Steps covered:**
- Step 1: Data overview (shape, dtypes, nulls, summary stats)
- Step 2: Target variable distribution (class imbalance check)
- Step 3: Univariate analysis (distributions of all features)
- Step 4: Bivariate analysis (each feature vs churn)
- Step 5: Correlation heatmap
- Step 6: Segment deep-dives (Geography × Gender × Activity)

### 🔍 Key Findings

**Top churn drivers:**

| Factor | Finding |
|---|---|
| NumOfProducts | Customers with 3 products churn at 83%, with 4 at 100% |
| Geography | Germany churns at 32.4% — double France (16.2%) and Spain (16.7%) |
| Age | Churned customers average 44.8 years vs 37.4 for retained |
| IsActiveMember | Inactive members churn at 26.9% vs 14.3% for active |
| Gender | Females churn at 25.1% vs males at 16.5% |
| Balance | Churned customers hold higher balances (€91K vs €73K) |

**Weakest predictors:** CreditScore, Tenure, EstimatedSalary, HasCrCard — near-zero correlation with churn.

**Most at-risk segment:** Inactive German females — **44.6% churn rate**.

---

## 🤖 Module 2 — Churn Prediction 

File: 2_churn_prediction/churn_prediction.py
Steps covered:

Step 1: Load & inspect data
Step 2: Preprocessing (drop irrelevant columns, encode categoricals, train/test split 80/20, feature scaling)
Step 3: Train 3 classifiers — Logistic Regression, Random Forest, XGBoost
Step 4: Evaluate models (Accuracy, AUC-ROC, Confusion Matrix, Classification Report)
Step 5: Visualizations (Confusion Matrices, ROC Curves, Model Comparison, Feature Importance)
Step 6: Final summary & best model selection

📈 Model Performance
ModelAccuracyAUC-ROCChurned PrecisionChurned RecallLogistic Regression80.50%0.77100.590.14Random Forest86.40%0.84640.780.46XGBoost ✅86.50%0.85600.770.48
🏆 Best Model: XGBoost
🔍 Key Findings
Model comparison:
FindingDetailXGBoost winsHighest AUC-ROC (0.8560) and Accuracy (86.50%)Logistic Regression strugglesRecall of only 14% — misses most actual churnersRandom Forest vs XGBoostNearly identical accuracy, XGBoost slightly better at separating churners
Feature Importance — what drives churn prediction:
RankRandom ForestXGBoost1Age (23.9%)NumOfProducts (29.7%)2EstimatedSalary (14.7%)IsActiveMember (22.3%)3CreditScore (14.4%)Age (17.4%)4Balance (14.1%)Geography (7.8%)5NumOfProducts (12.9%)Balance (5.7%)
Weakest predictors: HasCrCard and Gender — lowest importance scores across both models.
Confusion Matrix highlight (XGBoost on 2,000 test samples):

Correctly identified churners (TP): 196
Churners missed (FN): 211 — still room to improve with class balancing techniques like SMOTE

---

## 🔵 Module 3 — Customer Segmentation *(Coming Soon)*

Planned approach:
- K-Means Clustering
- Segment profiling (high-risk vs loyal vs dormant)
- Retention strategy recommendations per segment

---

## 📈 Module 4 — Visualization & Dashboard *(Coming Soon)*

Planned approach:
- Interactive churn dashboard
- Charts by Geography, Age Group, Product Count
- Built with Matplotlib / Seaborn / Plotly

---

## 📝 Module 5 — Business Insights Report *(Coming Soon)*

Final report summarizing all findings, models, and recommendations for a non-technical audience.

---

## 🛠️ Tools & Libraries

```
Python 3.x
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
plotly
openpyxl
google colab
```

---

## 🚀 How to Run

```bash
# 1. Clone the repo
git clone https://github.com/your-username/bank-customer-churn.git
cd bank-customer-churn

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run EDA
python 1_exploratory_data_analysis/eda.py
```

---

## 📁 Repository Structure

```
bank-customer-churn/
│
├── data/
│   └── churn_bank_customers.xlsx
│
├── 1_exploratory_data_analysis/
│   └── eda.py
│
├── 2_churn_prediction/
│   └── (coming soon)
│
├── 3_customer_segmentation/
│   └── (coming soon)
│
├── 4_dashboard/
│   └── (coming soon)
│
├── 5_business_insights_report/
│   └── (coming soon)
│
└── README.md
```

---

## 👤 Author

Made by **[Norman Khalis]**
- GitHub: [@NormanKhalis]([https://github.com/your-username](https://github.com/NormanKhalis))
- LinkedIn: [NormanKhalis](www.linkedin.com/in/normankhalis)
