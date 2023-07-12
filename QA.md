What are your risk areas? Identify and describe them.
> I believe that it might be a risk if all the team working on the same database and doing permanent 
changes like deleting or amending values. Creating copies could prevent this issue, considering that 
the storage cost is relatively affordable, nowadays. 

> Performance; As databases grow in size and complexity, ensuring optimal performance 
becomes crucial. Poorly optimized queries, inadequate indexing, or inefficient database
 design can lead to slow query execution and performance degradation.
 
> Database Availability and Reliability: Databases need to be highly available and reliable to
 ensure uninterrupted access to critical data. System failures, hardware issues, or network
 outages can impact database availability, causing downtime and affecting business operations.
 
> Backup and Recovery: Inadequate backup and recovery strategies can result in data loss or
 the inability to restore databases in case of failures or disasters. Regular backups, 
 proper storage, and tested recovery procedures are essential to mitigate this risk.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
> The purpose of th QA process is to check the accuracy and quality of data. Below we will try 
to detect invalid information from the database "Ecommerce". 
>1. By using below queries, we will identify the schema of the Ecommerce database and its tables:
```SQL
SELECT *
FROM information_schema.tables
WHERE table_schema = 'public'
```
> The result shows that we have 5 tables and 2 views, which I created fot the Task 3, Question 5. 
> 2. We can check data types and nullability for the `all_sessions` table by using below query:
```SQL
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'all_sessions'
```
> 3. Now we can check field values, using `fullvisitorid` column:
```SQL
select fullvisitorid
from all_sessions
where country is NULL or city is NULL

select fullvisitorid
from all_sessions
where visitid is NULL

select fullvisitorid
from all_sessions
where productprice is NULL
	or v2productname is NULL
	or v2productcategory is NULL
```
>4. we can also check for unique fields:
```SQL
SELECT fullvisitorid,
    SUM(1) AS count
FROM all_sessions
GROUP BY 1
HAVING SUM(1) > 1;
```
> `all_sessins.fullvisitorid` is not unique.
```SQL
select productsku,
	sum(1) as count
from sales_report
group by 1
having sum(1)>1
```
> `sales_report.productsku` is unique.
```SQL
select sku,
	sum(1) as count
from products
group by 1
having sum(1)>1
```
> `products.sku` is unique.


