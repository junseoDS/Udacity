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
    
### LOWER & UPPER
