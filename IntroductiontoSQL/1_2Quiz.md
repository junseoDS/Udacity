# Your Fist Queries in SQL

SELECT id, account_id, occurred_at
FROM orders

## Quiz LIMIT
### Q1
''' Try using LIMIT yourself below by writing a query that displays all the data in the occurred_at, account_id, 
and channel columns of the web_events table, and limits the output to only the first 15 rows.
'''

### A1
SELECT occurred_at, account_id,channel
FROM web_events
LIMIT 15

## Quiz ORDER BY
### Q1
'''Write a query to return the 10 earliest orders in the orders table. Include the id, occurred_at, and total_amt_usd.'''

### A1
SELECT id, occurred_at, total_amt_usd
FROM orders
ORDER BY occurred_at
LIMIT 10

### Q2
'''Write a query to return the top 5 orders in terms of largest total_amt_usd. Include the id, account_id, and total_amt_usd.'''

### A2
SELECT id, account_id, total_amt_usd
FROM orders
ORDER BY total_amt_usd DESC 
LIMIT 5

### Q3
'''Write a query to return the lowest 20 orders in terms of smallest total_amt_usd. Include the id, account_id, and total_amt_usd.'''

### A3
SELECT id, account_id, total_amt_usd
FROM orders
ORDER BY total_amt_usd DESC 
LIMIT 5

## Quiz ORDER BY 2 
### Q1
'''Write a query that displays the order ID, account ID, and total dollar amount for all the orders, 
sorted first by the account ID (in ascending order), and then by the total dollar amount (in descending order). '''

### A1
SELECT id, account_id, total_amt_usd
FROM orders
ORDER BY account_id, total_amt_usd DESC

### Q2
'''Now write a query that again displays order ID, account ID, and total dollar amount for each order, 
but this time sorted first by total dollar amount (in descending order), and then by account ID (in ascending order). '''

### A2
SELECT id, account_id, total_amt_usd
FROM orders
ORDER BY total_amt_usd DESC, account_id

### Q3
'''Compare the results of these two queries above. 
How are the results different when you switch the column you sort on first?'''

### A3
"""
In query #1, all of the orders for each account ID are grouped together, and then within each of those groupings,
the orders appear from the greatest order amount to the least. In query #2, since you sorted by the total dollar amount first, 
the orders appear from greatest to least regardless of which account ID they were from. 
Then they are sorted by account ID next. (The secondary sorting by account ID is difficult to see here, since only if there were two orders with equal total dollar amounts would there need to be any sorting by account ID.)
"""

## Quiz WHERE
### Q1
'''
Pulls the first 5 rows and all columns from the orders table that have a dollar amount of gloss_amt_usd greater than or equal to 1000.
'''
### A1
SELECT *
FROM orders
WHERE gloss_amt_usd >= 1000
LIMIT 5

### Q2
'''
Pulls the first 10 rows and all columns from the orders table that have a total_amt_usd less than 500.
'''
### A2
SELECT *
FROM orders
WHERE total_amt_usd < 500
LIMIT 10

## Quiz WHERE with non-numeric
### Q1
'''Filter the accounts table to include the company name, website, 
and the primary point of contact (primary_poc) just for the Exxon Mobil company in the accounts table.'''

### A1
SELECT name, website,primary_poc
FROM accounts
WHERE website ='www.exxonmobil.com'

### A1-1
SELECT name, website, primary_poc
FROM accounts
WHERE name = 'Exxon Mobil'

## Quiz Arithmetic Operators
  
### Q1
'''Create a column that divides the standard_amt_usd by the standard_qty to find the unit price for standard paper for each order. 
Limit the results to the first 10 orders, and include the id and account_id fields. '''
### A1
SELECT id, account_id, standard_amt_usd/standard_qty AS unit_price
FROM orders
LIMIT 10

### Q2
'''Write a query that finds the percentage of revenue that comes from poster paper for each order. 
You will need to use only the columns that end with _usd. (Try to do this without using the total column.) 
Display the id and account_id fields also. 
NOTE - you will receive an error with the correct solution to this question. 
This occurs because at least one of the values in the data creates a division by zero in your formula.
You will learn later in the course how to fully handle this issue. 
For now, you can just limit your calculations to the first 10 orders, as we did in question #1,
and you'll avoid that set of data that causes the problem. 

### A2
SELECT *,poster_amt_usd/(standard_amt_usd+gloss_amt_usd+poster_amt_usd) AS percent_of_poster
FROM orders
LIMIT 10

## Quiz LIKE

### Q1
'''All the companies whose names start with 'C'. '''
### A1
SELECT name
FROM accounts
WHERE name LIKE 'C%'

### Q2
'''All companies whose names contain the string 'one' somewhere in the name.'''
### A2
SELECT name
FROM accounts
WHERE name LIKE '%one%'

### Q3
'''All companies whose names end with 's'.'''
### A3
SELECT *
FROM accounts
WHERE name LIKE '%s'


## Quiz IN
### Q1
'''Use the accounts table to find the account name, primary_poc, and sales_rep_id for Walmart, Target, and Nordstrom.'''
### A2
SELECT name, primary_poc, sales_rep_id
FROM accounts
WHERE name IN ('Walmart', 'Target', 'Nordstrom')

### Q2
'''Use the web_events table to find all information regarding individuals who were contacted via the channel of organic or adwords.'''

### A2
SELECT *
FROM web_events
WHERE channel IN ('organic','adwords')

### Q1
### 1-1
'''Use the accounts table to find the account name, primary poc, and sales rep id for all stores except Walmart, Target, and Nordstrom.'''
### A1-1
SELECT name, primary_poc, sales_rep_id
FROM accounts
WHERE name NOT IN ('Walmart', 'Target', 'Nordstrom')
### 1-2
'''Use the web_events table to find all information regarding individuals 
who were contacted via any method except using organic or adwords methods.'''
### A1-2
SELECT *
FROM web_events
WHERE channel NOT IN ('organic', 'adwords')

### Q2
Use the accounts table to find:
### 2-1
'''All the companies whose names do not start with 'C'.'''
### A2-1
SELECT name
FROM accounts
WHERE name NOT LIKE 'C%'

### 2-2
'''All companies whose names do not contain the string 'one' somewhere in the name.'''
### A2-2
SELECT name
FROM accounts
WHERE name NOT LIKE '%one%'

### 2-3
'''All companies whose names do not end with 's' '''
### A2-3
SELECT *
FROM accounts
WHERE name NOT LIKE '%s'


## Quiz BETWEEN and AND

### Q1
'''Write a query that returns all the orders where the standard_qty is over 1000,
the poster_qty is 0, and the gloss_qty is 0.'''

### A1
SELECT *
FROM orders
WHERE standard_qty > 1000 AND poster_qty = 0 AND gloss_qty = 0

### Q2
'''Using the accounts table, find all the companies whose names do not start with 'C' and end with 's'. '''

### A2
SELECT name
FROM accounts
WHERE name NOT LIKE 'C%' AND name LIKE '%s'

### Q3
'''When you use the BETWEEN operator in SQL, do the results include the values of your endpoints, or not? 
Figure out the answer to this important question by writing a query that displays the order date and gloss_qty data for all orders where gloss_qty is between 24 and 29. 
Then look at your output to see if the BETWEEN operator included the begin and end values or not.'''

### A3
SELECT occurred_at, gloss_qty 
FROM orders
WHERE gloss_qty BETWEEN 24 AND 29

### Q4
'''Use the web_events table to find all information regarding individuals who were contacted via the organic or adwords channels,
and started their account at any point in 2016, sorted from newest to oldest.'''

### A4
SELECT *
FROM web_events
WHERE channel IN ('organic','adwords') AND occurred_at BETWEEN 


### Quiz OR

### Q1
'''Find list of orders ids where either gloss_qty or poster_qty is greater than 4000.
Only include the id field in the resulting table.'''

### A1
SELECT id
FROM orders
WHERE gloss_qty > 4000 OR poster_qty > 4000


### Q2
'''Write a query that returns a list of orders where the standard_qty is zero and either the gloss_qty or poster_qty is over 1000.'''

### A2
SELECT *
FROM orders
WHERE standard_qty = 0 AND (gloss_qty > 1000 OR poster_qty > 1000)

### Q3
'''Find all the company names that start with a 'C' or 'W', and the primary contact contains 'ana' or 'Ana',
but it doesn't contain 'eana'.'''

### A3
SELECT *
FROM accounts
WHERE name LIKE 'C%' or name LIKE 'W%'
AND primary_poc LIKE '%ana%' or primary_poc LIKE 'Ana%' or primary_poc NOT LIKE '%eana%'




