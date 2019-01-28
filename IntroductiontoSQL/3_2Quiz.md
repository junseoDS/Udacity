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

