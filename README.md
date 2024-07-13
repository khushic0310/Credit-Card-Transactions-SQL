README for Credit Card Transactions SQL Queries
This document provides an overview of SQL queries used to analyze credit_card_transactions data. These queries are designed to extract meaningful insights from transaction records, including analyses of transaction dates, card types, expenses by category and location, and spending patterns.

SQL Queries Overview
1. Date Range of Transactions
Extracts the earliest and latest dates on which transactions occurred.

'''sql
SELECT MIN(transaction_date) AS earliest_date, MAX(transaction_date) AS latest_date
FROM credit_card_transactions;

2. Card Types
Lists all unique credit card types represented in the transactions.

sql
Copy
SELECT DISTINCT card_type
FROM credit_card_transactions;
3. Expense Types
Identifies all unique types of expenses from the transactions.

sql
Copy
SELECT DISTINCT exp_type
FROM credit_card_parse_actions;
4. Total Expenses by City
Calculates the total expenditure for each city.

sql
Copy
SELECT city, SUM(amount) AS total_expenses
FROM credit_card_transactions
GROUP BY city;
5. Number of Transactions by Card Type
Counts the number of transactions per card type to determine the popularity of each card.

sql
Copy
SELECT card_type, COUNT(transaction_id) AS transaction_count
FROM credit_card_transactions
GROUP BY card_type;
6. Monthly Expenses by Expense Type
Analyzes expenses month by month for each type of expense, ordered by month and expense type.

sql
Copy
SELECT EXTRACT(MONTH FROM transaction_date) AS month, exp_type, SUM(amount) AS monthly_expense
FROM credit_card_transactions
GROUP BY month, exp_type
ORDER BY month, exp_type;
7. Top 5 Cities with Highest Spends
Identifies the top five cities by total credit card spend and their percentage contribution to the total spends.

sql
Copy
WITH TotalSpends AS (
    SELECT SUM(amount) AS total_amount
    FROM credit_card_transactions
),
CitySpends AS (
    SELECT city, SUM(amount) AS city_amount
    FROM credit_card_transactions
    GROUP BY city
)
SELECT CitySpends.city, CitySpends.city_amount, (CitySpends.city_amount / TotalSpends.total_amount * 100) AS percentage_contribution
FROM CitySpends, TotalSpends
ORDER BY CitySpends.city_amount DESC
LIMIT 5;
8. Highest Spend Month and Amount for Each Card Type
Finds the month in which the highest total spend occurred for each card type.

sql
Copy
WITH TotalSpends AS (
    SELECT card_type, 
    EXTRACT(YEAR FROM transaction_date) AS year, 
    EXTRACT(MONTH FROM transaction_date) AS month, 
    SUM(amount) AS total_amount
    FROM credit_card_transactions
    GROUP BY card_type, year, month
)
SELECT * FROM (SELECT *, RANK() OVER(PARTITION BY card_type ORDER BY total_amount DESC) AS rn
FROM TotalSpends) a WHERE rn=1;
9. City with Lowest Percentage Spend for Gold Card Type
Determines the city with the lowest spending as a percentage for transactions made with gold-type cards.

sql
Copy
-- Query details to be added based on specification
(Note: Query implementation for the lowest percentage spend for gold card type needs to be defined.)

Usage
These SQL queries can be run against a database that includes a credit_card_transactions table with relevant columns such as transaction_date, card_type, exp_type, city, and amount. The output from these queries will help in making informed decisions on credit card usage, expense monitoring, and budget allocation across different segments.
