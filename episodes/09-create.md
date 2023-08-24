---
title: Creating tables and modifying data
teaching: 15
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Write statements that create tables.
- Write statements to insert, modify, and delete records.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I create, modify, and delete tables and data?

::::::::::::::::::::::::::::::::::::::::::::::::::

So far we have only looked at how to get information out of a database,
both because that is more frequent than adding information,
and because most other operations only make sense
once queries are understood.
If we want to create and modify data,
we need to know two other sets of commands.

The first pair are [`CREATE TABLE`][create-table] and [`DROP TABLE`][drop-table].
While they are written as two words,
they are actually single commands.
The first one creates a new table;
its arguments are the names and types of the table's columns.
For example,
the following statement creates the table `filmsAndSeries`:

```sql

CREATE TABLE "filmsAndSeries" (	"id" TEXT, "title" TEXT, "type" TEXT, "description" TEXT, "release_year" INTEGER, "age_certification" TEXT, "runtime" INTEGER, "seasons" TEXT, "imdb_score" REAL, "imdb_votes" INTEGER, "tmdb_popularity" REAL, "tmdb_score" REAL)
```

We can get rid of one of our tables using:

```sql
DROP TABLE filmsAndSeries;
```

Be very careful when doing this:
if you drop the wrong table, hope that the person maintaining the database has a backup,
but it's better not to have to rely on it.

We talked about data types earlier [in Introduction to SQL: SQL Data Type Quick Reference](01-introduction.md#sql-data-type-quick-reference).

When we create a table,
we can specify several kinds of constraints on its columns.
For example,
a better definition for the `filmsAndSeries` table would be:

```sql
CREATE TABLE "filmsAndSeries" (
	"id"	TEXT NOT NULL PRIMARY KEY,
	"title"	TEXT,
	"type"	TEXT,
	"description"	TEXT,
	"release_year"	INTEGER,
	"age_certification"	TEXT,
	"runtime"	INTEGER,
	"seasons"	TEXT,
	"imdb_score"	REAL,
	"imdb_votes"	INTEGER,
	"tmdb_popularity"	REAL,
	"tmdb_score"	REAL,
)
```

Once again,
exactly what constraints are available
and what they're called
depends on which database manager we are using.

Once tables have been created,
we can add, change, and remove records using our other set of commands,
`INSERT`, `UPDATE`, and `DELETE`.

Here is an example of inserting rows into the `filmsAndSeries` table:

```sql

INSERT INTO "filmsAndSeries" VALUES ("tm84618",  "Taxi Driver",  "MVIE",	"A mentally unstable Vietnam War...",	        1976,	"R",	    114,,	    8.2,	808582,     40.965,	8.179)
INSERT INTO "filmsAndSeries" VALUES ("tm154986",  "Deliverance",  "MOVIE",	"Intent on seeing the Cahulawassee...",	      1972,	"R"	      109,,	    7.7,	107673,     10.01,	7.3)
INSERT INTO "filmsAndSeries" VALUES ("ts22164",  "Monty Python's Flying Circus",  "SHOW",	"A British sketch comedy...",	  1969,	"TV-14",	30,	4.0,	8.8,	73424,      17.617, 8.306)
INSERT INTO "filmsAndSeries" VALUES ("tm120801",  "The Dirty Dozen",	"MOVIE",	"12 American military prisoners in ...",	      1967, , 	      150,,		  7.7,	72662,	    20.398,	7.6)



```

We can also insert values into one table directly from another:

```sql
CREATE TABLE "myMovies"(title TEXT, description TEXT, year INTEGER );
INSERT INTO "myMovies" SELECT title, decription, release_year FROM filmsAndSeries;
```

Modifying existing records is done using the `UPDATE` statement.
To do this we tell the database which table we want to update,
what we want to change the values to for any or all of the fields,
and under what conditions we should update the values.

For example, we made a typo when entering the type
of the first `INSERT` statement above, we can correct it with an update:

```sql
UPDATE filmsAndSeries SET type = "MOVIE", WHERE id = "tm84618";
```

Be careful to not forget the `WHERE` clause or the update statement will
modify *all* of the records in the database.

Deleting records can be a bit trickier,
because we have to ensure that the database remains internally consistent.
If all we care about is a single table,
we can use the `DELETE` command with a `WHERE` clause
that matches the records we want to discard.
We can remove the movie `Deliverance` from the `filmsAndSeries` table like this:

```sql
DELETE FROM filmsAndSeries WHERE title = 'Deliverance';
```


:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

Write an SQL statement to add the country "Gibraltar" (code: GI) to the table
`countries`. 

:::::::::::::::  solution

## Solution

```sql

INSERT INTO "countries" VALUES ("GI", "Gibraltar");
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Backing Up with SQL

SQLite has several administrative commands that aren't part of the
SQL standard.  One of them is `.dump`, which prints the SQL commands
needed to re-create the database.  Another is `.read`, which reads a
file created by `.dump` and restores the database.  
:::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Use CREATE and DROP to create and delete tables.
- Use INSERT to add data.
- Use UPDATE to modify existing data.
- Use DELETE to remove data.
- It is simpler and safer to modify data when every record has a unique primary key.
- Do not create dangling references by deleting records that other records refer to.

::::::::::::::::::::::::::::::::::::::::::::::::::

[create-table]: https://www.sqlite.org/lang_createtable.html
[drop-table]: https://www.sqlite.org/lang_droptable.html



