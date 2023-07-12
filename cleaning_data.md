What issues will you address by cleaning the data?

>1. First of all I openned all tables in seperate panels, so I can check the same issue in all tables at the same time
>2. The first issue is the values of all column related to cost that seems huge, so it have been divided by 100000
>3. Missing values. Some columns have no values at all, so it is better to delete these columns
>4. I checked the column `all_sessions.currencycode` and found out that there are only 2 options: USD and NULL. So we can change NULL to USD or remove this column at all. I decided to use USD for entire column.
>5. Some columns starts with blanks. It is better to trim it these columns 


Queries:
Below, provide the SQL queries you used to clean your data.

1. Opened 5 separate columns
```SQL
SELECT * FROM each table separately 
```

2. Amended all values related to cost using below query:
```SQL
UPDATE all_sessions
SET totalTransactionRevenue=totalTransactionRevenue/1000000
WHERE totalTransactionRevenue>0
```
Use the same way to update columns `all_sessions.productprice`, `all_sessions.productrevenue`, `all_sessions.transactionrevenue`, `analytics.revenue`, `analytics.unit_price`

3. I have checked columns if there any values by using below query:
```SQL
select transactionrevenue
from all_sessions
where transactionrevenue is not null
```
then, dropped columns with no values:
```SQL
ALTER TABLE all_sessions
DROP COLUMN productrefundamount
DROP COLUMN itemquantity,
DROP COLUMN itemrevenue,
DROP COLUMN searchkeyword
```

4. Firtly, I run below quere to check unique values:
```SQL
select distinct(currencycode)
from all_sessions
```
then, replaced NULL with USD:
```SQL
UPDATE all_sessions
SET currencycode='USD'
WHERE currencycode is NULL
```

5. Colunm `name` in tables `products` and `sales_report` are trimmed by using below queries:
```SQL
UPDATE products
SET name = TRIM (name)

UPDATE sales_report
SET name = TRIM (name)
```


