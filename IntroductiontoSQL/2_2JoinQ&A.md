## Your First JOIN

    SELECT orders.*
    FROM orders
    JOIN accounts
    ON orders.account_id = accounts.id


Q1.
Provide a table for all web_events associated with account name of Walmart. There should be three columns. Be sure to include the primary_poc, time of the event, and the channel for each event. Additionally, you might choose to add a fourth column to assure only Walmart events were chosen. 

A1.
SELECT web_events.* , accounts.name
FROM web_events
JOIN accounts
ON web_events.account_id=accounts.id
WHERE accounts.name = 'Walmart’


Q2.
Provide a table that provides the region for each sales_rep along with their associated accounts. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name. 

Wrong A2.
SELECT accounts.name,sales_reps.name , region.name
FROM accounts
JOIN sales_reps
ON accounts.sales_rep_id = sales_reps.id
JOIN region
ON sales_reps.region_id = region.id

A2.
SELECT sr.name SRname,r.name Rname,a.name Aname
FROM sales_reps AS sr
JOIN region AS r
ON  sr.region_id = r.id
JOIN accounts AS a
ON a.sales_rep_id=sr.id
=> should use alias when colnames are same

Q3.
Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. Your final table should have 3 columns: region name, account name, and unit price. A few accounts have 0 for total, so I divided by (total + 0.01) to assure not dividing by zero.


A3.
SELECT r.name AS region_name , a.name AS account_name, o.total_amt_usd/(o.total+0.01) AS unit_price
FROM orders AS o
JOIN accounts AS a
ON o.account_id=a.id
JOIN sales_reps AS sr
ON a.sales_rep_id=sr.id
JOIN region AS r
ON sr.region_id=r.id

Last Check
Q1.
Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

A1.
SELECT r.name Region, sr.name Sales_rep, a.name Account
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id = r.id
ORDER BY Account

Q2.
Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for accounts where the sales rep has a first name starting with S and in the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name. 

A2.SELECT r.name Region, sr.name Sales_rep, a.name Account
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id = r.id
WHERE sr.name LIKE 'S%' AND r.name ='Midwest'
ORDER BY Account

Q3.
Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for accounts where the sales rep has a last name starting with K and in the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

A3.SELECT r.name Region, sr.name Sales_rep, a.name Account
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id = r.id
WHERE sr.name LIKE '%K%' AND r.name ='Midwest'
ORDER BY Account

Q4.
Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100. Your final table should have 3 columns: region name, account name, and unit price. In order to avoid a division by zero error, adding .01 to the denominator here is helpful total_amt_usd/(total+0.01). 

A4.
SELECT r.name Region, a.name Account, o.total_amt_usd/(o.total +0.01) Unit_price
FROM orders o
JOIN accounts a
ON o.account_id=a.id
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id=r.id
WHERE o.standard_qty > 100

with 4509 results

Q5.
Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100 and the poster order quantity exceeds 50. Your final table should have 3 columns: region name, account name, and unit price. Sort for the smallest unit price first. In order to avoid a division by zero error, adding .01 to the denominator here is helpful (total_amt_usd/(total+0.01). 

SELECT r.name Region, a.name Account, o.total_amt_usd/(o.total +0.01) Unit_price
FROM orders o
JOIN accounts a
ON o.account_id=a.id
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id=r.id
WHERE o.standard_qty > 100 AND o.poster_qty>50
ORDER BY unit_price

with 835 results

Q6.
Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100 and the poster order quantity exceeds 50. Your final table should have 3 columns: region name, account name, and unit price. Sort for the largest unit price first. In order to avoid a division by zero error, adding .01 to the denominator here is helpful (total_amt_usd/(total+0.01). 

A7.
SELECT r.name Region, a.name Account, o.total_amt_usd/(o.total +0.01) Unit_price
FROM orders o
JOIN accounts a
ON o.account_id=a.id
JOIN sales_reps sr
ON a.sales_rep_id=sr.id
JOIN region r
ON sr.region_id=r.id
WHERE o.standard_qty > 100 AND o.poster_qty>50
ORDER BY unit_price DESC

with 835 results


Q7.
What are the different channels used by account id 1001? Your final table should have only 2 columns: account name and the different channels. You can try SELECT DISTINCT to narrow down the results to only the unique values.

A7.
SELECT we.channel Channel, a.name Account
FROM web_events we
JOIN accounts a
ON we.account_id = a.id
WHERE a.id = 1001

with 39 results

Q8.
Find all the orders that occurred in 2015. Your final table should have 4 columns: occurred_at, account name, order total, and order total_amt_usd.

A8.
SELECT o.occurred_at,a.name,o.total,o.total_amt_usd
FROM orders o
LEFT JOIN accounts a
ON a.id = o.account_id
WHERE o.occurred_at BETWEEN '01-01-2015' AND '01-01-2016'
ORDER BY o.occurred_at DESC

with 1725 results
