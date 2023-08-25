---
title: Extra challenges (optional)
teaching: 0
exercises: 35
---

::::::::::::::::::::::::::::::::::::::: objectives

- Extra challenges to practice creating SQL queries.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Are there extra challenges to practice translating plain English queries to SQL queries?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Extra challenges (optional)

SQL queries help us *ask* specific *questions* which we want to answer about our data. The real skill with SQL is to know how to translate our questions into a sensible SQL queries (and subsequently visualise and interpret our results).

Have a look at the following questions; these questions are written in plain English. Can you translate them to *SQL queries* and give a suitable answer?

Also, if you would like to learn more SQL concepts and try additional challenges, see the [Software Carpentry Databases and SQL](https://swcarpentry.github.io/sql-novice-survey/) lesson.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge 1

How many series  are there for each `genre`? 
:::::::::::::::  solution

## Solution 1

```sql
SELECT genre, COUNT( * )
FROM genres
JOIN filmsAndSeries
USING (id)
WHERE type = "SHOW"
GROUP BY genre;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge 2

How many shows ran for only one season? How many ran for 2 seasons? How many 3? etc?

:::::::::::::::  solution

## Solution 2

```sql
SELECT seasons, COUNT( * )
FROM filmsAndSeries
WHERE type = "SHOW"
GROUP BY seasons

```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge 3

How many movies or shows are there from each `country`? Can you make an alias for the number of movies? Can you order the results by number of movies?

:::::::::::::::  solution

## Solution 3

```sql
SELECT countries.country, COUNT( * ) AS n_movies
FROM productionCountries
JOIN countries
ON countries.code = productionCountries.country
GROUP BY productionCountries.country
ORDER BY n_movies DESC;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge 4

How many titles are there for each `age_certification` type, and what is the average
imdb_score for that `age_certification` type?

:::::::::::::::  solution

## Solution 4

```sql
SELECT age_certification, AVG( imdb_score ), COUNT( * )
FROM filmsAndSeries
GROUP BY Licence;
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- It takes time and practice to learn how to translate plain English queries into SQL queries.

::::::::::::::::::::::::::::::::::::::::::::::::::


