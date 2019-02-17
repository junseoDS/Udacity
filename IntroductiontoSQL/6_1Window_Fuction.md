# LESSON 6 : WINDOW FUNCTION

### Window function

> Window function does not come with GROUP BY function even you use aggregating function.


    aggregating_function OVER ( ) new_name
    
    aggregatinf_function OVER (PARTITION BY -- ORDER BY --) new_name
    
### ROW_NUMBER()

> counts row by adding 1.
>
> looks similiar to index or id or rank

> RANK() and ROW_NUMBER() looks similiar
>
> difference is when there are same values,
>
> RANK gives same numbers
>
> ROW_NUMBER gives different numbers
>
> DENSE_RANK() dose not skip numbers evne when same ranks exist.

### Aliase for multiple windows.

    WINDOW __ AS (__)
    
### LAG & LEAD

> Comparing a row to previous row.
>
> LAG return the value from *previous row .
>
> LEAD return the value from *following row .

    LAG(__) OVER (__)
    LEAD(__) OVER (__)

### NTILE

> gives you percentile or ntile.

     NTILE(*# of buckets*) OVER (__)









