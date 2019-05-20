# Aggregations

## Quiz SUM

### Q1
Find the total amount of poster_qty paper ordered in the orders table.

### A1
    SELECT SUM(poster_qty) AS poster
    FROM orders

### Q2
Find the total amount of standard_qty paper ordered in the orders table.

### A2
    SELECT SUM(standard_qty) AS standard
    FROM orders
    
### Q3
Find the total dollar amount of sales using the total_amt_usd in the orders table.

### A3
    SELECT SUM(total_amt_usd) AS usd
    FROM orders

### Q4
Find the total amount spent on standard_amt_usd and gloss_amt_usd paper for each order in the orders table. This should give a dollar amount for each order in the table.

### Wrong A4
    SELECT SUM(standard_amt_usd) AS standard,
    	     SUM(gloss_amt_usd) AS gloss
    FROM orders
  => misunderstood
  
### A4
    SELECT standard_amt_usd + gloss_amt_usd AS total_standard_gloss
    FROM orders

### Q5
Find the standard_amt_usd per unit of standard_qty paper. Your solution should use both an aggregation and a mathematical operator.

### A5
    SELECT SUM(standard_amt_usd)/  SUM(standard_qty) AS unit
    FROM orders

## Quiz MIN, MAX & AVG

### Q1
When was the earliest order ever placed? You only need to return the date.

### A1
    SELECT MIN(occurred_at) AS earlies_order
    FROM orders

### Q2
Try performing the same query as in question 1 without using an aggregation function. 

### A2
    SELECT occurred_at
    FROM orders
    ORDER BY occurred_at 
    LIMIT 1


### Q3
When did the most recent (latest) web_event occur?

### A3
    SELECT MAX(occurred_at) AS latest_event
    FROM web_events

### Q4
Try to perform the result of the previous query without using an aggregation function.

### A4
    SELECT occurred_at
    FROM web_events
    ORDER BY occurred_at DESC
    LIMIT 1

### Q5
Find the mean (AVERAGE) amount spent per order on each paper type, 
as well as the mean amount of each paper type purchased per order. 
Your final answer should have 6 values - one for each paper type for the average number of sales, 
as well as the average amount.

### A5
    SELECT AVG(standard_qty) AS Standard_qty, 
    AVG(gloss_qty) AS Gloss_qty,
    AVG(poster_qty) AS Poster_qty,
    AVG(standard_amt_usd) AS Standard_amt, 
    AVG(gloss_amt_usd) AS Gloss_amt, 
    AVG(poster_amt_usd) AS Poster_amt
    FROM orders

### Q6
Via the video, you might be interested in how to calculate the MEDIAN. 
Though this is more advanced than what we have covered so far try finding - what is the MEDIAN total_usd spent on all orders?

### WRONG A6
    SELECT total_amt_usd
    FROM orders
    ORDER BY total_amt_usd DESC
    LIMIT COUNT(total_amt_usd)/2
=> aggregate function are not allowed in LIMIT

### A6
    SELECT *
    FROM (SELECT total_amt_usd
          FROM orders
          ORDER BY total_amt_usd
          LIMIT 3457) AS Table1
    ORDER BY total_amt_usd DESC
    LIMIT 2;


## Quiz GROUP BY

### Q1
Which account (by name) placed the earliest order? Your solution should have the account name and the date of the order.

### A1
    SELECT a.name, o.occurred_at
    FROM accounts a
    JOIN orders o
    ON a.id = o.account_id
    ORDER BY occurred_at
    LIMIT 1

### Q2
Find the total sales in usd for each account. You should include two columns - the total sales for each company's orders in usd and the company name.

### A2
    SELECT a.name , SUM(o.total_amt_usd) AS total_usd
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name
    ORDER BY a.name

### Q3
Via what channel did the most recent (latest) web_event occur, which account was associated with this web_event? Your query should return only three values - the date, channel, and account name.

### A3
    SELECT w.channel , MAX(w.occurred_at), a.name
    FROM web_events AS w
    JOIN accounts AS a
    ON w.account_id = a.id
    GROUP BY w.channel, a.name
    ORDER BY w.channel

### diffent answer
    SELECT w.occurred_at, w.channel, a.name
    FROM web_events w
    JOIN accounts a
    ON w.account_id = a.id 
    ORDER BY w.occurred_at DESC
    LIMIT 1

       

### Q4
Find the total number of times each type of channel from the web_events was used. Your final table should have two columns - the channel and the number of times the channel was used.

### A4
    SELECT channel, COUNT(channel)
    FROM web_events
    GROUP BY channel
    ORDER BY channel
### Q5
Who was the primary contact associated with the earliest web_event? 

### A5
    SELECT a.primary_poc, MIN(w.occurred_at)
    FROM accounts AS a
    JOIN web_events AS w
    ON a.id = w.account_id
    GROUP BY a.primary_poc
    ORDER BY a.primary_poc
    
### diffent answer
    SELECT a.primary_poc
    FROM web_events w
    JOIN accounts a
    ON a.id = w.account_id
    ORDER BY w.occurred_at
    LIMIT 1

### Q6
What was the smallest order placed by each account in terms of total usd. Provide only two columns - the account name and the total usd. Order from smallest dollar amounts to largest.

### A6
    SELECT a.name , MIN(o.total_amt_usd)
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name
    ORDER BY a.name

### Q7
Find the number of sales reps in each region. Your final table should have two columns - the region and the number of sales_reps. Order from fewest reps to most reps.

### A7
    SELECT r.name, COUNT(s.name)
    FROM region AS r
    JOIN sales_reps AS s
    ON r.id = s.region_id
    GROUP BY r.name
    ORDER BY COUNT(s.name)


## Quiz GROUP BY 2

### Q1
For each account, determine the average amount of each type of paper they purchased across their orders. Your result should have four columns - one for the account name and one for the average quantity purchased for each of the paper types for each account. 

### A1
    SELECT a.name, AVG(o.standard_qty) AS AVG_S, AVG(o.gloss_qty) AS AVG_G, AVG(o.poster_qty) AS AVG_P
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name
### Q2
For each account, determine the average amount spent per order on each paper type. Your result should have four columns - one for the account name and one for the average amount spent on each paper type.

### A2
    SELECT a.name, AVG(o.standard_amt_usd) AS AVG_S, AVG(o.gloss_amt_usd) AS AVG_G, AVG(o.poster_amt_usd) AS AVG_P
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name

### Q3
Determine the number of times a particular channel was used in the web_events table for each sales rep. Your final table should have three columns - the name of the sales rep, the channel, and the number of occurrences. Order your table with the highest number of occurrences first.

### A3
    SELECT COUNT(*) AS occurence, w.channel, s.name
    FROM web_events AS w
    JOIN accounts AS a
    ON a.id = w.account_id
    JOIN sales_reps AS s
    ON a.sales_rep_id = s.id
    GROUP BY w.channel, s.name
    ORDER BY occurence DESC
### Q4
Determine the number of times a particular channel was used in the web_events table for each region. Your final table should have three columns - the region name, the channel, and the number of occurrences. Order your table with the highest number of occurrences first.

### A4
    SELECT COUNT(*) AS occurence, w.channel, r.name
    FROM web_events AS w
    JOIN accounts AS a
    ON a.id = w.account_id
    JOIN sales_reps AS s
    ON a.sales_rep_id = s.id
    JOIN region AS r
    ON s.region_id = r.id
    GROUP BY w.channel, r.name
    ORDER BY occurence DESC

## Quiz DISTINCT

### Q1
Use DISTINCT to test if there are any accounts associated with more than one region.

### A1
    SELECT a.name AS account_name, r.name AS region_name
    FROM accounts AS a
    JOIN sales_reps AS s
    ON a.sales_rep_id=s.id
    JOIN region AS r
    ON r.id= s.region_id
    ORDER BY a.name
    
### different answer
    SELECT DISTINCT id, name
    FROM accounts

### Q2
Have any sales reps worked on more than one account?

### A2
    SELECTa.name AS account_name, s.name AS sales_name
    FROM accounts AS a
    JOIN sales_reps AS s
    ON a.sales_rep_id=s.id
    ORDER BY a.name

### different answer
    SELECT DISTINCT id, name
    FROM sales_reps;

## Quiz HAVING

#### Confusion about the difference between WHERE and HAVING
> WHERE subsets the returned data based on a logical condition.
> WHERE appears after the FROM,JOIN, and ON clauses, but before GROUP BY.
> HAVING appears after the GROUP BY clause, but before the ORDERE BY clause.
> HAVING is like WHERE, but it works on logical statements invoning aggregations.


### Q1
How many of the sales reps have more than 5 accounts that they manage?

### A1
    SELECT s.name, COUNT(a.name) AS account_count
    FROM sales_reps AS s
    JOIN accounts AS a
    ON a.sales_rep_id = s.id
    GROUP BY s.name
    HAVING COUNT(a.name) >=5
    ORDER BY s.name
### Q2
How many accounts have more than 20 orders?

### A2
    SELECT a.name, COUNT(*) AS count_orders
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name
    HAVING COUNT(*)>=20

### Q3
Which account has the most orders?

### A3
    SELECT a.name, COUNT(*) AS count_orders
    FROM accounts AS a
    JOIN orders AS o
    ON a.id = o.account_id
    GROUP BY a.name
    HAVING COUNT(*)>=20
    ORDER BY COUNT(*) DESC
    LIMIT 1
### Q4
Which accounts spent more than 30,000 usd total across all orders?

### A4
    SELECT a.name, SUM(o.total_amt_usd)
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY a.name
    HAVING SUM(o.total_amt_usd)>=30000

### Q5
Which accounts spent less than 1,000 usd total across all orders?

### A5
    SELECT a.name, SUM(o.total_amt_usd)
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY a.name
    HAVING SUM(o.total_amt_usd)<1000

### Q6
Which account has spent the most with us?

### A6
    SELECT a.name, SUM(o.total_amt_usd)
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY a.name
    ORDER BY SUM(o.total_amt_usd) DESC
    LIMIT 1

### Q7
Which account has spent the least with us?

### A7
    SELECT a.name, SUM(o.total_amt_usd)
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY a.name
    ORDER BY SUM(o.total_amt_usd) 
    LIMIT 1 
### Q8
Which accounts used facebook as a channel to contact customers more than 6 times?

### A8
    SELECT a.name, w.channel,COUNT(*)	
    FROM accounts AS a
    JOIN web_events AS w
    ON a.id=w.account_id
    GROUP BY a.name,w.channel
    HAVING COUNT(*)>6 AND w.channel = 'facebook'
### Q9
Which account used facebook most as a channel? 

### A9
    SELECT a.name, w.channel,COUNT(*)	
    FROM accounts AS a
    JOIN web_events AS w
    ON a.id=w.account_id
    GROUP BY a.name,w.channel
    HAVING COUNT(*)>6 AND w.channel = 'facebook'
    ORDER BY COUNT(*) DESC
    LIMIT 1


### Q10
Which channel was most frequently used by most accounts?

### A10

    SELECT a.name, w.channel,COUNT(*)	
    FROM accounts AS a
    JOIN web_events AS w
    ON a.id=w.account_id
    GROUP BY a.name,w.channel
    ORDER BY COUNT(*) DESC
    LIMIT 10

## Quiz DATE Functions

### Q1
Find the sales in terms of total dollars for all orders in each year, ordered from greatest to least. Do you notice any trends in the yearly sales totals?

### A1
    SELECT DATE_TRUNC('year',occurred_at), SUM(total_amt_usd) AS year_total
    FROM orders
    GROUP BY 1
    ORDER BY 2 DESC

### Q2
Which month did Parch & Posey have the greatest sales in terms of total dollars? Are all months evenly represented by the dataset?

### A2
    SELECT DATE_PART('month',occurred_at), SUM(total_amt_usd) AS month_total
    FROM orders
    GROUP BY 1
    ORDER BY 2 DESC
### Q3
Which year did Parch & Posey have the greatest sales in terms of total number of orders? Are all years evenly represented by the dataset?

### A3
    SELECT DATE_PART('year',occurred_at), SUM(total) AS year_total
    FROM orders
    GROUP BY 1
    ORDER BY 2 DESC
    
### given answer
    SELECT DATE_PART('year', occurred_at) ord_year,  COUNT(*) total_sales
    FROM orders
    GROUP BY 1
    ORDER BY 2 DESC

### Q4
Which month did Parch & Posey have the greatest sales in terms of total number of orders? Are all months evenly represented by the dataset?

### A4
    SELECT DATE_PART('month',occurred_at), SUM(total) AS month_total
    FROM orders
    GROUP BY 1
    ORDER BY 2 DESC

### Q5
In which month of which year did Walmart spend the most on gloss paper in terms of dollars?

### A5
    SELECT DATE_TRUNC('month', o.occurred_at), SUM(o.gloss_amt_usd), a.name
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY 1,3
    HAVING a.name='Walmart'
    ORDER BY 2 DESC

## Quiz CASE

### Q1
Write a query to display for each order, the account ID, total amount of the order, and the level of the order - ‘Large’ or ’Small’ - depending on if the order is $3000 or more, or smaller than $3000.

### A1
    SELECT account_id,
           total,       
    	   CASE WHEN total_amt_usd >= 3000 THEN 'large' ELSE 'small' END AS level
    FROM orders
    ORDER BY account_id

### Q2
Write a query to display the number of orders in each of three categories, based on the 'total' amount of each order. The three categories are: 'At Least 2000', 'Between 1000 and 2000' and 'Less than 1000'.

### A2
    SELECT total,
           CASE WHEN total >= 2000 THEN 'At Least 2000'
           WHEN total<2000 AND total >=1000 THEN 'Between 1000 and 2000'
           ELSE 'Less than 1000' END 
    FROM orders

### Q3
We would like to understand 3 different levels of customers based on the amount associated with their purchases. The top level includes anyone with a Lifetime Value (total sales of all orders) greater than 200,000 usd. The second level is between 200,000 and 100,000 usd. The lowest level is anyone under 100,000 usd. Provide a table that includes the level associated with each account. You should provide the account name, the total sales of all orders for the customer, and the level. Order with the top spending customers listed first.

### A3
    SELECT a.name, SUM(o.total_amt_usd) AS total_sales,
           CASE WHEN SUM(o.total_amt_usd) >=200000 THEN 'top'
           WHEN SUM(o.total_amt_usd)<200000 AND SUM(o.total_amt_usd)>100000 THEN 'second'
           ELSE 'low' END AS level
    FROM accounts AS a
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY 1
    ORDER BY 3 DESC

### Q4
We would now like to perform a similar calculation to the first, but we want to obtain the total amount spent by customers only in 2016 and 2017. Keep the same levels as in the previous question. Order with the top spending customers listed first.

### A4
    SELECT account_id,
           total,       
    	   CASE WHEN total_amt_usd >= 3000 THEN 'large' ELSE 'small' END AS level
    FROM orders
    WHERE occurred_at BETWEEN '2016-01-01' AND '2018-01-01'
    ORDER BY account_id, total DESC
### Q5
We would like to identify top performing sales reps, which are sales reps associated with more than 200 orders. Create a table with the sales rep name, the total number of orders, and a column with top or not depending on if they have more than 200 orders. Place the top sales people first in your final table.

### A5
    SELECT s.name, COUNT(*) AS orders,
           CASE WHEN COUNT(*)>200 THEN 'top'
           ELSE 'not' END AS TOP
    FROM sales_reps AS s
    JOIN accounts AS a
    ON s.id = a.sales_rep_id
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY s.name
    ORDER BY COUNT(*) DESC

### Q6
The previous didn't account for the middle, nor the dollar amount associated with the sales. Management decides they want to see these characteristics represented as well. We would like to identify top performing sales reps, which are sales reps associated with more than 200 orders or more than 750000 in total sales. The middle group has any rep with more than 150 orders or 500000 in sales. Create a table with the sales rep name, the total number of orders, total sales across all orders, and a column with top, middle, or low depending on this criteria. Place the top sales people based on dollar amount of sales first in your final table. You might see a few upset sales people by this criteria!

### A6
    SELECT s.name, COUNT(*) AS orders, SUM(o.total_amt_usd) AS total_sale,
           CASE WHEN COUNT(*)>200 OR SUM(o.total_amt_usd)>750000  THEN 'top'
           WHEN (COUNT(*)<=200 AND COUNT(*)>150 ) OR (SUM(o.total_amt_usd)<=750000 AND SUM(o.total_amt_usd)>500000) THEN 'middle'
           ELSE 'low' END AS TOP
    FROM sales_reps AS s
    JOIN accounts AS a
    ON s.id = a.sales_rep_id
    JOIN orders AS o
    ON a.id=o.account_id
    GROUP BY s.name
    ORDER BY COUNT(*) DESC,SUM(o.total_amt_usd) DESC
