# assignnment-windows-function
Student: MUCYO Fabrice
Student ID: 29085

## Business Problem

This project analyzes sales data for a retail company to identify top products per region, track monthly revenue trends, and segment customers for targeted marketing. The goal is to provide actionable insights to improve sales strategies and customer engagement.

## Database Schema

The project uses three related tables:

Customers

customer_id (PK)

name

region

Products

product_id (PK)

name

category

Transactions

transaction_id (PK)

customer_id (FK)

product_id (FK)

sale_date

amount

An ER diagram is included in the screenshots/ folder.

## SQL JOINs

INNER JOIN: Retrieves transactions for valid customers and products.

LEFT JOIN: Identifies customers with no transactions.

RIGHT/FULL OUTER JOIN: Finds products without sales.

FULL OUTER JOIN: Combines customers and products including unmatched records.

SELF JOIN: Compares customers within the same region.

Each query includes comments, screenshots, and a short business interpretation.

Window Functions

Implemented to provide advanced business insights:

Ranking Functions: ROW_NUMBER(), RANK(), DENSE_RANK(), PERCENT_RANK() — identify top customers and products.

Aggregate Functions: SUM() OVER(), AVG() OVER(), MIN(), MAX() — track running totals and trends.

Navigation Functions: LAG(), LEAD() — compare month-to-month performance.

Distribution Functions: NTILE(4), CUME_DIST() — segment customers by spending quartiles.

All functions include SQL scripts, screenshots, and interpretations.
Results Analysis

Descriptive: Revenue increased steadily from January to March; top customers generate most sales; some products dominate specific regions.

Diagnostic: Seasonal demand, regional promotions, and frequent purchases explain trends.

Prescriptive: Focus campaigns on top customers, ensure stock for best-selling products, and consider promotions for slow-moving items.

## References

Oracle Documentation – https://docs.oracle.com

W3Schools SQL Tutorial – https://www.w3schools.com/sql/

GeeksforGeeks – SQL Window Functions – https://www.geeksforgeeks.org/sql-window-functions/

## Academic Integrity Statement

All sources were properly cited. Implementations and analysis represent original work. No AI-generated content was copied without adaptation.
