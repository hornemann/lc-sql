---
title: Aggregating & calculating values
teaching: 15
exercises: 5
---

::::::::::::::::::::::::::::::::::::::: objectives

- Use SQL functions like `AVG` in combination with clauses like `Group By` to aggregate values and return results for reports.
- Make calculations on fields using SQL.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can we aggregate values in SQL for reports?
- Can SQL be used to make calculations?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Aggregation

SQL contains functions which allow you to make calculations on data in your database for reports. Some of the most common functions are `MAX, MIN, AVG, COUNT, SUM`, and they will: `MAX` (find the maximum value in a field), `MIN` (find the minimum value in a field), `AVG` (find the average value of a field), `COUNT` (count the number of values in a field and present the total), and `SUM` (add up the values in a field and present the sum).

Let's say we wanted to get the average `tmdb_score` and `imdb_score` for each  `release_year`. We can use `AVG` and the `GROUP BY` clause in a query:

```sql
SELECT release_year, AVG(imdb_score), AVG(tmdb_score)
FROM filmsAndSeries
GROUP BY release_year;
```

`GROUP BY` is used by SQL to arrange identical data into groups. In this case, we are arranging all the scores by years. `AVG` acts on the `imdb_score` or `tmdb_score` in parentheses. This process is also called **aggregation** which allows us to combine results by grouping records based on value and calculating combined values in groups.

As you can see, it is difficult to tell though which year has the highest average score  and the least. We can improve upon the query above by using `ORDER BY` and `DESC`.

```sql
SELECT release_year, AVG(imdb_score), AVG(tmdb_score)
FROM filmsAndSeries
GROUP BY release_year 
ORDER BY AVG(imdb_score) DESC;
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Write a query using an aggregate function that returns the number of movies per year, sorted by title count in descending order. Which year has the most titles?  (Hint to choosing which aggregate function to use - it is one of the common aggregate functions `MAX, MIN, AVG, COUNT, SUM`.)

:::::::::::::::  solution

## Solution

```sql
SELECT release_year, COUNT(title)
FROM filmsAndSeries
WHERE Type = 'MOVIE'
GROUP BY release_year
ORDER BY COUNT(Title) DESC;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## The `HAVING` keyword

SQL offers a mechanism to filter the results based on aggregate functions, through the `HAVING` keyword.

For example, we can adapt the last request we wrote to only return information about  `release_year` with 100 or more movies:

```sql
SELECT release_year, COUNT(*)
FROM filmsAndSeries
WHERE Type = 'MOVIE'
GROUP BY release_year
HAVING COUNT(Title) >= 100
ORDER BY COUNT(Title) DESC;
```

The `HAVING` keyword works exactly like the `WHERE` keyword, but uses aggregate functions instead of database fields.  When you want to filter based on an aggregation like `MAX, MIN, AVG, COUNT, SUM`, use `HAVING`; to filter based on the individual values in a database field, use `WHERE`.

Note that `HAVING` comes *after* `GROUP BY`. One way to think about this is: the data are retrieved (`SELECT`), can be filtered (`WHERE`), then joined in groups (`GROUP BY`); finally, we only select some of these groups (`HAVING`).

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Write a query that returns year and average `imdb_score` from the `filmsAndSeries` table, the average `imdb_score` for each release year
but only for the years with an average score større end eller lig med 5.

:::::::::::::::  solution

## Solution

```sql
SELECT release_year, AVG(imdb_score)
FROM filsAndSeries
GROUP BY release_year
HAVING AVG(imdb_score)>=5;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Calculations

In SQL, we can also perform calculations as we query the database. Also known as computed columns, we can use expressions on a column or multiple columns to get new values during our query. For example, what if we wanted to calculate a new column called `score_difference`:

```sql
SELECT title, release_year, imdb_score - tmdb_score AS score_difference
FROM filmsAndSeries
ORDER BY imdb_score - tmdb_score DESC;
```

In section [6\. Joins and aliases](06-joins-aliases.md) we are going to learn more about the SQL keyword `AS` and how to make use of aliases - in this example we simply used the calculation and `AS` to represent that the new column is different from the original SQL table data.

We can use any arithmetic operators (like `+`, `-`, `*`, `/`, square root `SQRT` or the modulo operator `%`) if we would like.



:::::::::::::::::::::::::::::::::::::::: keypoints

- SQL can be used for reporting purposes.
- Queries can do arithmetic operations on field values.

::::::::::::::::::::::::::::::::::::::::::::::::::


