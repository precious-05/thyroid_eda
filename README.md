#  Exploratory Data Analysis — Thyroid Cancer Risk Dataset

A comprehensive step-by-step EDA on the **Thyroid Cancer Risk** dataset, covering initial exploration, data cleaning, univariate analysis, outlier detection using box plots (IQR method), bivariate analysis, multivariate analysis, class imbalance measurement, and categorical encoding guidance.

---

##  Project Overview

This project analyzes patient-level data to explore factors associated with thyroid cancer risk. The analysis investigates the role of demographics, lifestyle habits, medical history, and clinical lab values (TSH, T3, T4 levels) in determining cancer risk levels — Low, Medium, or High.

---

## 📂 Dataset

**File:** `thyroid_cancer_risk_data.csv`

| Column | Description |
|---|---|
| `Patient_ID` | Unique patient identifier (dropped before analysis) |
| `Age` | Age of the patient |
| `Gender` | Male / Female |
| `Country` | Country of the patient |
| `Ethnicity` | Ethnicity of the patient |
| `Family_History` | Whether the patient has a family history of thyroid cancer (Yes/No) |
| `Radiation_Exposure` | History of radiation exposure (Yes/No) |
| `Iodine_Deficiency` | Whether the patient has iodine deficiency (Yes/No) |
| `Smoking` | Smoking status (Yes/No) |
| `Obesity` | Obesity status (Yes/No) |
| `Diabetes` | Diabetes status (Yes/No) |
| `TSH_Level` | Thyroid Stimulating Hormone level (mIU/L) |
| `T3_Level` | Triiodothyronine hormone level |
| `T4_Level` | Thyroxine hormone level |
| `Nodule_Size` | Size of thyroid nodule (cm) |
| `Thyroid_Cancer_Risk` | Target variable — Low / Medium / High |
| `Diagnosis` | Final diagnosis (Cancer / No Cancer) |

---

## 🔍 Analysis Workflow

### 1. Initial Exploration
- Loaded the dataset and previewed using `head()`, `tail()`, `info()`, `describe()`
- Checked unique values for all columns to understand data diversity
- Dropped `Patient_ID` as it has no analytical value
- Investigated `Nodule_Size == 0.0` rows — concluded they are valid (only 0.09% of data) and medically plausible since lab values (TSH, T3, T4) were within normal ranges

### 2. Data Cleaning
- Confirmed no missing values required major handling
- Validated negative/zero entries in clinical columns
- Kept `Nodule_Size == 0.0` rows as they contribute valid patterns for ML models

---

## 📊 Visualizations

### 🔹 Univariate Analysis — Distribution Plots (Histograms + KDE)

Each numerical column was visualized using **Seaborn histograms with KDE curves** and custom bin ranges. Mean and Median reference lines were added where applicable.

| Plot | What It Shows |
|---|---|
| **Age Distribution** | Patient age spread with mean & median lines; bins from 10–90 |
| **TSH Level Distribution** | TSH spread across 0–11 mIU/L; elevated values indicate hypothyroidism |
| **T3 Level Distribution** | T3 values across 0–4 range; most within normal clinical range |
| **T4 Level Distribution** | T4 spread across 0–12.5; most within normal range |
| **Nodule Size Distribution** | Nodule size from 0–5.5 cm with KDE overlay |

> **Clinical Note:** TSH above 4.0 mIU/L indicates underactive thyroid. TSH above 10 mIU/L is a strong indicator of overt hypothyroidism.

---

### 🔹 Univariate Analysis — Bar Charts (Categorical Columns)

Frequency bar charts were plotted for all categorical features with count labels on each bar.

| Plot | What It Shows |
|---|---|
| **Gender Distribution** | Count of Male vs Female patients |
| **Top 10 Countries** | Countries with highest representation in dataset |
| **Top Ethnicities** | Distribution of ethnic groups |
| **Family History** | Yes/No frequency of family thyroid history |
| **Radiation Exposure** | Proportion of patients with/without radiation history |
| **Iodine Deficiency** | Yes/No breakdown |
| **Smoking Status** | Smoker vs Non-smoker count |
| **Obesity Status** | Obese vs Non-obese count |
| **Diabetes Status** | Diabetic vs Non-diabetic count |
| **Thyroid Cancer Risk** | Count across Low, Medium, High risk classes |
| **Diagnosis Distribution** | Cancer vs No Cancer patient count |

---

### 🔹 Outlier Detection — Box Plots with IQR Method

For each numerical column, outliers were first detected **numerically using the IQR method** (lower bound = Q1 − 1.5×IQR, upper bound = Q3 + 1.5×IQR), then visualized using **annotated Seaborn box plots**.

Each box plot includes:
- **Box** → Q1 to Q3 (Interquartile Range)
- **Red line** → Median (Q2)
- **Purple dashed lines** → Q1 and Q3 markers
- **Green dashed lines** → Lower and Upper Whiskers
- **Dots beyond whiskers** → Outliers

| Box Plot | Color | Key Observations |
|---|---|---|
| **Age** | Orange (`#EE925D`) | No significant outliers; uniform spread |
| **TSH Level** | Blue (`#6EB5FF`) | Some elevated values indicating hypothyroidism |
| **T3 Level** | Mauve (`#BB96B2`) | Mostly within normal biological range |
| **T4 Level** | Teal (`#69D0C4`) | Relatively symmetric distribution |
| **Nodule Size** | Peach (`#FCA373`) | Some patients have zero nodule size (validated as legitimate) |

---

### 🔹 Class Imbalance Check

The distribution of `Thyroid_Cancer_Risk` was measured using:
- **Imbalance Ratio** = Majority class count / Minority class count
- **Percentage distribution** per class using `value_counts(normalize=True)`
- **Bar chart (%)** showing proportion of Low, Medium, and High risk patients

> **Finding:** The High risk class is underrepresented. This class imbalance could cause ML models to ignore high-risk patients and predict mostly Low or Medium risk — making it critical to address before modeling (e.g., using SMOTE or `class_weight='balanced'`).

---

### 🔹 Bivariate Analysis — Grouped Count Plots

Each categorical feature was plotted against `Thyroid_Cancer_Risk` using **Seaborn countplots with `hue`**, breaking down risk levels (Low / Medium / High) within each category. Count labels were added on top of every bar.

| Plot | Question Answered |
|---|---|
| **Gender vs Cancer Risk** | Which gender has more high-risk patients? |
| **Country vs Cancer Risk** | Which countries have higher risk concentrations? |
| **Ethnicity vs Cancer Risk** | Risk distribution across ethnic groups |
| **Family History vs Cancer Risk** | Does family history correlate with higher risk? |
| **Radiation Exposure vs Cancer Risk** | Impact of radiation on risk level |
| **Iodine Deficiency vs Cancer Risk** | Does iodine deficiency elevate risk? |
| **Smoking vs Cancer Risk** | Risk level distribution among smokers vs non-smokers |
| **Obesity vs Cancer Risk** | Obese vs non-obese risk comparison |
| **Diabetes vs Cancer Risk** | Risk level breakdown among diabetic patients |
| **Diagnosis vs Cancer Risk** | How final diagnosis aligns with risk levels |

---

### 🔹 Multivariate Analysis

More complex relationships were explored by combining 3+ variables:

| Plot | Analysis | Chart Type |
|---|---|---|
| **Age & Gender vs High Risk** | Which age group among males and females has the most high-risk patients? | Line Plot |
| **Radiation + High Risk vs Diabetes** | Among high-risk, radiation-exposed patients — how many also had diabetes? | Bar Chart |
| **Radiation + Medium Risk vs Diabetes** | Same analysis for medium-risk patients | Bar Chart |
| **Diabetes + Obesity vs Gender vs Risk** | Among diabetic and obese individuals, does gender influence being high-risk? | Stacked Bar Chart |
| **High Risk Smokers by Gender** | Is high thyroid cancer risk more prevalent among female or male smokers? | Bar Chart |
| **Iodine Deficiency vs TSH & Risk Distribution** | Average TSH levels and risk distribution among iodine-deficient patients | Bar Chart |
| **Low TSH + High Risk → Top Diagnoses** | Most common diagnoses among patients with low TSH and high cancer risk | Bar Chart |

---

## 🧹 Encoding Categorical Features

The notebook concludes with a guide on preparing categorical columns for ML modeling:

- **Ordinal Encoding** (via `OrdinalEncoder`) — for columns with a natural order, e.g., `Thyroid_Cancer_Risk` (Low → Medium → High)
- **Mean vs Median comparison** — used to detect skewness and decide on imputation strategy
- **Class imbalance handling** — recommends SMOTE, undersampling, or `class_weight='balanced'` when ratio > 3

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python | Core programming language |
| Pandas | Data manipulation and cleaning |
| NumPy | Numerical operations |
| Matplotlib | Base plotting library |
| Seaborn | Statistical data visualization |
| Scikit-learn | Encoding reference (OrdinalEncoder) |

---

## 🚀 How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/thyroid-cancer-eda.git
   cd thyroid-cancer-eda
   ```

2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```

3. Place the dataset `thyroid_cancer_risk_data.csv` in the project root.

4. Open and run the notebook:
   ```bash
   jupyter notebook thyroid_eda.ipynb
   ```

---

## 👤 Author

**Alina Liaquat**
🔗 [LinkedIn](https://www.linkedin.com/in/alina-liaquat-779347325)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
