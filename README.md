# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner 

## Objectives

1. **Set up a retail sales database**
2. **Data Cleaning**
3. **Exploratory Data Analysis (EDA)**
4. **Business Analysis**

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail`.
- **Table Creation**: A table named `products_name` is created to store the sales data. The table includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, COGS, and total sale amount.

```sql
CREATE DATABASE retail;
USE retail;
CREATE TABLE products_name(
transactions_id INT PRIMARY KEY,
	sale_date DATE,
    sale_time TIME,
    customer_id	INT,
    gender	VARCHAR(20),
    age	INT,
    category	VARCHAR(20),
    quantiy	INT,
    price_per_unit	FLOAT,
    cogs	FLOAT,
    total_sale FLOAT
    );
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
 select * from products_name
   WHERE 
   transactions_id IS NULL
   or
   sale_date IS NULL
   or
   sale_time IS NULL
   or
   customer_id IS NULL
   or
   gender IS NULL
   OR
   age IS NULL
   or
   category IS NULL
   OR
   quantiy IS NULL
   or
   price_per_unit IS NULL
   or
   cogs IS NULL
   OR 
   total_sale IS NULL;
```

### 3. Data Analysis

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select * from products_name;

select * from products_name
where sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT * FROM products_name
where category ='Clothing'
and
quantiy>=4
and 
year(sale_date) = 2022
and
month(sale_date) =11
;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select category,
SUM(total_sale) as sale_sum,
count(*) as total_order from products_name
group by 1;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select 
round(AVG(age),2)
from products_name
WHERE category ='Beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT total_sale FROM products_name
WHERE total_sale>1000 ;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select category,gender, count(*)  from products_name
group by gender, category
order by 1;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT
year,
month,
avg_sale
from
(SELECT 
extract(year from sale_date) as year,
extract(month from sale_date) as month,
avg(total_sale) as avg_sale,

    RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank

from products_name
group by 1,2) as t1
;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
select customer_id, 
sum(total_sale) as total_sale
 from products_name
group by 1
order by 2 desc
limit 5
 ;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
 select category,
 COUNT( DISTINCT customer_id) as coustmer
 from products_name
 group by category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
 select 
 CASE
 WHEN extract(Hour from sale_time) <12 then 'morning'
 when extract(hour from sale_time) between 12 and 17 then 'afternoon'
 else 'evening'
 end as shift,
 count(*) as no_coustmers
 from products_name
 group by 1
;
```
