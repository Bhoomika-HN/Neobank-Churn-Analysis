# NeoBank India — Customer Churn Analysis

A end-to-end data analytics project analysing customer churn for a mid-sized Indian neobank. Covers SQL-based data extraction, real-world data cleaning challenges, feature engineering, and exploratory data analysis — culminating in actionable business recommendations for the retention team.

---

## Key Finding

> **Churn at NeoBank is driven by behavioural disengagement, not financial stress.**
>
> `days_since_last_transaction` has a **0.87 correlation** with churn — the strongest predictor in the dataset. Credit score and average balance show near-zero correlation (−0.04). Inactive customers churn at **59%** vs only **0.8%** for Active customers — a 74x difference.

---

## Project Structure

```
neobank-churn-analysis/
│
├── 01_Data_Cleaning.ipynb      # SQL extraction, cleaning, outlier handling, export
├── 02_EDA.ipynb                # Feature engineering, univariate analysis, churn analysis
│
├── requirements.txt            # Python dependencies
└── README.md
```

> **Note on Data:** The SQLite database (`neobank_churn.db`) is synthetically generated to simulate realistic banking data quality issues — inconsistent encodings, mixed formats, outliers, and missing values. All customer records are fictional and created for portfolio/educational purposes only.

---

## Notebooks

### 01 — Data Extraction & Cleaning

Extracts data from a normalised 6-table SQLite database via a single SQL JOIN, then resolves a comprehensive set of real-world data quality issues:

| Issue | Columns Affected | Action |
|---|---|---|
| Duplicate records (exact + partial) | All | Dropped exact; aggregated `support_calls` for partial duplicates |
| Inconsistent categorical encodings | `gender`, `city_tier`, `occupation`, `account_type`, `digital_engagement`, `churn` | Standardised all variants to canonical values |
| Non-numeric formats | `avg_balance` (₹ symbol), `days_since_last_transaction` (text units) | Stripped and converted |
| Invalid values | `age` (= 0), `credit_score` (outside 300–850 bureau range) | Replaced with occupation-group median |
| Extreme outlier | `avg_balance` (value of 99,999,999) | Detected via distribution analysis; imputed |
| Structurally missing values | `credit_score`, `avg_balance` | Segment-level group median imputation |

### 02 — Feature Engineering & EDA

Engineers 7 new features from raw columns, then systematically analyses churn across numerical and categorical dimensions.

**Engineered Features:**

| Feature | Based On | Purpose |
|---|---|---|
| `engagement_score` | Transactions, app logins, recency | Composite behavioural activity score (0–3) |
| `customer_status` | `engagement_score` | Active / At Risk / Inactive classification |
| `tenure_segment` | `tenure_months` | New / Developing / Established / Loyal |
| `age_group` | `age` | Life-stage grouping |
| `balance_segment` | `avg_balance` | Financial value tier |
| `credit_risk_category` | `credit_score` | CIBIL-aligned credit health band |
| `product_profile` | All product columns + financials | 9-category relationship type |

**Statistical Methods Used:** Pearson correlation, chi-square test of independence, Cramér's V effect size

---

## Key Findings

1. **Churn is behavioural, not financial** — `days_since_last_transaction` (r = 0.87) is the dominant churn predictor; financial metrics show near-zero correlation
2. **Engagement score is a powerful early warning signal** — 74x churn difference between Active and Inactive customers
3. **Product depth drives retention** — Minimal Relationship customers (1 product) churn at 21% vs 1.7% for Stable Homeowners
4. **Stressed Borrowers are active but not loyal** — high transaction frequency is driven by loan EMI obligations, not genuine engagement
5. **Metro is the highest-risk market** — highest customer count but also highest churn rate

---

## Business Recommendations

| Priority | Recommendation |
|---|---|
| 🔴 High | Build a weekly behavioural early warning system monitoring `engagement_score` and `days_since_last_transaction` |
| 🔴 High | Launch targeted re-engagement campaign for Inactive customers (59% churn rate) |
| 🟡 Medium | Cross-sell a second product to single-product customers at the 3–6 month tenure mark |
| 🟡 Medium | Develop post-loan retention strategy for Stressed Borrowers before loan completion |
| 🟢 Low | Prioritise retention spend in Metro — largest segment, highest churn |

---

## Setup

```bash
git clone https://github.com/your-username/neobank-churn-analysis.git
cd neobank-churn-analysis
pip install -r requirements.txt
jupyter notebook
```

Open `01_Data_Cleaning.ipynb` first, then `02_EDA.ipynb`.

> The notebooks reference `neobank_churn.db` (for Part 1) and `neobank_cleaned.csv` (output of Part 1, input to Part 2). The database file is not included in the repo as the data is synthetic — the cleaning notebook documents every transformation applied.

---

## Tech Stack

- **Python 3.10+**
- **pandas** — data manipulation and cleaning
- **NumPy** — numerical operations
- **Matplotlib / Seaborn** — visualisation
- **SciPy** — chi-square tests, statistical analysis
- **SQLite3** — database extraction

---

## Author
Bhoomika H N
https://github.com/Bhoomika-HN
www.linkedin.com/in/bhoomika-hn

