# Trend Analysis – Banking Transactions Dashboard

## Project Overview

This project analyzes historical banking transaction data to identify patterns in transaction behavior, cash flow movements, and temporal trends.
The objective of the project was to explore historical transaction data, identify meaningful patterns, and build a Power BI dashboard that supports future forecasting analysis.

The dataset contains transaction records between 2014 and 2020, including transaction codes, transaction amounts, and effective transaction dates. The analysis focuses on understanding how different transaction types contribute to overall financial activity and how transaction behavior varies across time.


## Project Goals

The project focuses on answering the following analytical questions.

1. How do daily credit and debit transaction patterns contribute to predictable periods of cash inflow and outflow within a month?

2. Which transaction codes exhibit stable monthly patterns that can support forecasting models?

3. Are weekday and weekend transaction behaviors significantly different?

4. What is the year-over-year change in transaction volume and net transaction amount?

These questions were designed to uncover patterns that can later support predictive forecasting models.


## Dataset

The dataset consists of 67,672 transaction records with the following key attributes.

- Transaction Code  
- Transaction Description  
- Transaction Amount  
- Transaction Effective Date  

The dataset showed high data quality, with no missing values and only four zero-value transactions.

Transaction amounts include both positive and negative values, representing cash inflows and outflows.


## Data Preparation

Data transformation and preparation were performed before analysis.


### Transaction Code Grouping

Approximately 90 transaction codes were provided in the reference table. Codes that did not appear in the dataset or contained no transactions were removed.

The remaining codes were grouped into four analytical categories.

- Positive Cash Moves  
- Negative Cash Moves  
- ATM Transactions  
- Service Fees  

This grouping simplifies analysis and allows trends to be evaluated at a category level rather than by individual codes.


### Date Feature Engineering

Additional date fields were created to support time-based analysis.

    Year = YEAR(HistoricalTransactions[Report_TransactionEffectiveDate])

    Quarter = QUARTER(HistoricalTransactions[Report_TransactionEffectiveDate])

    Month = FORMAT(HistoricalTransactions[Report_TransactionEffectiveDate], "MMM")

    Day = DAY(HistoricalTransactions[Report_TransactionEffectiveDate])

    Weekdays = FORMAT(HistoricalTransactions[Report_TransactionEffectiveDate], "dddd")

    Weekend or Weekday =
    IF(WEEKDAY(HistoricalTransactions[Report_TransactionEffectiveDate],2)>5,
    "Weekend","Weekday")

    End of Month =
    IF(HistoricalTransactions[Report_TransactionEffectiveDate] =
    EOMONTH(HistoricalTransactions[Report_TransactionEffectiveDate],0),
    "End of Month","Non-EOM")


## Data Model

A one-to-many relationship was created between the transaction grouping table and the transaction dataset.

    TranGroups[TranCode] 1 → * HistoricalTransactions[TranCode]

## Key DAX Measures

Several KPIs were created using DAX to analyze transaction behavior.


### Net Transaction Amount

    Net Amount = SUM(HistoricalTransactions[TransactionAmount])


### Transaction Count

    Transaction_Count = COUNTROWS(HistoricalTransactions)


### Absolute Transaction Value

    Trans_Absolute_Amount =
    SUMX(HistoricalTransactions,
    ABS(HistoricalTransactions[TransactionAmount]))


## Year-Over-Year Metrics

Year-over-year measures were used to analyze long-term trends.

    YoY Net Growth $ =
    SUM(HistoricalTransactions[TransactionAmount])
    -
    CALCULATE(
    SUM(HistoricalTransactions[TransactionAmount]),
    SAMEPERIODLASTYEAR(HistoricalTransactions[Report_TransactionEffectiveDate])
    )

    YoY Volume Growth =
    [Transaction_Count]
    -
    CALCULATE(
    [Transaction_Count],
    SAMEPERIODLASTYEAR(HistoricalTransactions[Report_TransactionEffectiveDate])
    )


## Dashboard Design

The final deliverable was a single-page Power BI dashboard designed to highlight the most important transaction trends.

The dashboard focuses on the following areas.

- Transaction behavior by category  
- Daily credit versus debit activity  
- Settlement transaction patterns  
- Weekday versus weekend transaction differences  

Multiple exploratory charts were created during development, but only the most informative visualizations were included in the final dashboard.


## Key Insights


### Negative Cash Moves Dominate Transaction Value

Most transaction activity comes from cash outflows, such as bill payments and recurring deductions.

This suggests that operational spending drives the majority of financial activity, while service fees contribute only a small portion of total transaction value.

Implication: Financial operations should prioritize cash flow management and liquidity planning.


### Electronic Transfers Are the Most Consistent Transaction Stream

Electronic or AFT transactions occur more frequently than physical ATM settlement transactions.

These transaction types also show parallel movement patterns, suggesting they are driven by similar financial cycles.

Implication: These transaction streams are stable predictors for forecasting models.


### End-of-Month Debit Activity Spikes

Debit transaction volume increases significantly near the end of the month, while credits decrease.

This likely reflects bill payments, rent payments, payroll processing, and supplier payments.

Implication: Forecast models should incorporate end-of-month seasonality.


### Weekday Transactions Are Significantly Higher

Transaction activity is much higher during business days than on weekends.

Weekend transactions contribute very little to overall transaction volume.

Implication: Forecasting models should treat weekday and weekend behavior differently.


## Tools Used

- Power BI  
- DAX  
- Data Modeling  
- Data Profiling  
- Time-Series Trend Analysis  


## Skills Demonstrated

- Data cleaning and transformation  
- Feature engineering  
- Power BI dashboard development  
- DAX calculations  
- Time-based trend analysis  
- Business insight generation  
