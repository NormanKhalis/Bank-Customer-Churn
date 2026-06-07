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
| 3 | Customer Segmentation | ✅ Complete |
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


**File:** `2_churn_prediction/churn_prediction.py`

**Steps covered:**
- Step 1: Load data
- Step 2: Preprocessing (drop irrelevant columns, encode categoricals, train/test split 80/20, feature scaling)
- Step 3: Train 3 classifiers — Logistic Regression, Random Forest, XGBoost
- Step 4: Evaluate models (Accuracy, AUC-ROC, Confusion Matrix, Classification Report)
- Step 5: Visualizations (Confusion Matrices, ROC Curves, Model Comparison, Feature Importance)
- Step 6: Final summary & best model selection

### 🔍 Key Findings

**Model performance:**

| Model | Accuracy | AUC-ROC | Churned Recall |
|---|---|---|---|
| Logistic Regression | 80.50% | 0.7710 | 0.14 |
| Random Forest | 86.40% | 0.8464 | 0.46 |
| XGBoost | 86.50% | 0.8560 | 0.48 |

**Top features by importance (XGBoost):** NumOfProducts (29.7%), IsActiveMember (22.3%), Age (17.4%) — consistent with EDA findings.

**Best model: XGBoost** — highest AUC-ROC (0.8560) and accuracy (86.50%).

---

## 🔵 Module 3 — Customer Segmentation

## 📊 Dataset

**File:** `churn_bank_customers.xlsx`  
**Rows:** 10,000 customers &nbsp;|&nbsp; **Columns:** 14

| Column | Type | Description |
|---|---|---|
| `CustomerId` | int | Unique customer identifier |
| `Surname` | str | Customer surname |
| `CreditScore` | int | Credit score (350–850) |
| `Geography` | str | Country: France, Germany, Spain |
| `Gender` | str | Male / Female |
| `Age` | int | Customer age |
| `Tenure` | int | Years with the bank (0–10) |
| `Balance` | float | Account balance |
| `NumOfProducts` | int | Number of bank products held (1–4) |
| `HasCrCard` | int | Has credit card: 1 = Yes, 0 = No |
| `IsActiveMember` | int | Active member: 1 = Yes, 0 = No |
| `EstimatedSalary` | float | Annual estimated salary |
| `Exited` | int | **Target** — churned: 1 = Yes, 0 = No |

---

## 🔬 Methodology

### Step 1 — Exploratory Data Analysis
- Overall churn rate: **20.4%**
- Key churn drivers identified before clustering:

| Factor | Churn Rate |
|---|---|
| NumOfProducts = 1 | 27.7% |
| NumOfProducts = 2 | 7.6% |
| NumOfProducts = 3 | 82.7% |
| NumOfProducts = 4 | 100.0% |
| IsActiveMember = 0 | 26.9% |
| IsActiveMember = 1 | 14.3% |

### Step 2 — Feature Engineering
- Encoded `Gender` → `Gender_enc` (Male = 1)
- Encoded `Geography` → `Geo_Germany`, `Geo_Spain` (one-hot)
- **11 features used for clustering** (`Exited` excluded to avoid data leakage)
- All features standardised with `StandardScaler`

### Step 3 — K-Means Clustering
- Elbow method tested k = 2 through 9
- **k = 4 selected** — clear inflection point in the inertia curve

| k | Inertia |
|---|---|
| 2 | 97,252 |
| 3 | 88,349 |
| **4** | **82,586** ← chosen |
| 5 | 79,247 |
| 6 | 76,405 |

### Step 4 — Segment Labelling
Business-rule labels applied on top of cluster outputs:

| Rule | Segment |
|---|---|
| `Balance == 0` | **Dormant** |
| `IsActiveMember == 0` OR `NumOfProducts >= 3` | **High-Risk** |
| `IsActiveMember == 1` AND `NumOfProducts == 2` AND `CreditScore >= 650` | **Loyal** |
| Everything else | **Moderate-Risk** |

### Step 5 — PCA Visualisation
- 2-component PCA for scatter plot (explains ~25.4% of variance)
- Used for visual separation confirmation only — not for clustering

---

## 📈 Key Findings

### Segment Overview

| Segment | Count | Share | Churn Rate |
|---|---|---|---|
| 🔴 High-Risk | 3,191 | 31.9% | **33.0%** |
| ⚫ Dormant | 3,617 | 36.2% | 14.0% |
| 🔵 Moderate-Risk | 2,635 | 26.4% | 16.0% |
| 🟢 Loyal | 557 | 5.6% | **9.0%** |

### Segment Profiles

| Metric | High-Risk | Dormant | Moderate-Risk | Loyal |
|---|---|---|---|---|
| Avg Age | 38.7 | 38.4 | 39.9 | 38.8 |
| Avg Balance | $120,508 | $0 | $119,119 | $119,282 |
| Avg Credit Score | 648 | 649 | 638 | **730** |
| Avg Products | 1.44 | 1.78 | 1.19 | **2.00** |
| Active Rate | **3%** | 52% | **100%** | **100%** |
| Avg Salary | $101,675 | $98,984 | $98,802 | $104,293 |
| Avg Tenure | 5.1 yrs | 5.1 yrs | 4.9 yrs | 4.8 yrs |

### Critical Observations

**1. Inactivity is the strongest churn signal**
High-Risk customers have only a 3% active member rate, vs 100% for Loyal and Moderate-Risk. When a customer goes inactive, churn probability more than doubles.

**2. Product count is non-linear**
Customers with 2 products have the lowest churn (7.6%). Customers with 3+ products have catastrophic churn (83–100%), suggesting product overload and poor cross-sell targeting.

**3. Dormant segment is a hidden risk**
36% of customers hold zero balance despite earning ~$99K on average. They are not actively churning yet, but represent significant revenue leakage — their money is simply held elsewhere.

**4. Loyal segment is tiny but powerful**
Only 5.6% of customers qualify as Loyal. This group has the highest credit score (730), all hold exactly 2 products, and churn at just 9%. Growing this segment is the highest-ROI retention play.

**5. Tenure does not protect against churn**
All four segments have similar average tenure (~5 years), meaning long-standing customers are not immune. Engagement and product fit matter more than relationship length.

---

## 🎯 Retention Strategies

### 🔴 High-Risk (n=3,191 | Churn 33%)
> Inactive members and over-productised customers — act immediately.

1. **Immediate outreach** — personal banker call within 48h for inactive accounts
2. **Re-engagement offer** — fee waiver or bonus interest rate for 3 months
3. **Product simplification** — review 3–4 product customers and consolidate to 2 optimal products
4. **Loyalty incentives** — cashback or points programme to rebuild engagement
5. **Predictive trigger** — automate alert when `IsActiveMember` flips to 0

---

### ⚫ Dormant (n=3,617 | Churn 14%)
> Zero-balance customers who earn well but bank elsewhere.

1. **Deposit incentive** — promotional interest rate (e.g. 5% for 6 months) to fund accounts
2. **Salary crediting campaign** — payroll deposit bonus to create recurring engagement
3. **Mobile push** — personalised nudges showing savings goals and milestones
4. **Cross-sell** — entry-level investment product aligned to salary bracket
5. **Win-back email series** — "Your account is ready when you are"

---

### 🔵 Moderate-Risk (n=2,635 | Churn 16%)
> Engaged, single-product customers — one step from Loyal.

1. **Product upsell** — introduce a second product (savings account, credit card) to deepen relationship
2. **Financial health check** — free annual review to build trust and stickiness
3. **Milestone rewards** — tenure-based perks at 3, 5, and 10-year marks
4. **Digital engagement** — enrol in budgeting and analytics features in the mobile app
5. **Referral programme** — reward existing customers for bringing new accounts

---

### 🟢 Loyal (n=557 | Churn 9%)
> Active, multi-product, high credit-score customers — protect and grow.

1. **VIP programme** — dedicated relationship manager and priority service
2. **Exclusive rates** — preferential mortgage/loan rates for long-term retention
3. **Advocacy** — referral bonuses; they are the best brand ambassadors
4. **Premium upgrade** — offer premium/black card tier with added benefits
5. **Early access** — beta features, new products, and exclusive events


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
