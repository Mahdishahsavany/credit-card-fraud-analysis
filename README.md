# Credit Card Fraud Detection: Exploratory Data Analysis

[فارسی / Persian version](README.fa.md)

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458)

Final project of a Data Analysis course. The goal is to explore about **1.85 million credit card transactions** and find the patterns that separate **fraudulent** transactions from **normal** ones: *when* fraud happens, *where* (which purchase categories), *who* is affected, and *how much* money is involved.

The whole analysis lives in one Jupyter notebook: [`fraud_detection_final.ipynb`](fraud_detection_final.ipynb). The notebook text (explanations and analyses) is written in **Persian (right-to-left)**, and the code and plots are standard Python.

---

## Table of Contents

- [Dataset](#dataset)
- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [Methods and Tools](#methods-and-tools)
- [Repository Structure](#repository-structure)
- [How to Run](#how-to-run)
- [Limitations](#limitations)
- [Author](#author)

---

## Dataset

**Credit Card Transactions Fraud Detection Dataset** from Kaggle:
<https://www.kaggle.com/datasets/kartik2112/fraud-detection>

| Item | Value |
|---|---|
| Files | `fraudTrain.csv` and `fraudTest.csv` (same columns, concatenated in the notebook) |
| Transactions | 1,852,394 |
| Period | 2019-01-01 to 2020-12-31 |
| Target column | `is_fraud` (1 = fraud, 0 = normal) |
| Fraud share | 9,651 transactions (**0.521%**), so the data is highly **imbalanced** |
| Nature | **Simulated** data (customers, names, addresses and card numbers are fictional) |

> The dataset is **not included** in this repository (it is too large for GitHub). See [How to Run](#how-to-run).

---

## Project Overview

The notebook is organized into six parts:

| Part | Content |
|---|---|
| **1. Data preparation** | Reading and merging the two files, data types optimization (memory dropped from **595.3 MB to 99.0 MB**, about 83% less), dropping unnecessary columns (names, street, transaction id, unix time), missing values and duplicates check, feature engineering (`hour`, `weekday`, `month`, `age`, `age group`) |
| **2. Univariate analysis** | Each column on its own: distributions, skewness, mean/median/mode, histograms, box plots, outlier detection (3-sigma vs IQR), top categories |
| **3. Bivariate analysis** | Fraud rate by category, hour, weekday, month, gender, age group and state; transaction amount of fraud vs normal; correlation matrix |
| **4. Multivariate analysis** | Fraud rate by category and gender, median amount by hour for fraud vs normal, fraud rate by age group and gender |
| **5. Statistical tests** | Mann-Whitney U test and Kolmogorov-Smirnov test on the transaction amount |
| **6. Conclusion** | Main findings, limitations and a practical recommendation |

Because only 0.52% of transactions are fraudulent, groups are compared using the **fraud rate** (percentage of fraud inside each group) instead of raw counts.

---

## Key Findings

- **Time is the strongest pattern.** The fraud rate is about **2.6%** at 22:00 and 23:00, about **1.3%** from 00:00 to 03:00, and only about **0.1%** during the rest of the day.
- **Amount matters a lot.** The median fraudulent transaction is **390 USD** versus **47 USD** for normal ones. **75.8%** of fraudulent transactions are above 200 USD, compared with **4.3%** of normal ones. `amt` also has the highest correlation with `is_fraud` (0.21).
- **Purchase category.** The highest fraud rates are in `shopping_net` (**1.59%**), `misc_net` (**1.30%**) and `grocery_pos` (**1.26%**); most other categories are below 0.5%.
- **Gender and age.** Men (0.57%) are slightly higher than women (0.48%). Older customers (56+) and the youngest group (<=25) have higher rates than the 36-45 group (the lowest, 0.39%). Differences are small.
- **Weekday and month.** Differences are small: Thursday/Friday are slightly higher, Monday is the lowest; February is the highest month and December the lowest.
- **State.** After excluding states with fewer than 1,000 transactions (their rates are unstable), `AK` has the highest fraud rate (about 1.69%).
- **Statistical tests.** Both the Mann-Whitney U test and the Kolmogorov-Smirnov test (KS statistic = 0.72) reject the hypothesis that fraudulent and normal amounts come from the same distribution (p-value is effectively 0). In practice, a random fraudulent transaction is larger than a random normal one about 83% of the time.
- **Correlation is not always informative.** `hour` has almost zero linear correlation with fraud (0.013) although its effect is strong, because the relationship is not linear.

---

## Methods and Tools

- **Python**: `pandas`, `numpy`, `matplotlib`, `scipy`
- **Data handling**: memory optimization with `category`, `float32`, `int32` and `int8` types; date features with the `.dt` accessor; `groupby` aggregations
- **Descriptive statistics**: mean, median, mode, quartiles, skewness, coefficient of variation
- **Outlier detection**: 3-sigma rule and IQR rule, with a comparison on skewed data
- **Visualization** (matplotlib): histograms, box plots, violin plot, bar and line charts, heatmap of the correlation matrix
- **Statistical testing**: Mann-Whitney U and Kolmogorov-Smirnov (non-parametric tests, chosen because the amount is strongly right-skewed and not normal)

---

## Repository Structure

```
.
├── README.md
├── README.fa.md                 # Persian version of this file
├── fraud_detection_final.ipynb  # the full analysis (with outputs)
├── requirements.txt
└── .gitignore
```

---

## How to Run

1. Clone the repository and install the requirements:

   ```bash
   git clone https://github.com/mahdishahsavany/credit-card-fraud-analysis.git
   cd credit-card-fraud-analysis
   pip install -r requirements.txt
   ```

2. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/kartik2112/fraud-detection) and place `fraudTrain.csv` and `fraudTest.csv` in the **same folder as the notebook**.

3. Start Jupyter and run the notebook from top to bottom:

   ```bash
   jupyter notebook fraud_detection_final.ipynb
   ```

**Notes**

- Reading the full data uses roughly 600 MB of RAM at first; after the optimization step it drops to about 100 MB.
- A recent version of matplotlib (3.10 or newer) is recommended, because the notebook uses `orientation="horizontal"` in `plt.boxplot`.
- The explanations are written right-to-left. For the best display, open the notebook in JupyterLab or VS Code.

---

## Limitations

- The data is **simulated**, so some patterns (for example the sudden step in the number of transactions at 12:00) probably come from the simulation and may not appear in real data.
- The data is **highly imbalanced**, which makes the fraud rate of small groups unstable (for example state `DE`, which has only 9 transactions).
- Monthly results combine two years (2019 and 2020).
- Correlations and group differences describe patterns only; they do **not** prove causation.

---

## Author

**mahdi shahsavany**
GitHub: [@mahdishahsavany](https://github.com/mahdishahsavany) | LinkedIn: [mahdishahsavany](https://www.linkedin.com/in/mahdishahsavany)

*The dataset belongs to its original author on Kaggle. See the dataset page for its terms of use.*
