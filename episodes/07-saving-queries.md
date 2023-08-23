---
title: Saving queries
teaching: 20
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn how to save repeated queries as 'Views' and how to drop them.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I save a query for future use?
- How can I remove a saved query?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Saving queries for future use

It is not uncommon to repeat the same operation more than once, for example
for monitoring or reporting purposes. SQL comes with a very powerful mechanism
to do this: views. Views are queries saved in the database. You query it as a
(virtual) table that is populated every time you query it.

Creating a view from a query requires you to add `CREATE VIEW viewname AS`
before the query itself. For example, if we want to save the query giving
the number of productions from each country in a view, we can write:

```sql
CREATE VIEW production_counts AS
SELECT countries.country, COUNT(*)
FROM productionCountries
JOIN countries
ON countries.code = productionCountries.Country
GROUP BY countries.Country;
```

Now, we will be able to access these results with a much shorter notation:

```sql
SELECT *
FROM production_count;
```

Assuming we do not need this view anymore, we can remove it from the database.

```sql
DROP VIEW production_counts;
```

In DBBrowser for SQLite, you can also create a view from any query by omitting
the `CREATE VIEW viewname AS` statement and instead, clicking the small Save
icon at the top of the Execute SQL tab and then clicking **Save as view**.
Whatever method you use to create a view, it will appear in the list of views
under the Database Structure tab.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Write a `CREATE VIEW` query that `JOINS` the `filmsAndSeries` table with the
`productionCountries` table on `id` and returns the `AVG` of imdb_score
grouped by the `Country` in `DESC` order.

:::::::::::::::  solution

## Solution

```sql
CREATE VIEW scores_by_country AS
SELECT countries.country, AVG(filmsAndSeries.imdb_score) as score
FROM productionCountries
JOIN filmsAndSeries
USING (id)
JOIN countries
ON countries.code = productionCountries.Country
GROUP BY countries.country
ORDER BY score DESC
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Saving queries as 'Views' allows you to save time and avoid repeating the same operation more than once.

::::::::::::::::::::::::::::::::::::::::::::::::::


