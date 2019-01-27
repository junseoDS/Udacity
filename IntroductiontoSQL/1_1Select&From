SELECT : Filling out a form to get a set of result
  indicates which columns
  
FROM : Tells the query which table to use
  specifies from which tables

"*" : ALL

-Upper and lower does not matter ,but upper makes easy to read.

LIMIT : see just first few rows
  write it on last
  
ORDER BY : to sort our result using the data in any column
          only temporary, doesn't change data
          default is ascending order
          DESC is descending order
         
WHERE : allows you to filter a set of results based on specific criteria
    operators : > , < , >= , <= , = , !=
    can used with non-numeric data with =, !=
    use single quotes ' '
    
Arithmetric Operators
    Derived Column : a new column that is a manipulation of the existing colums in your database.
    AS : alias, give a name
    * : multiplication
    + : addition
    - : substraction
    / : division

Logical Operators
  LIKE : similiar to using WHERE and = , but for not knowing exactly what you are looking for / used with %
  IN   :                "              , but for more than one condition  /more than one item of that particular column
  NOT  : used with LIKE, IN
  AND & BETWEEN : combine operation where all condition must be true    /multiple criteria
  OR : combine operation where at least one condition must be true      / combine multiple statements
  
  
  
  
  
  
  
Statement      	How to Use It	                  Other Details
SELECT	        SELECT Col1, Col2, ...	        Provide the columns you want
FROM	          FROM Table	                    Provide the table where the columns exist
LIMIT	          LIMIT 10	                      Limits based number of rows returned
ORDER BY	      ORDER BY Col	                  Orders table based on the column. Used with DESC.
WHERE	          WHERE Col > 5	                  A conditional statement to filter your results
LIKE	          WHERE Col LIKE '%me%'	          Only pulls rows where column has 'me' within the text
IN	            WHERE Col IN ('Y', 'N')   	    A filter for only rows with column of 'Y' or 'N'
NOT	            WHERE Col NOT IN ('Y', 'N')	    NOT is frequently used with LIKE and IN
AND	            WHERE Col1 > 5 AND Col2 < 3	    Filter rows where two or more conditions must be true
OR	            WHERE Col1 > 5 OR Col2 < 3	    Filter rows where at least one condition must be true
BETWEEN	        WHERE Col BETWEEN 3 AND 5	      Often easier syntax than using an AND
