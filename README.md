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


## Data Model

A one-to-many (1:*) relationship exists between TranGroups[TranCode] and HistoricalTransactions[TranCode]. It allows category-based filters to propagate correctly.
    
<img width="350" height="300" alt="Data Model" src="https://github.com/user-attachments/assets/db4d2a52-e2e4-4a70-be23-92ea331ab3d9" />

## Dashboard Design

The final deliverable was a single-page Power BI dashboard designed to highlight the most important transaction trends.

The dashboard focuses on the following areas.

- Transaction behavior by category  
- Daily credit versus debit activity  
- Settlement transaction patterns  
- Weekday versus weekend transaction differences  

Multiple exploratory charts were created during development, but only the most informative visualizations were included in the final dashboard.

<img width="1235" height="696" alt="Main Dashboard" src="https://github.com/user-attachments/assets/58090b7f-cbba-4035-b352-6859865db555" />

## Key Insights

### Chart 1: Transaction Amount ($) by Category

Interpretation:
The dominance of Negative Cash Moves indicates that outflows occur more frequently and regularly, reflecting operational spending, bill payments, and recurring deductions. ATM activity also represents a significant portion of transaction behavior, though it does not necessarily translate into net cash creation. Because service fees are such a small slice, revenue from fees is unlikely to drive major changes in total transaction value.

Why it matters:
This distinction highlights the difference between transaction activity and net cash impact, reinforcing the importance of analyzing categories separately rather than relying solely on total amounts. From a business perspective, cash management and ATM replenishment should be prioritized over fee-related strategies, since that is where most dollars move.


### Chart 2: Transaction Volume by AFT (ET) and ATM (TM) Settlement

Interpretation:
Customers rely more on electronic/AFT transfers (ET) than physical ATM settlements (TM). The parallel movement suggests that these transaction types are influenced by shared underlying drivers, such as customer payment cycles, settlement timing, or institutional processing schedules.

Why it matters:
Because these transaction codes behave consistently over time, they represent stable and predictable transaction streams, making them strong candidates for inclusion in future forecasting models. Forecast models could use historical seasonality from these codes to estimate monthly demand, support staffing decisions, and anticipate system capacity needs.


### Chart 3: Transaction Volume by Credit and Debit (Daily)

Interpretation:
The end-of-month increase in debit activity likely reflects scheduled outgoing payments such as supplier invoices, rent, loan repayments, or payroll processing. The drop in credit volume at the same time suggests fewer incoming funds relative to outflows during this period.

Why it matters:
This pattern confirms that cash outflows are temporally concentrated. For forecasting, this is critical:
•	models should account for end-of-month seasonality,
•	cash management teams can plan liquidity buffers, and
•	operations can align settlement schedules to reduce risk of shortfalls.



### Chart 4: Transaction Volume and Net Amount by Day of Week

Interpretation:
This pattern reflects business-driven transaction behavior, where most financial activity occurs during standard operating days. Weekend transactions are limited and do not materially affect overall cash movement.

Why it matters:
Day-of-week seasonality is structural and predictable, making it essential for:
•	forecasting daily transaction demand,
•	scheduling staff and system processing windows, and
•	avoiding models that treat all days equally (which would overestimate weekend activity).

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
