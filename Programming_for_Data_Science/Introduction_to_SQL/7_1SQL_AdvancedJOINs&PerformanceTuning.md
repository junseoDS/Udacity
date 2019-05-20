# LESSON 7 : SQL Advanced JOINs & Performance Tuning

## Advanced JOINs

### OUTER JOIN

    SELECT
    FROM A
    FULL OUTER JOIN B 
    ON 
    
    
To return unmatched rows only

    WHERE table_A.col_name IS NULL OR table_B.col_name IS NULL
    
 
### JOIN with Comparison Operators.

> Inequality operatiors don't need to be date or number, work on strings.

### SELF JOINs

> Same table, joinning it to itself by giving diffenct alias.

### UNION

> Appending data via UNION
>
> Two strict rules for appending data : 
>
>> 1. Both tables must have the same number of columns.
>>
>> 2. Those columns must have the same data types in the same order as the first table.
>
>
> UNION drops identical rows.
>
> for all values : UNION ALL

    SELECT *
        FROM accounts

    UNION ALL

    SELECT *
      FROM accounts

> Pretreat tables before doing UNION 
>
> Perfom operations with WITH
 
 
 
 


## Performing Tuning

Things that affect the number of calculations :
>
> * Table size
> * JOINs
> * Aggregations
>

Things that cannot control : 
>
> * Other users running queries concurrently on the database.
> * Database sofrware and optimization
>

### Filtering the data

LIMIT does not make time shorter.

Aggregations work first then LIMIT works.

So use subquery to limit.

> make join less complicated.
>
> reduce table size before join.
>
> pre-aggregation => subquery

    
