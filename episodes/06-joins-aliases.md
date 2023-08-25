---
title: Joins and aliases
teaching: 25
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand how to link tables together via joins.
- Understand when it is valuable to use aliases or shorthand.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I join two tables if they share a common point of information?
- How can I use aliases to improve my queries?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Joins

The SQL `JOIN` clause allows us to combine columns from one or more tables in a database by using values common to each. It follows the `FROM` clause in a SQL statement. We also need to tell the computer which columns provide the link between the two
tables using the word `ON`.

Let's start by joining data from the `filmAndSeries` table with the `genres` table. The `id` columns in both these tables links them.

```sql
SELECT *
FROM filmsAndSeries
JOIN genres
ON filmsAndSeries.id  = genres.id;
```

`ON` is similar to `WHERE`, it filters things out according to a test condition.  We use the `table.colname` format to tell the SQL manager what column in which table we are referring to.

Alternatively, we can use the word `USING`, as a short-hand.  In this case we are telling DB Browser that we want to combine `filmsAndSeries` with `genres` and that the common column is `id`.

```sql
SELECT *
FROM filmsAndSeries
JOIN genres
USING (id);
```

When joining tables, you can specify the columns you want by using `table.colname` instead of selecting all the columns using `*`. For example:

```sql
SELECT filmsAndSeries.title, genres.genre, filmsAndSeries.release_year
FROM filmsAndSeries
JOIN genres
ON filmsAndSeries.id = genres.id;
```

Joins can be combined with sorting, filtering, and aggregation.  So, if we wanted the average scores for movies in each genre, we can use the following query:

```sql
SELECT genres.genre,  ROUND(AVG(moviesAndSeries.imdb_score), 2), ROUND(AVG(moviesAndSeries.imdb_score), 2)
FROM genres
JOIN filmsAndSeries
USING (id)
WHERE type = "MOVIE"
GROUP BY genres.genre;
```

The `ROUND` function allows us to round the score number returned by the `AVG` function by 2 decimal places.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Write a query that `JOINS` the `genres` and `filmAndSeries` tables and that returns the `genre`, total number of series and average number seasons for every genre.

:::::::::::::::  solution

## Solution

```sql
SELECT genres.genre, count(*), avg(filmsAndSeries.seasons)
FROM filmsAndSeries
JOIN genres
USING (id)
GROUP BY genres.genre;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

You can also join multiple tables. For example:

```sql
SELECT filmsAndSeries.title, productionCountries.country, genres.genre
FROM filmsAndSeries
JOIN genres
ON filmsAndSeries.id = genres.id
JOIN productionCountries
ON productionCountries.id = genres.id;
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge:

Write a query that returns the `title`, number of
production countries, number of genres, ordered by number of production countries in descending order.

:::::::::::::::  solution

## Solution

```sql
SELECT filmsAndSeries.title, COUNT(DISTINCT productionCountries.Country), COUNT(DISTINCT genres.genre)
FROM filmsAndSeries
JOIN productionCountries
USING (id)
JOIN genres
USING (id)
GROUP BY title
ORDER BY COUNT(DISTINCT Country) DESC;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

There are different types of joins which you can learn more about at [SQL Joins Explained](https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/).

## Aliases

As queries get more complex, names can get long and unwieldy. To help make things clearer we can use aliases to assign new names to items in the query.

We can alias both table names:

```sql
SELECT fas.title, g.genre
FROM filmsAndSeries AS fas
JOIN genres  AS g
ON fas.id = g.id;
```

And column names:

```sql
SELECT fas.title AS title, pc.Country AS country
FROM filmsAndSeries AS fas
JOIN productionCountries  AS pc
ON fas.id = pc.id;
```

The `AS` isn't technically required, so you could do:

```sql
SELECT fas.Title t
FROM filmsAndSeries fas;
```

But using `AS` is much clearer so it is good style to include it.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Joining two tables in SQL is an good way to analyse datasets, especially when both datasets provide partial answers to questions you want to ask.
- Creating aliases allows us to spend less time typing, and more time querying!

::::::::::::::::::::::::::::::::::::::::::::::::::


