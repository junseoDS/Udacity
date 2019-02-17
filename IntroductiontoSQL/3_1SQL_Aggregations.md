# LESSION 3 : SQL Aggregations

## intro

What I learned earlier

> * Concept 1 : LEFT, RIGHT and INNER JOINs
> * Concept 2 : Filter result with WHERE and ON clauses

Somtimes row-level data could be overwhlming

A single number (aggregated) can be more valuable

Similiar syntax with Exel

> COUNT, SUM, MIN, MAX, AVERAGE
> Operating down columns

### NULL

Null : No data

to find null

    WHERE --- IS NULL
    
or

    WHERE --- NOT NULL

Null is not value, *it's property of data

Two common ways in which I'm likely to encounter NULLs :

> * Performing LEFT or RIGHT JOIN
> * Simply missing data in database

## COUNT

COUNT : counts the number of rows in a table

    SELECT COUNT(*)
    FROM accounts
    
Count the number of non-null records ; looking for non-null data and *text is not null

## SUM

> only can be used on numeric columns
>
> ignore null values
    

## MIN,MAX

> can be used on numerical and non-numerical columns
>
> For MIN, the lowest number, earliest date, or early in the alphabet
    
## AVG

> can be used on numerical columns ,     
>
> ignores null

## GROUP BY

GROUP BY 

> allows creating segments that will aggregate independent from one another
>
> goes in between the WHERE and ORDER BY 
>
> any column in the SELECT statement that is not with aggregatoe must be in the GROUP BY clause.
>
> ORDER BY works like SORT in spreadsheet
>
> multipe columns at once
>
> Reference the columns in SELECT statements in GROUP BY, ORDER BY clause with numbers (1,2,...)

## DISTINCT 

> group by some columns but not necessarily include aggregations


## HAVING 

> Clean way to filter a query that's been aggregated
>
> WHERE cannot filter on aggregate columns
>
> Syntax is same with WHERE

## DATE FUNCTION

For US

    MM DD YY
    
or any other

    DD MM YY
    
In the Database

    YYYY MM DD
    
> Easy to find most recent or oldest
>
> Easily truncated in order to group them for analysis

### DATE_TRUNC

> Truncate date to particular part of date-time column.
>
> 'day', 'month', 'year' , 'dow' for day of the week

### DATE_PART

> Pulling a specific portion of a date.



## CASE

> Handles "if", "then" logic
>
> goes in the SELECT clause
>
> must include WHEN, THEN, END. ELSE is optional
>
> can make any conditional statement using any conditional operation between WHEN and THEN
>
>> WHEN : condition
>> THEN : if given condition is fitted, mark new name.
>> END : closing CASE statement.








