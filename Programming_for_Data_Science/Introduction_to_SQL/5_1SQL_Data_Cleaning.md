# LESSON 5 : SQL DATA Cleaning

### Intro

> 1. Clean and re-stucture messy data
> 2. Convert columns to different data type
> 3. Tricks for manipulating Nulls

## LEFT & RIGHT

#### LEFT

> Pull characters from the left side of the string and present them as a separate string

    LEFT(column_name , #)

#### RIGHT

> Pull characters from the right side of the string and present them as a separate string

    RIGHT(column_name , #)

They can have nested function, runs inner to outer.

### LENGTH

> Pulls the length of the string

    LENGTH(column_name)
    

### POSITION

> Provides the position of a string counting from the left.

    POSITION( ' particular character ' IN column_name )
    
### STRPOS

> Provides the position of a string counting from the left, 
>
> Same with POSITION, but diffent grammar

    STRPOS(column_name , ' particular character')
    
Both POSITION and STRPOS are *case sensitive

So looking for A is different than looking for a.
    
### LOWER & UPPER

> If you want to pull an index regardless of the case of letter, 
>
> you should use LOWER or UPPER to make all of the characters lower or uppercase


### CONCAT

> combines values from several columns into one column
>
> or you can do it with piping ||

    CONCAT(first,' ',last)

OR

    first||' '||last
    
### CAST

> allows us to change columns from one data type to another.
>
> DATE_PART('month', TO_DATE(month, 'month')) here changed a month name into the number associated with that particular month.

    CAST(year_data||month_data||day_data AS date)

OR

    (year_data||month_data||day_data) :: date

### COALESCE

> returns the first non-NULL value passed for each row







