## Quiz LEFT & RIGHT

### Q1
In the accounts table, there is a column holding the website for each company. The last three digits specify what type of web address they are using. A list of extensions (and pricing) is provided here. Pull these extensions and provide how many of each website type exist in the accounts table.

### A1
    SELECT RIGHT(website ,3) extention, COUNT(*)
    FROM accounts
    GROUP BY 1

### Q2
There is much debate about how much the name (or even the first letter of a company name) matters. Use the accounts table to pull the first letter of each company name to see the distribution of company names that begin with each letter (or number). 

### A2
    SELECT LEFT(name,1), COUNT(*)
    FROM accounts
    GROUP BY 1 
    ORDER BY 2 DESC
    
### Q3
Use the accounts table and a CASE statement to create two groups: one group of company names that start with a number and a second group of those company names that start with a letter. What proportion of company names start with a letter?

### A3
        WITH t1 AS (SELECT name, CASE WHEN LEFT(name,1) >='0' AND LEFT(name,1) <='9' THEN 'number' 
                                      WHEN LEFT(name,1) >='A' AND LEFT(name,1)<='Z' THEN 'letter' END AS type
                    FROM accounts)

        SELECT type, COUNT(*)
        FROM t1
        GROUP BY 1

### Given answer A3
    SELECT SUM(num) nums, SUM(letter) letters
    FROM (SELECT name, CASE WHEN LEFT(UPPER(name), 1) IN ('0','1','2','3','4','5','6','7','8','9') 
                           THEN 1 ELSE 0 END AS num, 
             CASE WHEN LEFT(UPPER(name), 1) IN ('0','1','2','3','4','5','6','7','8','9') 
                           THEN 0 ELSE 1 END AS letter
          FROM accounts) t1;

### Q4
Consider vowels as a, e, i, o, and u. What proportion of company names start with a vowel, and what percent start with anything else?

 ### A4
        WITH t1 AS (SELECT name, CASE WHEN LEFT(name,1) IN ('A','E','I','O','U') THEN 'vowel' 
                                 ELSE 'not vowel' END AS type
                    FROM accounts)

        SELECT type, COUNT(*)
        FROM t1
        GROUP BY 1

## Quiz POSITION, STRPOS

### Q1
Use the accounts table to create first and last name columns that hold the first and last names for the primary_poc. 

### A1
    SELECT primary_poc, STRPOS(primary_poc,' '),LEFT(primary_poc,STRPOS(primary_poc,' ')) AS frist, RIGHT(primary_poc, LENGTH(primary_poc)- STRPOS(primary_poc,' '))  AS last
    FROM accounts


### Q2
Now see if you can do the same thing for every rep name in the sales_reps table. Again provide first and last name columns.

### A2
    SELECT name, STRPOS(name,' '), LEFT(name,STRPOS(name,' ')) AS first, RIGHT(name, LENGTH(name)-STRPOS(name,' '))  AS last
    FROM sales_reps

## Quiz CONCAT

### Q1
Each company in the accounts table wants to create an email address for each primary_poc. The email address should be the first name of the primary_poc . last name primary_poc @ company name .com.

### A1
    SELECT primary_poc, LEFT(primary_poc,STRPOS(primary_poc,' '))||'.'||RIGHT(primary_poc, LENGTH(primary_poc)- STRPOS(primary_poc,' '))||'@'||name||'.com'
    FROM accounts

### Q2
You may have noticed that in the previous solution some of the company names include spaces, which will certainly not work in an email address. See if you can create an email address that will work by removing all of the spaces in the account name, but otherwise your solution should be just as in question 1.


### A2
    WITH t1 AS (
     SELECT LEFT(primary_poc,     STRPOS(primary_poc, ' ') -1 ) first_name,  RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name
     FROM accounts)
    SELECT first_name, last_name, CONCAT(first_name, '.', last_name, '@', REPLACE(name, ' ', ''), '.com')
    FROM  t1

### Q3
We would also like to create an initial password, which they will change after their first log in. The first password will be the first letter of the primary_poc's first name (lowercase), then the last letter of their first name (lowercase), the first letter of their last name (lowercase), the last letter of their last name (lowercase), the number of letters in their first name, the number of letters in their last name, and then the name of the company they are working with, all capitalized with no spaces.


### A3

    WITH t1 AS (SELECT primary_poc, name,LEFT(primary_poc,STRPOS(primary_poc,' ')-1) AS first, RIGHT(primary_poc, LENGTH(primary_poc)- STRPOS(primary_poc,' ')) AS last
    FROM accounts)

    SELECT primary_poc, LOWER(LEFT(first,1)||RIGHT(first,1)||LEFT(last,1)||RIGHT(last,1)||LENGTH(first)||LENGTH(last))||UPPER(name)
    FROM t1


#### Quiz CAST
    WITH t1 AS (SELECT LEFT(date,2) AS month, LEFT(SUBSTR(date,4),2) AS day, LEFT(SUBSTR(date,7),4) AS years

    FROM sf_crime_data)
    SELECT CAST(years||month||day AS date)
    FROM t1



#### Quiz COALESCE
    SELECT COALESCE(a.id, a.id) filled_id, a.name, a.website, a.lat, a.long, a.primary_poc, a.sales_rep_id, COALESCE(o.account_id, a.id) account_id, o.occurred_at, COALESCE(o.standard_qty, 0) standard_qty, COALESCE(o.gloss_qty,0) gloss_qty, COALESCE(o.poster_qty,0) poster_qty, COALESCE(o.total,0) total, COALESCE(o.standard_amt_usd,0) standard_amt_usd, COALESCE(o.gloss_amt_usd,0) gloss_amt_usd, COALESCE(o.poster_amt_usd,0) poster_amt_usd, COALESCE(o.total_amt_usd,0) total_amt_usd
    FROM accounts a
    LEFT JOIN orders o
    ON a.id = o.account_id;
