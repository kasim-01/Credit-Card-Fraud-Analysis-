# Credit Card Fraud Risk Analysis Dashboard (Power BI)

An interactive Power BI dashboard that analyses 1,000 credit card transactions to show **how much fraud is happening, what kind, where, and on which merchants and categories**, so a risk team can see exposure at a glance.

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-blue)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-green)
![Domain](https://img.shields.io/badge/Domain-Banking%20%26%20Fraud%20Analytics-red)

> `Credit-Card-Fraud-Analysis-/blob/main/dashboard%20image.png`

---

## Business Problem

Card fraud costs banks and customers money, and fraud teams need quick answers to questions like:

- What share of transactions is fraudulent, and what is the money at stake?
- Which fraud type is the most common?
- Which states, merchants and spending categories are most affected?
- How is fraud trending over time?
- How are transactions distributed across risk levels?

This dashboard answers these in one page with interactive filters.

---

## Dataset

| Item | Detail |
|------|--------|
| Source file | `Credit Card Fraud Risk Analysis.csv` |
| Records | 1,000 transactions |
| Period | 16 Dec 2023 to 15 Dec 2024 |
| Currency | INR |
| Columns | 14 |

| Column | Description |
|--------|-------------|
| Transaction ID | Unique transaction identifier |
| Customer Name | Cardholder |
| Merchant Name | Merchant where the transaction occurred |
| Transaction Date | Date of transaction |
| Transaction Amount (INR) | Value of the transaction |
| Fraud Risk | Risk band: Low, Medium, High, Critical |
| Fraud Type | Card Not Present, Card Skimming, Identity Theft, Account Takeover, Phishing |
| State | Indian state of the transaction |
| Card Type | Visa, Mastercard, Amex, Rupay |
| Bank | Issuing bank |
| IsFraud | 1 = fraudulent, 0 = genuine |
| Fraud Score | Numeric fraud score |
| Transaction Category | E-commerce, Electronics, Groceries, Apparel, Food Delivery, Transportation |
| Merchant Location | Location of the merchant |

---

## Approach

1. **Data import (Power Query):** loaded the CSV, promoted headers and set data types (integers, text, dates).
2. **Data model:** a single flat fact table, with an auto-generated date hierarchy on `Transaction Date`.
3. **DAX measures:** built KPIs for fraud volume, fraud amount, fraud rate, critical-risk share and the top fraud type.
4. **Dashboard design:** KPI cards, charts for distribution and trend, and slicers for interactive filtering.

---

## DAX Measures

```DAX
Total Transaction Amount =
SUM ( 'Credit Card Fraud Risk Analysis'[Transaction Amount (INR)] )

Fraudulent Transactions =
CALCULATE ( COUNTROWS ( 'Credit Card Fraud Risk Analysis' ),
            'Credit Card Fraud Risk Analysis'[IsFraud] = 1 )

Fraudulent Transaction Amount =
CALCULATE ( [Total Transaction Amount],
            'Credit Card Fraud Risk Analysis'[IsFraud] = 1 )

Fraud Rate % =
DIVIDE ( [Fraudulent Transactions],
         COUNTROWS ( 'Credit Card Fraud Risk Analysis' ), 0 ) * 100

Critical Risk Transactions % =
DIVIDE (
    CALCULATE ( COUNTROWS ( 'Credit Card Fraud Risk Analysis' ),
                'Credit Card Fraud Risk Analysis'[Fraud Risk] = "Critical" ),
    COUNTROWS ( 'Credit Card Fraud Risk Analysis' ), 0 ) * 100

Top Fraud Type =
VAR TopType =
    TOPN (
        1,
        SUMMARIZE (
            'Credit Card Fraud Risk Analysis',
            'Credit Card Fraud Risk Analysis'[Fraud Type],
            "Fraudulent Count",
                CALCULATE ( COUNTROWS ( 'Credit Card Fraud Risk Analysis' ),
                            'Credit Card Fraud Risk Analysis'[IsFraud] = 1 )
        ),
        [Fraudulent Count], DESC
    )
RETURN
    MAXX ( TopType, 'Credit Card Fraud Risk Analysis'[Fraud Type] )
```

**Concepts used:** `CALCULATE` with filter context, `DIVIDE` for safe division, `VAR`/`RETURN`, `TOPN`, `SUMMARIZE`, `MAXX`, iterator and table functions.

---

## Dashboard Contents

| Visual | Purpose |
|--------|---------|
| KPI cards | Fraudulent transactions, fraudulent amount, fraud rate %, critical-risk %, top fraud type |
| Bar chart | Transaction amount by fraud type and transaction category |
| Donut chart | Transaction amount split by fraud risk level |
| Column chart | Fraudulent transactions by state |
| Line chart | Fraudulent transactions over time |
| Slicers | State, Merchant Name, Fraud Type |

---

## Key Findings

Figures from the data in this report:

- **1,000 transactions** worth about **₹1.22 crore** (₹12.23M) in total
- **286 fraudulent transactions**, a **28.6% fraud rate**
- Fraudulent transactions account for about **₹33.1 lakh** (₹3.31M)
- **Card Not Present** is the most frequent fraud type (72 cases), followed by Identity Theft (64) and Card Skimming (54)
- **10.7%** of transactions are rated **Critical** risk

---

## Skills Demonstrated

- Power Query data cleaning and type handling
- DAX measure design (filter context, variables, table functions)
- KPI and dashboard design for a risk/fraud use case
- Interactive filtering with slicers
- Translating business questions into visuals

---

## Repository Structure

```
├── credit_card_fraud_analysis.pbix
├── Credit Card Fraud Risk Analysis.csv
├── README.md
└── docs/
    └── dashboard.png
```

## How to Use

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
2. Open `credit_card_fraud_analysis.pbix`
3. If prompted, point the data source to your local copy of the CSV (Transform data → Data source settings)
4. Use the slicers to explore by state, merchant and fraud type

## Future Improvements

- Add drill-through pages by customer, bank and card type
- Add fraud-rate-by-category and month-over-month trend measures
- Build a risk-scoring view using `Fraud Score` thresholds
- Publish to Power BI Service with scheduled refresh

---

## Author

**Kasim**
B.Tech CSE (2027) | Aspiring Data Analyst | SQL · Excel · Python · Power BI

[LinkedIn](https://www.linkedin.com/in/kasim-abbasi-889b61308) · [GitHub](https://github.com/kasim-01)
