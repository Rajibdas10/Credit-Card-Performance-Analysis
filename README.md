# Credit-Card-Performance-Analysis on Power BI

## Project Overview
This project leverages Power BI to analyze credit card transaction data, providing actionable insights into revenue trends, customer behavior, and transaction patterns. The dashboards and reports created are designed to help stakeholders make data-driven decisions to improve customer engagement and revenue growth.

---

## Objective
The primary objective of this project is to:
- Analyze week-over-week (WoW) and year-to-date (YTD) performance metrics.
- Identify key contributors to revenue and transaction patterns.
- Highlight high-performing customer segments and regions.
- Provide insights into activation and delinquency rates for better risk management.

---

## Dataset Description
The dataset includes detailed transaction-level data with the following key attributes:
- **Customer Demographics:** Gender, region, and credit card type.
- **Transaction Details:** Amount, count, and revenue contributions.
- **Performance Metrics:** Activation rates and delinquency rates.

**Dataset Highlights:**
- Total Revenue: $57M
- Total Interest: $8M
- Total Transaction Amount: $46M
- Key Demographics: Male ($31M revenue) and Female ($26M revenue)

---

## Key Insights
### Week-over-Week Change:
- **Revenue:** Increased by 28.8%.
- **Total Transaction Amount & Count:** Increased by significant margins.
- **Customer Count:** Experienced noticeable growth.

### Year-to-Date Overview:
- **Overall Revenue:** $57M.
- **Total Interest:** $8M.
- **Total Transaction Amount:** $46M.
- **Demographics:** Male customers contribute $31M, and females contribute $26M.
- **Credit Card Contributions:** Blue and Silver credit cards account for 93% of all transactions.
- **Regional Analysis:** TX, NY, and CA contribute 68% of total revenue.
- **Activation Rate:** 57.5%.
- **Delinquent Rate:** 6.06%.

---

## Technical Details
### SQL Queries:
- **Database Creation and Table Setup:**
```sql
CREATE DATABASE ccdb;

CREATE TABLE cc_detail (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);

CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);
```
- **Data Import:**
```sql
COPY cc_detail
FROM 'D:\credit_card.csv' 
DELIMITER ',' 
CSV HEADER;

COPY cust_detail
FROM 'D:\customer.csv' 
DELIMITER ',' 
CSV HEADER;
```

### DAX Formulas:
- **Age Grouping:**
```dax
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)
```
- **Income Grouping:**
```dax
IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] <70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)
```
- **week num2 Calculation:**
```dax
week_num2 = WEEKNUM('public cc_detail'[week_start_date])
```

- **Current week Revenue Calculation:**
```dax
Current_week_Reveneue = CALCULATE(
SUM('public cc_detail'[Revenue]),
FILTER(
ALL('public cc_detail'),
'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])))
```
- **Previous week ReveneueCalculation:**
```dax
Previous_week_Reveneue = CALCULATE(
SUM('public cc_detail'[Revenue]),
FILTER(
ALL('public cc_detail'),
'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])-1))
```
- **Revenue Calculation:**
```dax
Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]
```

---

## Recommendations
- **Revenue Growth:** Focus marketing efforts on TX, NY, and CA to capitalize on their 68% revenue contribution.
- **Customer Segmentation:** Develop targeted offers for Blue and Silver cardholders, contributing to 93% of transactions.
- **Delinquency Management:** Implement risk mitigation strategies to lower the delinquency rate of 6.06%.
- **Activation Improvement:** Increase customer engagement to boost the activation rate beyond 57.5%.

---

## Visualizations
The Power BI dashboards include the following key visualizations:
1. **Revenue Trends:** Week-over-week and year-to-date revenue growth.
2. **Customer Segmentation:** Revenue contributions by gender and credit card type.
3. **Geographic Analysis:** State-level performance metrics (TX, NY, and CA).
4. **Activation and Delinquency Rates:** Highlighting potential risk factors.
5. **Transaction Patterns:** Volume and amount trends across customer groups.

---

## Tools Used
- **Power BI:** For data visualization and dashboard creation.
- **Excel:** For initial data cleaning and preprocessing.
- **SQL:** For data extraction and transformation.

---

## How to Access the Dashboards
1. Clone this repository:
   ```bash
   git clone Credit_Card_Report.pbix
   ```
2. Open the Power BI file located in the `dashboards/` folder.
3. Review the interactive dashboards and explore the key insights.

---

## Future Enhancements
- Incorporate predictive analytics to forecast revenue and delinquency trends.
- Expand analysis to include customer lifetime value (CLV).
- Introduce industry benchmarks for comparative analysis.

---

## Contact
For inquiries , feel free to reach out:
- **Email:** rajibdas10.10.1999@gmail.com
- **LinkedIn:** [Rajib Das](https://www.linkedin.com/in/rajib-das-4a6063215)

