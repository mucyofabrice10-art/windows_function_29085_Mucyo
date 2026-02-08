Course: Database Development with PL/SQL (INSY 8311)
Student Name: Mucyo Fabrice
Student ID: 29085
Instructor: Eric Maniraguha

```sql

## Business Problem

A retail analytics company aims to understand how customers purchase products across different regions and time periods in order to improve inventory allocation, marketing campaigns, and revenue forecasting.

Decision-makers lack clear insight into:

Which products dominate sales in specific locations

How revenue changes month by month

Which customers generate the most value

Which products or customers show low activity

This project applies SQL JOINs to combine operational data and window functions to perform advanced analytical calculations without collapsing result sets.

 ## Success Criteria

The analysis targets five measurable goals:

Identify top-selling products by region or quarter

Track cumulative monthly revenue

Compare current and previous months

Group customers into four spending levels

Compute moving averages for trend analysis

## Database Schema Overview

Three core tables form the analytical model:

Customers — stores personal information and geographic region.

Products — contains product details such as category and price.

Transactions — logs purchase events with quantity and date.

Primary keys uniquely identify records, while foreign keys ensure valid relationships between transactions, customers, and products. This relational structure allows accurate aggregation and segmentation.

## Part A — JOINs (Conceptual Focus)

JOINs are used to combine rows across tables so that business questions can be answered using a single query.

Instead of listing every possible JOIN, this project focuses on key analytical use cases:

INNER JOIN to analyze valid sales records

LEFT JOIN to detect inactive customers

FULL JOIN to uncover unsold products

SELF JOIN to compare customers in the same region

Example — Finding Customers With No Purchases

This query links customers to transactions and filters for those without matching sales.

SELECT c.customer_id,
       c.customer_name,
       t.transaction_id
FROM customers c
LEFT JOIN transactions t
       ON c.customer_id = t.customer_id
WHERE t.transaction_id IS NULL,


Explanation:

The LEFT JOIN keeps all customers, even when no transaction exists.

Rows where transaction_id is NULL represent customers who never purchased.

These customers are strong candidates for onboarding promotions or discounts.

##  Part B — Window Functions (Analytical Focus)

Window functions perform calculations across related rows without grouping them into a single row, which is ideal for ranking, trend detection, and comparisons over time.

This project emphasizes four families of window functions:

Ranking — ordering products or customers

Aggregates — running totals and averages

Navigation — comparing earlier and later rows

Distribution — customer segmentation

## Ranking Example — Top Products per Region

This query ranks products based on total revenue inside each region and selects only the top five.

SELECT region,
       product_name,
       total_sales,
       RANK() OVER (
           PARTITION BY region
           ORDER BY total_sales DESC
       ) AS rank_in_region
FROM (
     SELECT c.region,
            p.product_name,
            SUM(t.total_amount) AS total_sales
     FROM transactions t
     JOIN customers c ON t.customer_id = c.customer_id
     JOIN products p ON t.product_id = p.product_id
     GROUP BY c.region, p.product_name
)
WHERE rank_in_region <= 5;


What this achieves:

Sales are first aggregated by product and region.

PARTITION BY resets the ranking inside each region.

RANK() assigns positions based on revenue.

Management can quickly see which items deserve priority stocking locally.

Trend Example — Running Monthly Revenue

This calculation shows how sales accumulate across months.

SELECT month,
       monthly_sales,
       SUM(monthly_sales)
         OVER (
           ORDER BY month
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
         ) AS running_total
FROM (
   SELECT TRUNC(transaction_date,'MM') AS month,
          SUM(total_amount) AS monthly_sales
   FROM transactions
   GROUP BY TRUNC(transaction_date,'MM')
);


Why this matters:

Monthly totals alone show performance per period.

The running total highlights long-term growth patterns.

Sudden plateaus or jumps signal operational or marketing changes.

## omparison Example — Month-Over-Month Growth

Navigation functions allow comparison between rows.

SELECT month,
       monthly_sales,
       LAG(monthly_sales) OVER (ORDER BY month) AS prev_month,
       monthly_sales - 
       LAG(monthly_sales) OVER (ORDER BY month) AS growth
FROM (
     SELECT TRUNC(transaction_date,'MM') AS month,
            SUM(total_amount) AS monthly_sales
     FROM transactions
     GROUP BY TRUNC(transaction_date,'MM')
);


Interpretation:

LAG() fetches the previous month’s revenue.

Subtracting values reveals increase or decrease.

Negative growth may indicate seasonal decline or inventory issues.

## Segmentation Example — Customer Quartiles

Customers are grouped into four spending tiers.

SELECT customer_id,
       total_spent,
       NTILE(4) OVER (ORDER BY total_spent DESC) AS quartile
FROM (
     SELECT customer_id,
            SUM(total_amount) AS total_spent
     FROM transactions
     GROUP BY customer_id
);


Business value:

Quartile 1 represents top customers.

Quartile 4 includes low-engagement buyers.

Marketing teams can personalize loyalty programs.

## Results Summary
Descriptive — What Happened?

Sales were concentrated in a limited number of products and regions, while several items recorded minimal activity.

Diagnostic — Why Did It Happen?

Differences are linked to customer density, repeat purchase behavior, and seasonal effects.

Prescriptive — What Should Be Done?

Increase stock in high-performing areas

Promote under-selling items

Focus loyalty campaigns on high-quartile customers

Reallocate marketing budgets regionally

 Integrity Statement

“All sources were properly cited, and this work reflects original academic effort.”

## References

Oracle SQL Language Reference
PostgreSQL Window Functions Documentation
Course Lecture Notes
