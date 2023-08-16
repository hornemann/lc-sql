---
title: Filtering
teaching: 20
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Write queries that `SELECT` data based on conditions, such as `AND`, `OR`, and `NOT`.
- Understand how to use the `WHERE` clause in a statement.
- Learn how to use comparison keywords such as `LIKE` in a statement.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I filter data?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Filtering

SQL is a powerful tool for filtering data in databases based on a set of conditions. Let's say we only want data for a specific year. To filter by a year, we will use the `WHERE` clause.

```sql
SELECT *
FROM filmsAndSeries
WHERE release_year = 2020;
```

We can add additional conditions by using `AND`, `OR`, and/or `NOT`. For example, suppose we want the films with more than 500000 votes:

```sql
SELECT *
FROM filmsAndSeries
WHERE type="MOVIE" AND imdb_votes > 500000;
```



If we want to get data for movies from two years, we can combine the tests using OR:

```sql
SELECT *
FROM filmsAndSeries
WHERE type = "MOVIE" AND (release_year = 2020 OR release_year = 2022);
```

Or we could use IN:

```sql
SELECT *
FROM filmsAndSeries
WHERE type = "MOVIE" AND release_year IN (2020 ,2022);
```

When you do not know the entire value you are searching for, you can use comparison keywords such as `LIKE`, `IN`, `BETWEEN...AND`, `IS NULL`. For instance, we can use `LIKE` in combination with `WHERE` to search for data that matches a pattern.

For example, using the `filmsAndSeries` table again, let's `SELECT` all of the data `WHERE` the `title` contains "Monty Python":

```sql
SELECT *
FROM filmsAndSeries
WHERE title LIKE '%monty python%';
```

You may have noticed the wildcard character `%`. It is used to match zero to many characters. So in the SQL statement above, it will match zero or more characters before and after 'Monty Python'.

Let's see what variations of the term we got. Notice uppercase and lowercase, the addition of 's' at the end of structures, etc.

To learn more about other comparison keywords you can use, see Beginner SQL Tutorial on [SQL Comparison Keywords](https://beginner-sql-tutorial.com/sql-like-in-operators.htm).

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Write a query that returns the `title`, `description`,  `release_year`
for all movies and series that have "monty python" in the title or description.

:::::::::::::::  solution

## Solution

```sql
SELECT title, description, release_year
FROM filmsAndSeries
WHERE title LIKE "%Monty Python%" OR description like "%Monty Python%" ;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

You can continue to add or chain conditions together and write more advanced queries.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `WHERE` to filter and retrieve data based on specific conditions.
- Use `AND, OR, and NOT` to add additional conditions.
- Use the comparison keyword `LIKE` and wildcard characters such as `%` to match patterns.

::::::::::::::::::::::::::::::::::::::::::::::::::


