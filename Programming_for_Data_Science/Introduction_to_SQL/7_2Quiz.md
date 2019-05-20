# Advanced JOINs

## Quiz FULL OUTER JOIN

### Q
> each account who has a sales rep and each sales rep that has an account (all of the columns in these returned rows will be full)
> but also each account that does not have a sales rep and each sales rep that does not have an account (some of the columns in these returned rows will be empty)

### A
    SELECT a.name account, s.name sales_rep
    FROM accounts a
    FULL OUTER JOIN sales_reps s
    ON a.sales_rep_id=s.id
    WHERE a.name IS NULL OR s.name IS NULL

## Quiz JOINs with Comparison Operator

### Q
In the following SQL Explorer, write a query that left joins the accounts table and the sales_reps tables on each sale rep's ID number and joins it using the < comparison operator on accounts.primary_poc and sales_reps.name

### A
    SELECT a.name account_name, a.primary_poc primary_name, s.name sales_rep_name
    FROM accounts a
    LEFT JOIN sales_reps s
    ON a.sales_rep_id=s.id
    AND a.primary_poc < s.name
    
## Quiz Self JOINs

### Q

change the interval to 1 day to find those web events that occurred after, but not more than 1 day after, another web event
add a column for the channel variable in both instances of the table in your query


### A
    SELECT w1.id w1_id, w1.account_id w1_account, w1.occurred_at w1_occurred,w1.channel w1_channel, w2.id w2_id, w2.account_id w2_account,w2.occurred_at w2_occurred, w2.channel w2_channel
    FROM web_events w1
    LEFT JOIN web_events w2
    ON w1.account_id=w2.account_id
    AND w2.occurred_at > w1.occurred_at
    AND w2.occurred_at <= w1.occurred_at + INTERVAL '1 days'
    ORDER BY w1.account_id, w1.occurred_at
    

## Quiz Union

### Q1
Write a query that uses UNION ALL on two instances (and selecting all columns) of the accounts table. Then inspect the results and answer the subsequent quiz.

### A1

    WITH account_2 AS (SELECT *
    FROM accounts

    UNION ALL
    SELECT *
    FROM accounts)  

    SELECT *
    FROM account_2

### Q2
Add a WHERE clause to each of the tables that you unioned in the query above, filtering the first table where name equals Walmart and filtering the second table where name equals Disney. Inspect the results then answer the subsequent quiz.

### A2
    WITH account_2 AS (
    SELECT *
    FROM accounts
    WHERE name = 'Walmart'

    UNION ALL

    SELECT *
    FROM accounts
    WHERE name = 'Disney')  

    SELECT *
    FROM account_2

### Q3
Perform the union in your first query (under the Appending Data via UNION header) in a common table expression and name it double_accounts. Then do a COUNT the number of times a name appears in the double_accounts table. If you do this correctly, your query results should have a count of 2 for each name.

### A3

    WITH double_account AS (SELECT *
    FROM accounts

    UNION ALL
    SELECT *
    FROM accounts)  

    SELECT name, COUNT(*)
    FROM double_account
    GROUP BY 1


































