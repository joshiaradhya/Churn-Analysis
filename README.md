# Churn Analysis & Customer Intelligence

A SQL + Python (pandas) project that analyzes customer churn for a subscription-based business. Raw data lives in a SQLite database and is cleaned, merged, and transformed into a single analytical dataset, from which key churn and revenue metrics are calculated.

## What this project does

- Loads customer, subscription, and support data from a SQLite database (`customer_churn.db`)
- Cleans and standardizes each table (renaming columns, dropping unused fields, fixing missing values, correcting data types)
- Engineers features such as `churn_flag` and `complaint_count`
- Merges everything into a single customer-level dataset
- Calculates key business metrics, including:
  - Churn rate & retention rate
  - Churn rate by plan type, state, and subscription source
  - ARPU (Average Revenue Per User)
  - Average customer tenure
  - Revenue at risk from churned customers
  - Escalation rate
  - Average complaints per user
- Exports the final merged dataset to CSV for further analysis or dashboarding

## Data Sources

The project reads from three tables inside `customer_churn.db`:

| Table | Description |
|---|---|
| `db_customer` | Customer demographics — ID, name, country, state, gender, DOB, etc. |
| `db_subscription` | Subscription details — plan type, contract type, dates, charges, cancellation info |
| `db_support` | Support/complaint history — complaint dates, escalations, CSAT scores |

## Data Cleaning Steps

- Renamed `name` → `customer_name` for clarity
- Dropped unused columns (`interests`, `pincode`, `col_1`, `comment`)
- Converted date columns (`dob`, subscription/cancellation dates) to proper datetime types
- Standardized inconsistent category values (e.g. gender labels)
- Filled missing `country` values using each customer's `state` as a lookup key

## Feature Engineering

- **`churn_flag`** — binary flag derived from whether a customer has a `cancellation_date`
- **`complaint_count`** — number of support complaints per customer, computed from `db_support` and merged in before deduplication
- Support records deduplicated to one row per customer (most recent complaint) prior to the final merge

## Getting Started

### Prerequisites

```bash
pip install numpy pandas matplotlib seaborn
```

### Running the analysis

1. Make sure `customer_churn.db` is in the same directory as the notebook.
2. Open `churn_analysis.ipynb` in Jupyter.
3. Run all cells top to bottom (**Kernel → Restart & Run All** is recommended to avoid stale-state issues).
4. The final merged dataset is exported to `exported_churn_data.csv`.

## Key Metrics Produced

| Metric | Description |
|---|---|
| Churn Rate | % of customers who have cancelled |
| Retention Rate | % of customers still active |
| Churn by Plan Type | Churn rate broken down by Basic/Standard/Premium |
| Churn by State | Churn rate, user count, and revenue by state |
| Churn by Subscription Source | Churn rate by Organic/Paid/Referral |
| ARPU | Average monthly revenue per user |
| Average Tenure | Average customer lifetime in days |
| Revenue at Risk | Total monthly revenue tied to churned customers |
| Escalation Rate | % of support interactions that were escalated |
| Avg. Complaints per User | Average number of complaints across all customers |

## Tech Stack

- Python 3
- pandas / numpy
- matplotlib / seaborn
- SQLite3

## Repository Structure

```
Churn-Analysis/
├── churn_analysis.ipynb      # Main analysis notebook
├── customer_churn.db         # SQLite database (not included — see note below)
├── exported_churn_data.csv   # Output: cleaned & merged dataset
└── README.md
```

> **Note:** `customer_churn.db` should be added to the repo (or `.gitignore`'d if it contains sensitive data) for the notebook to run out of the box.
