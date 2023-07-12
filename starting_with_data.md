Question 1: To find the total number of unique visitors

SQL Queries:
```SQL
SELECT COUNT(DISTINCT fullvisitorid) AS unique_visitors
FROM all_sessions
```
Answer: 
> The total number of unique visitors is 14147


Question 2: Find the average price of products in a table named products

SQL Queries:
```SQL
SELECT round(AVG(productprice), 1) AS average_price
FROM all_sessions
```
Answer:
> The avarage product price is 27.9


Question 3: Best saling product during 2017 year

SQL Queries:
```SQL
select v2productname, count(*) as total_sales
from all_sessions
where extract(year from date)=2017
group by v2productname
order by total_sales
limit 1
```
Answer:
> By using above query, we found the best selling product in 2017 year, which is "15 oz Ceramic Mug".


Question 4: Find out the revenue generated during May 2017. 

SQL Queries:
```SQL
SELECT SUM(total_revenue) as revenue_May_2017
FROM (
SELECT TO_DATE(date::text, 'YYYYMMDD') AS date, SUM(revenue) AS total_revenue
FROM analytics
WHERE revenue IS NOT NULL 
  AND EXTRACT(YEAR FROM TO_DATE(date::text, 'YYYYMMDD')) = 2017 
  AND EXTRACT(MONTH FROM TO_DATE(date::text, 'YYYYMMDD')) = 05
GROUP BY date
ORDER BY date) as rev_May_2017
```

Answer:
> Revenue for May 2017 is 357152. 
> The idea was to check the revenue during December, Festive time, 
> but the database has no values for this time. May considered as an example

