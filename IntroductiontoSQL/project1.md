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
(HINT: Think about whether you should group by actor id or the full name of the actor.) Identify the actor who has made the maximum number movies.

### A3
    SELECT ac.actor_id,ac.first_name||' '||  ac.last_name AS full_name, COUNT(*)
    FROM actor ac
    JOIN film_actor fa ON ac.actor_id = fa.actor_id
    JOIN film f ON f.film_id = fa.film_id
    GROUP BY 1,2
    ORDER BY 3 DESC

## Practice Quiz 2

### Q1
Write a query that displays a table with 4 columns: actor's full name, film title, length of movie, and a column name "filmlen_groups" that classifies movies based on their length. Filmlen_groups should include 4 categories: 1 hour or less, Between 1-2 hours, Between 2-3 hours, More than 3 hours.

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

Now, we bring in the advanced SQL query concepts! Revise the query you wrote above to create a count of movies in each of the 4 filmlen_groups: 1 hour or less, Between 1-2 hours, Between 2-3 hours, More than 3 hours.

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
We want to understand more about the movies that families are watching. The following categories are considered family movies: Animation, Children, Classics, Comedy, Family and Music.

Create a query that lists each movie, the film category it is classified in, and the number of times it has been rented out.

#### Check Your Solution
For this query, you will need 5 tables: Category, Film_Category, Inventory, Rental and Film. Your solution should have three columns: Film title, Category name and Count of Rentals.

The following table header provides a preview of what the resulting table should look like if you order by category name followed by the film title.

HINT: One way to solve this is to create a count of movies using aggregations, subqueries and Window functions.


### Answer 1


### Question 2
Now we need to know how the length of rental duration of these family-friendly movies compares to the duration that all movies are rented for. Can you provide a table with the movie titles and divide them into 4 levels (first_quarter, second_quarter, third_quarter, and final_quarter) based on the quartiles (25%, 50%, 75%) of the rental duration for movies across all categories? Make sure to also indicate the category that these family-friendly movies fall into.

#### Check Your Solution
The data are not very spread out to create a very fun looking solution, but you should see something like the following if you correctly split your data. You should only need the category, film_category, and film tables to answer this and the next questions. 

HINT: One way to solve it requires the use of percentiles, Window functions, subqueries or temporary tables.

### Answer 2


### Question 3
Finally, provide a table with the family-friendly film category, each of the quartiles, and the corresponding count of movies within each combination of film category for each corresponding rental duration category. The resulting table should have three columns:

* Category

* Rental length category

* Count

#### Check Your Solution
The following table header provides a preview of what your table should look like. The Count column should be sorted first by Category and then by Rental Duration category.

HINT: One way to solve this question requires the use of Percentiles, Window functions and Case statements.













