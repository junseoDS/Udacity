# Investigate a Relation Database

## overview

> The project submission is a presentation, which will be reviewed, and for which you will need to Meet Expectations to pass. 
>
> For the presentation component, you will create four slides. Each slide will:
>
>> Have a question of interest.
>>
>> Have a supporting SQL query needed to answer the question.
>>
>> Have a supporting visualization created using the final data of your SQL query that answers your question of interest.
>>
> You will submit your project at the end of the project lessons. Your project will include:
>
>> A set of slides with a question, visualization, and small summary on each slide.
>>
>> A text file with your queries needed to answer each of the four questions.
>>
> The essentials of your project submission are discussed on the page labeled as Project Submission.



## Practice Quiz 1

### Q1
Let's start with creating a table that provides the following details:
actor's first and last name combined as full_name, film title, film description and length of the movie.

How many rows are there in the table?

### A1

    SELECT ac.first_name, ac.last_name,ac.first_name||' '||  ac.last_name AS full_name, f.title, f.description, f.length
    FROM actor ac
    JOIN film_actor fa ON ac.actor_id = fa.actor_id
    JOIN film f ON f.film_id = fa.film_id
        
### Q2
Write a query that creates a list of actors and movies where the movie length was more than 60 minutes. 
How many rows are there in this query result?

### A2
    SELECT ac.first_name, ac.last_name,ac.first_name||' '||  ac.last_name AS full_name, f.title, f.description, f.length
    FROM actor ac
    JOIN film_actor fa ON ac.actor_id = fa.actor_id
    JOIN film f ON f.film_id = fa.film_id
    WHERE f.length > 60

### Q3
Write a query that captures the actor id, full name of the actor, and counts the number of movies each actor has made. 
(HINT: Think about whether you should group by actor id or the full name of the actor.) 
Identify the actor who has made the maximum number movies.

### A3
    SELECT ac.actor_id,ac.first_name||' '||  ac.last_name AS full_name, COUNT(*)
    FROM actor ac
    JOIN film_actor fa ON ac.actor_id = fa.actor_id
    JOIN film f ON f.film_id = fa.film_id
    GROUP BY 1,2
    ORDER BY 3 DESC

## Practice Quiz 2

### Q1
Write a query that displays a table with 4 columns:
actor's full name, film title, length of movie, and a column name "filmlen_groups" that classifies movies based on their length. 
Filmlen_groups should include 4 categories: 1 hour or less, Between 1-2 hours, Between 2-3 hours, More than 3 hours.

Match the filmlen_groups with the movie titles in your result dataset.

### A1
    SELECT 
    ac.first_name||' '||ac.last_name AS actor_name, 
    f.title, f.length, 
    CASE WHEN f.length <=60 THEN '1 hour or less' 
         WHEN f.length >60 AND f.length <=120 THEN 'Between 1-2 hours' 
         WHEN f.length >120 AND f.length <=180 THEN 'Between 2-3 hours' 
         WHEN f.length >180 THEN 'More than 3 hours' 
         END AS Filmlen_groups
    FROM actor ac
    JOIN film_actor fa ON ac.actor_id = fa.actor_id
    JOIN film f ON f.film_id = fa.film_id


### Q2

Now, we bring in the advanced SQL query concepts! 
Revise the query you wrote above to create a count of movies in each of the 4 filmlen_groups:
1 hour or less, Between 1-2 hours, Between 2-3 hours, More than 3 hours.

Match the count of movies in each filmlen_group., 

### A2
    WITH table1 AS (SELECT f.title, f.length, 
    CASE WHEN f.length <=60 THEN '1 hour or less' 
         WHEN f.length >60 AND f.length <=120 THEN 'Between 1-2 hours' 
         WHEN f.length >120 AND f.length <=180 THEN 'Between 2-3 hours' 
         WHEN f.length >180 THEN 'More than 3 hours' 
         END AS Filmlen_groups
    FROM film f
    )

    SELECT Filmlen_groups, COUNT(*)
    FROM table1
    GROUP BY 1

## Question Set 1

### Question 1
We want to understand more about the movies that families are watching.
The following categories are considered family movies: Animation, Children, Classics, Comedy, Family and Music.

Create a query that lists each movie, the film category it is classified in, and the number of times it has been rented out.

#### Check Your Solution

For this query, you will need 5 tables: Category, Film_Category, Inventory, Rental and Film.
Your solution should have three columns: Film title, Category name and Count of Rentals.

The following table header provides a preview of what the resulting table should look like if you order by category name followed by the film title.

HINT: One way to solve this is to create a count of movies using aggregations, subqueries and Window functions.


### Answer 1

    SELECT DISTINCT(f.title) film_title, c.name category_name,  COUNT(r.rental_date) AS rental_count
    FROM category c
    JOIN film_category fc ON c.category_id=fc.category_id
    JOIN film f ON fc.film_id=f.film_id
    JOIN inventory i ON f.film_id=i.film_id
    JOIN rental r ON r.inventory_id=i.inventory_id
    WHERE c.name ='Animation' OR c.name ='Children' OR c.name ='Classics' OR c.name ='Comedy' OR c.name= 'Family' OR c.name= 'Music'
    GROUP BY 1,2
    ORDER BY c.name


### Question 2
Now we need to know how the length of rental duration of these family-friendly movies compares to the duration that all movies are rented for.
Can you provide a table with the movie titles and divide them into 4 levels (first_quarter, second_quarter, third_quarter, and final_quarter) based on the quartiles (25%, 50%, 75%) of the rental duration for movies across all categories? 
Make sure to also indicate the category that these family-friendly movies fall into.

#### Check Your Solution
The data are not very spread out to create a very fun looking solution, but you should see something like the following if you correctly split your data. 
You should only need the category, film_category, and film tables to answer this and the next questions. 

HINT: One way to solve it requires the use of percentiles, Window functions, subqueries or temporary tables.

### Answer 2

    SELECT film_title, category,rental_duration, NTILE(4) OVER (ORDER BY rental_duration) standard_quartile
    FROM
    (SELECT DISTINCT(f.title) film_title, c.name category, f.rental_duration rental_duration
    FROM category c
    JOIN film_category fc ON c.category_id=fc.category_id
    JOIN film f ON fc.film_id=f.film_id
    JOIN inventory i ON f.film_id=i.film_id
    JOIN rental r ON r.inventory_id=i.inventory_id
    ORDER BY 3
    ) table1
    WHERE category IN ('Animation','Children','Classics' ,'Comedy','Family','Music')

### Question 3
Finally, provide a table with the family-friendly film category, each of the quartiles, and the corresponding count of movies within each combination of film category for each corresponding rental duration category. 

The resulting table should have three columns:

* Category

* Rental length category

* Count

#### Check Your Solution
The following table header provides a preview of what your table should look like.
The Count column should be sorted first by Category and then by Rental Duration category.

HINT: One way to solve this question requires the use of Percentiles, Window functions and Case statements.

### Answer 3

    SELECT category, standard_quartile, COUNT(film_title) count
    FROM
    (SELECT film_title, category,rental_duration, NTILE(4) OVER (ORDER BY rental_duration) standard_quartile
    FROM
    (SELECT DISTINCT(f.title) film_title, c.name category, f.rental_duration rental_duration
    FROM category c
    JOIN film_category fc ON c.category_id=fc.category_id
    JOIN film f ON fc.film_id=f.film_id
    JOIN inventory i ON f.film_id=i.film_id
    JOIN rental r ON r.inventory_id=i.inventory_id
    ORDER BY 3
    ) table1
    WHERE category IN ('Animation','Children','Classics' ,'Comedy','Family','Music')) table2
    GROUP BY 1,2
    ORDER BY 1,2
    
    
    

## Question Set 2

### Question 1
We want to find out how the two stores compare in their count of rental orders during every month for all the years we have data for.
Write a query that returns the store ID for the store, the year and month and the number of rental orders each store has fulfilled for that month. 
Your table should include a column for each of the following:
year, month, store ID and count of rental orders fulfilled during that month.

#### Check Your Solution
The following table header provides a preview of what your table should look like. 
The count of rental orders is sorted in descending order.

HINT: One way to solve this query is the use of aggregations.

### Answer 1

    SELECT DATE_PART('month',r.rental_date) rental_month, DATE_PART('year',r.rental_date) rental_year,s.store_id, COUNT(*)
    FROM rental r
    JOIN staff sf ON r.staff_id=sf.staff_id
    JOIN store s ON s.store_id=sf.store_id
    GROUP BY 1,2,3
    ORDER BY 2,1,3


### Question 2
We would like to know who were our top 10 paying customers, how many payments they made on a monthly basis during 2007, and what was the amount of the monthly payments.
Can you write a query to capture the customer name, month and year of payment, and total payment amount for each month by these top 10 paying customers?

#### Check your Solution:
The following table header provides a preview of what your table should look like. 
The results are sorted first by customer name and then for each month. 
As you can see, total amounts per month are listed for each customer.

HINT: One way to solve is to use a subquery, limit within the subquery, and use concatenation to generate the customer name.

### Question 3
Finally, for each of these top 10 paying customers, I would like to find out the difference across their monthly payments during 2007. 
Please go ahead and write a query to compare the payment amounts in each successive month. 
Repeat this for each of these 10 paying customers. 
Also, it will be tremendously helpful if you can identify the customer name who paid the most difference in terms of payments.

#### Check your solution:
The customer Eleanor Hunt paid the maximum difference of $64.87 during March 2007 from $22.95 in February of 2007.

HINT: You can build on the previous questions query to add Window functions and aggregations to get the solution.



