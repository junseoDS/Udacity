# LESSON 4 : SQL Subqueries & Temporary Tables

### Intro

We will learn

> 1. Subquries
> 2. Tables Expressions
> 3. Persistent Derived Tables

## Subqueries

> Aloow you to answer more complex question than you can do with a single database table.


#### Subquery Formatting

> Provide some way for the reader to easily determine which parts of the query will be excuted together.
>
> By indenting the subqueries
>
> You can make subqueries as a table, a column or an individual value.
>
> If you are only returning a single value, you might use it for logical statement.

    ( 
    SUBQUERY
    ) AS name_of_sub

### WITH

> WITH is often called a common talbe expression or CTE.
>
> Declare new table on the top of the codes and only once.
>
> Works as if it is any other table in database.
> 
> Faster, more readable than subquery.

    WITH CTE_name AS ( 
    SUBQUERIES
    )
