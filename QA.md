What are your risk areas? Identify and describe them.
>I believe that it might be a risk if all the team working on the same database and doing permanent 
changes like deleting or amending values. Creating copies could prevent this issue, considering that 
the storage cost is relatively affordable, nowadays. 


QA Process:
Describe your QA process and include the SQL queries used to execute it.
> The purpose of th QA process is to check the accuracy and quality of data. Below we will try 
to detect invalid information from the database "Ecommerce". 
> By using below queries, we will identify the schema of the Ecommerce database and its tables:
```SQL
SELECT *
FROM information_schema.tables
WHERE table_schema = 'public'
```
> The result shows that we have 5 tables and 2 views, which I created fot the Task 3, Question 5. 
>
