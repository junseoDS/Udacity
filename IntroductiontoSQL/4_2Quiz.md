## Your First Subquery

### Q1
Find the number of events that occur for each day for each channel, using a subquery.
On which day-channel pair did the most events occur?

### A1
    SELECT *
    FROM
    (SELECT DATE_TRUNC('day', occurred_at) AS day, channel, COUNT(*) AS event_count
    FROM web_events
    GROUP BY 1,2
    ORDER BY 3 DESC) sub
 
### Q2
Find the average number of events for each channel.

### A2
    SELECT channel, AVG(event_count) AS avg_order
    FROM
    (SELECT DATE_TRUNC('day', occurred_at) AS day, channel, COUNT(*) AS event_count
    FROM web_events
    GROUP BY 1,2
    ORDER BY 3 DESC
    ) sub
    GROUP BY 1
    
## More on Subqueries

### Q1
Find only the orders that took place in the same month and year as the first order, and then pull the average for each type of paper qty in this month.

### A1
    SELECT AVG(standard_qty) AS avg_standard, 
    AVG(gloss_qty) AS avg_gloss, 
    AVG(poster_qty) AS avg_poster, 
    AVG(total_amt_usd) AS avg_total_usd
    FROM orders
    WHERE DATE_TRUNC('month', occurred_at)=
    (SELECT DATE_TRUNC('month',MIN(occurred_at))
    FROM orders)
    
    
## Quiz Subquery Mania

### Q1
Provide the name of the sales_rep in each region with the largest amount of total_amt_usd sales.

### A1
    SELECT *
    FROM (SELECT region_name, MAX(total_amt) AS max
          FROM (SELECT s.name sales_rep_name, r.name region_name, SUM(o.total_amt_usd) AS total_amt
                FROM region r
                JOIN sales_reps s
                ON r.id=s.region_id
                JOIN accounts a
                ON a.sales_rep_id=s.id
                JOIN orders o
                ON o.account_id=a.id
                GROUP BY 2,1) sub1
          GROUP BY 1) sub2
    JOIN (SELECT s.name sales_rep_name, r.name region_name, SUM(o.total_amt_usd) AS total_amt
          FROM region r
          JOIN sales_reps s
          ON r.id=s.region_id
          JOIN accounts a
          ON a.sales_rep_id=s.id
          JOIN orders o
          ON o.account_id=a.id
          GROUP BY 2,1) sub3
    ON sub2.region_name = sub3.region_name AND sub2.max = sub3.total_amt


### Q2
For the region with the largest (sum) of sales total_amt_usd, how many total (count) orders were placed? 

### A2
    SELECT r.name, COUNT(*)
    FROM region r
    JOIN sales_reps s
    ON r.id=s.region_id
    JOIN accounts a
    ON a.sales_rep_id=s.id
    JOIN orders o
    ON o.account_id=a.id
    GROUP BY 1
    HAVING SUM(o.total_amt_usd)=
        (
        SELECT MAX(sales_total_amt_usd)
        FROM
        (SELECT r.name region,SUM(o.total_amt_usd) sales_total_amt_usd
        FROM region r
        JOIN sales_reps s
        ON r.id=s.region_id
        JOIN accounts a
        ON a.sales_rep_id=s.id
        JOIN orders o
        ON o.account_id=a.id
        GROUP BY 1) sub1
          ) 
### Q3
For the name of the account that purchased the most (in total over their lifetime as a customer) standard_qty paper, how many accounts still had more in total purchases? 

### WRONG A3
    SELECT a.name, SUM(o.total) 
    FROM accounts a
    JOIN orders o
    ON a.id=o.account_id
    GROUP BY 1
    HAVING SUM(o.total) >
        (SELECT MAX(std_qty)
         FROM (SELECT a.name, SUM(o.standard_qty) std_qty
               FROM accounts a
               JOIN orders o
               ON a.id=o.account_id
               GROUP BY 1) sub1
          )
   
### A3
    SELECT COUNT(*)
    FROM (SELECT a.name
          FROM orders o
          JOIN accounts a
          ON a.id = o.account_id
          GROUP BY 1
          HAVING SUM(o.total) > (SELECT total 
                      FROM (SELECT a.name act_name, SUM(o.standard_qty) tot_std, SUM(o.total) total
                            FROM accounts a
                            JOIN orders o
                            ON o.account_id = a.id
                            GROUP BY 1
                            ORDER BY 2 DESC
                            LIMIT 1) inner_tab)
                ) counter_tab;

### Q4
For the customer that spent the most (in total over their lifetime as a customer) total_amt_usd, how many web_events did they have for each channel?

### A4
    SELECT w.channel,a.name, COUNT(*)
    FROM web_events w
    JOIN accounts a
    ON w.account_id = a.id
    GROUP BY 1,2
    HAVING a.name =
      (SELECT name
      FROM (SELECT a.name, SUM(o.total_amt_usd) total_usd
           FROM accounts a
           JOIN orders o
           ON a.id=o.account_id
           GROUP BY 1 
           ORDER BY 2 DESC
           LIMIT 1) sub1
      )
      
### Q5
What is the lifetime average amount spent in terms of total_amt_usd for the top 10 total spending accounts?

### A5
    SELECT AVG(total_usd) avg_top10
    FROM (SELECT a.name, SUM(o.total_amt_usd) total_usd
          FROM accounts a
          JOIN orders o
          ON a.id=o.account_id
          GROUP BY 1
          ORDER BY 2 DESC
          LIMIT 10) sub1

### Q6
What is the lifetime average amount spent in terms of total_amt_usd for only the companies that spent more than the average of all orders.

### A6
    SELECT a.name, SUM(o.total_amt_usd) total_usd 
    FROM accounts a
    JOIN orders o
    ON a.id=o.account_id
    GROUP BY 1
    HAVING SUM(o.total_amt_usd)>
      (SELECT AVG(total_usd) avg_total
      FROM (SELECT a.name, SUM(o.total_amt_usd) total_usd 
            FROM accounts a
            JOIN orders o
            ON a.id=o.account_id
            GROUP BY 1) sub1
       )

























