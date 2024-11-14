# Deposit Master Dashboard Analysis

## Interactive Dashboards: https://app.powerbi.com/groups/me/reports/ee97da9f-311e-4bae-a898-dc0e575916c2?ctid=df8679cd-a80e-45d8-99ac-c83ed7ff95a0&pbi_source=linkShare

## Project Overview

This project addresses the challenges a bank faces in managing and analyzing customer deposit data. The fragmented and unstructured nature of the data hindered the bank's ability to monitor deposit trends, analyze customer behavior, optimize product offerings, manage liquidity risk, and improve customer experience. This project leverages Power BI, PostgreSQL, and data modeling techniques to transform raw deposit data into actionable insights.

## Objectives

1. **Monitor Deposit Trends:** Track changes in deposit balances, growth rates, and customer behavior over time.
2. **Analyze Customer Behavior:** Identify customer preferences and opportunities for cross-selling and upselling.
3. **Optimize Product Offerings:** Assess deposit product performance to tailor offerings to customer needs.
4. **Manage Liquidity Risk:** Monitor liquidity levels and ensure sufficient funds are available to meet withdrawals and obligations.
5. **Enhance Customer Experience:** Identify and address pain points to boost customer satisfaction and loyalty.

## Solution

### Data Preparation 

To manage extensive datasets beyond Excel’s capacity, I implemented an ETL (Extract, Transform, Load) pipeline using PostgreSQL as an intermediary:

- **Database Setup:** A local PostgreSQL database was created in pgAdmin to store and manage large data volumes efficiently, bypassing Excel's limitations with large datasets.
- **Data Import using psql:** Data from CSV files was imported directly into PostgreSQL tables using the following commands:
  
  ```sql
  \COPY Branch(branch_name, region) FROM 'D:\001 Deposit Master Project\Data\Branch.csv' DELIMITER ',' CSV HEADER;

  \COPY AccountCategory(account_name, account_category, currency) FROM 'D:\001 Deposit Master Project\Data\AccountCategory.csv' DELIMITER ',' CSV HEADER;

  \COPY Balance(account_number, branch_id, cif_id, accountcategory_id, balance_date, balance) FROM 'D:\001 Deposit Master Project\Data\Balance_Modified.csv' DELIMITER ',' CSV HEADER;
  ```

   **Table Creation:** Tables were created in PostgreSQL to store and manage the data. The following SQL commands were used to drop existing tables (if any) and create new tables for structured data storage:

  ```sql
  -- Drop tables if they exist
  DROP TABLE IF EXISTS Balance CASCADE;
  DROP TABLE IF EXISTS AccountCategory CASCADE;
  DROP TABLE IF EXISTS Branch CASCADE;

  -- Create the AccountCategory table
  CREATE TABLE AccountCategory (
      id serial PRIMARY KEY,
      account_name varchar(150) NOT NULL,
      account_category varchar(30) NOT NULL,
      currency varchar(3) NOT NULL
  );

  -- Create the Branch table
  CREATE TABLE Branch (
      id serial PRIMARY KEY,
      branch_name varchar(25) NOT NULL,
      region varchar(25) NOT NULL
  );

  -- Create the Balance table
  CREATE TABLE Balance (
      account_number bigint NOT NULL,
      branch_id integer NOT NULL REFERENCES Branch(id),
      cif_id integer NOT NULL,
      accountcategory_id integer NOT NULL REFERENCES AccountCategory(id),
      balance_date date NOT NULL,
      balance numeric NOT NULL,
      PRIMARY KEY (account_number, balance_date)
  );
- **Data Cleaning and Transformation:** The extracted data was cleaned to remove inconsistencies and errors, including:
  - Standardizing data formats and units of measurement.
  - Handling missing values and outliers.
  - Creating necessary calculations and aggregations.
- **Power BI Integration:** Once refined, the data was seamlessly imported into Power BI from the PostgreSQL database, ensuring smooth handling and optimal performance within Power BI.

### Data Modeling

A star schema data model was developed to organize deposit data, with:

- **Fact Table:** Storing key deposit metrics.
- **Dimension Tables:** Providing details on account categories, regions, time periods, and customer demographics.

### Data Analysis & Visualization

Using Power BI, I created an interactive dashboard that visualizes key metrics and trends in customer deposits.

## Key Insights

### 1. Account Performance and Growth
- **Total Accounts:** 3 million
- **Account Growth Rate:** -18.11%
- **Account Loss:** 609,000, indicating challenges in customer retention or acquisition.

### 2. Deposit Volume and Growth
- **Total Deposits:** 5.68 trillion, with a 1.52% growth rate.
- Despite the decline in accounts, deposits grew, suggesting higher average deposits per account.

### 3. Monthly Trends in Account and Deposit Volume
- Stable account numbers from **September to January** around 3.3 million.
- A peak in **February** (3.4 million accounts, 5.7 trillion deposits) followed by a sharp decline in **March** (1 million accounts, 1.7 trillion deposits), possibly due to seasonal trends or a significant withdrawal event.

### 4. Account Category Insights
- **Savings Accounts:** Largest share of account volume.
- **Current Accounts:** Lowest in count but hold significant deposits, likely from high-value customers or businesses.
- **Fixed Deposit & Call Accounts:** Significant contributors to total deposits due to likely favorable terms or higher interest rates.

## Technologies Used

- **Power BI**: For data visualization and dashboard development.
- **PostgreSQL & pgAdmin**: For data storage, cleaning, and transformation.
- **SQL**: For efficient data extraction and transformation.

## Conclusion

The Deposit Master Dashboard provides valuable insights into customer deposit behavior, allowing the bank to make data-driven decisions to improve retention, optimize deposit mobilization strategies, and enhance customer satisfaction. 

By identifying trends and patterns in deposit data, this project helps the bank align its offerings with customer needs, ultimately contributing to better financial stability and growth.

## Future Improvements

Potential future improvements include:

- Automating the ETL process for real-time data updates.
- Expanding the data model to include additional customer behavior metrics.
- Incorporating machine learning to predict customer trends and inform proactive strategies.

---

---


