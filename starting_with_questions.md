Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

We need to use SUM function to calculate `transactionrevenue` for each city and country
 combination. We need exclude NULL results and to groups the results by city and country using the GROUP BY. 
 Then, we need to sort the result by setting it in descending order based on the 
 `total_revenue` using the ORDER BY. Finally, we use `limit 10` clause to limit the result
 set to the top 10 cities and countries with the highest transaction revenues.

SQL Queries:
```SQL
SELECT country, city, SUM(totaltransactionrevenue) AS total_revenue
FROM all_sessions
WHERE transactions IS NOT NULL
GROUP BY country, city
ORDER BY total_revenue DESC
LIMIT 10;
```



Answer:
>According to results, top 5 cities are from USA and Top 3 Countries are USA, Israel, Australia. 
The 1st city in the rank is Unknown, so it could be several cities, 
so we better consider #2 cita as first.  
>
	country	city
>1	United States Unknown city
>2	United States San Francisco
>3	United States Sunnyvale
>4	United States Atlanta
>5	United States Palo Alto
>6	Israel Tel Aviv-Yafo
>7	United States New York
>8	United States Los Angeles
>9	United States Mountain View
>10	United States Chicago

	countries:
>1. United States
>2. Israel
>3. Australia



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
For cities:
```SQL
select alls.city, avg(a.units_sold) as avg_num_product 
from all_sessions as alls
join analytics as a
on alls.visitid=cast(a.visitid as int)
where a.units_sold is not null
group by city
order by avg_num_product DESC
```
For Countries:
```
select alls.country, avg(a.units_sold) as avg_num_product 
from all_sessions as alls
join analytics as a
on alls.visitid=cast(a.visitid as int)
where a.units_sold is not null
group by alls.country
order by avg_num_product DESC
```

Answer:
> The output shows the below figure (limited to 3 countries/cities):

> Country   avg_num_products
>1. Canada   2.4
>2. United States   2.2
>3. Bulgaria   1.5

> City   avg_num_products
>1. Chicago   5
>2. Pittsburgh   4
>3. New York   3.8


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL
SELECT city, country, v2productcategory, COUNT(*) AS order_count
FROM all_sessions
WHERE country!='(not set)' and city!='(not set)'
GROUP BY city, country, v2productcategory
ORDER BY city, country, order_count DESC;
```

Answer:
>To identify any patterns in the types (product categories) of products ordered
 from visitors in each city and country, we can use above query to analyze the data.
 The result shows that products from "HOME" category ordered by visitors in 
 most of the cities and countries is most frequent, which shows the product preferences
 and can be considered as pattern and product trend.



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL
select alls.country, sr.name, SUM(sr.total_ordered) as top_selling
from sales_report as sr
join all_sessions as alls
on sr.productsku=alls.productsku
WHERE country!='(not set)' and city!='(not set)'
group by country, sr.name
order by top_selling desc
```

Answer:
> By runnig the above query, we identified top-selling product for each country. 
Below is the result for the Top 5 countries:

>1. Top-selling product in USA is 17oz Stainless Steel Sport Bottle.
>2. Top-selling product in United Kingdom is Hard Cover Journal.
>3. Top-selling product in Germany is Ballpoint LED Light Pen.
>4. Top-selling product in Canada is also 17oz Stainless Steel Sport Bottle.
>5. Top-selling product in India  is 17oz Stainless Steel Sport Bottle as well. 


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
with city_rev as (
SELECT city, SUM(totaltransactionrevenue) as total_sum
FROM all_sessions
where totaltransactionrevenue is not null and city !='not available in demo dataset'
group by city), 
total_world as (SELECT SUM(totaltransactionrevenue) AS total_worldwide_revenue
FROM all_sessions)
select city, round(city_rev.total_sum::numeric / total_world.total_worldwide_revenue::numeric*100, 1)
from city_rev
cross join total_world
```

Answer:
> To analyzed the inmact, I have derived the revenue of every city and total revenue. 
Then found the ration of the city revenue to compare with total revenue, to see the
 impact of each city to total revenue.






