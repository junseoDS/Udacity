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







