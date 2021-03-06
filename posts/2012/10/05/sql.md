---
date: 2012-10-05
title: The fast-track, hands-on, no-nonsense introduction to SQL
author: Dan Șerban
tags: SQL, RDBMS
---

Rather than relying on dry explanations of mathematical set theory, this
tutorial is organized as a survey of SQL statements and techniques. It is
designed to let you infer from examples what SQL is all about as well as the
kinds of problems it can help you solve.

<!--more-->

The promise of this tutorial is that you invest 30 minutes of your time and it
will enable you to "speak" SQL right here, right now.

The uncompromising emphasis here is on raw speed of learning new things.

### Introduction

SQL is the universal language that allows relational database management
systems (RDBMS) to communicate with the outside world.

The easiest way for a developer to become familiar with SQL is by using
SQLite, a file-based (serverless) RDBMS.

### Let's start coding already!

To check whether you already have SQLite on your GNU/Linux system, open up a
terminal and run:

    which sqlite3 && echo "OK - SQLite found"

To install SQLite on Ubuntu, simply run:

    sudo apt-get install sqlite3

The first few baby steps when learning SQL are:

- creating a new database;
- creating a new table inside that database;
- populating the table with some data.

First, let's create a new database. In a terminal, run:

    sqlite3 /tmp/dev.db

You should see output similar to this:

    SQLite version 3.7.13 2012-06-11 02:05:22
    Enter ".help" for instructions
    Enter SQL statements terminated with a ";"
    sqlite>

At this point, SQLite has created and appropriately formatted a new file,
`/tmp/dev.db`, and is now waiting for you to tell it what to do.

You communicate your intent to SQLite by typing in SQL statements.

As we already mentioned, when we start working on a new database, the first
thing we do is create a table.

At the `sqlite>` prompt, type the following SQL statement:

~~~ sql
CREATE TABLE presidencies (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  first_name VARCHAR(255),
  other_names VARCHAR(255),
  year_from INTEGER,
  year_to INTEGER,
  notes TEXT
);
~~~

SQLite does not give any output in response, and that means it has
successfully executed the statement.

We now have an empty table called `presidencies`.

Having an empty table in a database is like having an empty spreadsheet in
front of you.

The next step is therefore to populate the table with some data.

We do this by using the `INSERT` statement. Try this:

~~~ sql
INSERT INTO presidencies VALUES (
  NULL,
  "Barack",
  "Obama",
  2009,
  2012,
  "Won against John McCain"
);
~~~

Again, SQLite is not giving us any response, which means the `INSERT`
statement was successfully executed.

If we now query the contents of the table, we should see the newly inserted
row. Try this:

~~~ sql
SELECT * FROM presidencies;
~~~

The output you should see is this:

    1|Barack|Obama|2009|2012|Won against John McCain

As you can probably tell, the output is in the format:

    id|first_name|other_names|year_from|year_to|notes

Since we set `id` to `NULL` in our `INSERT` statement and `id` has the
`AUTOINCREMENT` attribute in the table definition, SQLite assigned the first
available positive integer value and stored it as this record's primary key.

"Primary key" simply means two things:

- The developer has communicated his intention to use this field to uniquely
  identify each row of this table;
- The RDBMS will therefore enforce the uniqueness of this key and use this
  information to optimize its execution of SQL statements against this table.

Let's put this behavior to the test. Try the same `INSERT` statement as
before, only replace `NULL` with `1` (an already existing `id`):

~~~ sql
INSERT INTO presidencies VALUES (
  1,
  "Barack",
  "Obama",
  2009,
  2012,
  "Won against John McCain"
);
~~~

The output you should see is:

    Error: PRIMARY KEY must be unique

To change a record in the table, we are going to use an `UPDATE` statement
with a precisely targeted `WHERE` clause:

~~~ sql
UPDATE presidencies SET notes = "His campaign slogan was yes-we-can" WHERE id = 1;
~~~

Query the whole table again to see the changes:

``` sql
SELECT * FROM presidencies;
```

The output you should see is this:

    1|Barack|Obama|2009|2012|His campaign slogan was yes-we-can

To remove a record from the table, we are going to use a precisely targeted
`DELETE` statement:

``` sql
DELETE FROM presidencies WHERE id = 1;
```

Query the whole table again to verify that it's empty.

To continue exploring the features of SQL we are going to need a lot more data.

Paste the statements below at the `sqlite>` prompt:

``` sql
INSERT INTO presidencies VALUES (NULL, "Theodore", "Roosevelt", 1905, 1908, "");
INSERT INTO presidencies VALUES (NULL, "William", "Taft", 1909, 1912, "");
INSERT INTO presidencies VALUES (NULL, "Woodrow", "Wilson", 1913, 1916, "1st term; WW 1 begins");
INSERT INTO presidencies VALUES (NULL, "Woodrow", "Wilson", 1917, 1920, "2nd term; WW 1 ends");
INSERT INTO presidencies VALUES (NULL, "Warren", "Harding", 1921, 1922, "");
INSERT INTO presidencies VALUES (NULL, "Calvin", "Coolidge", 1923, 1924, "1st term");
INSERT INTO presidencies VALUES (NULL, "Calvin", "Coolidge", 1925, 1928, "2nd term");
INSERT INTO presidencies VALUES (NULL, "Herbert", "Hoover", 1929, 1932, "");
INSERT INTO presidencies VALUES (NULL, "Franklin", "D. Roosevelt", 1933, 1936, "1st term");
INSERT INTO presidencies VALUES (NULL, "Franklin", "D. Roosevelt", 1937, 1940, "2nd term; WW 2 begins");
INSERT INTO presidencies VALUES (NULL, "Franklin", "D. Roosevelt", 1941, 1944, "3rd term");
INSERT INTO presidencies VALUES (NULL, "Harry", "Truman", 1945, 1948, "1st term; WW 2 ends");
INSERT INTO presidencies VALUES (NULL, "Harry", "Truman", 1949, 1952, "2nd term");
INSERT INTO presidencies VALUES (NULL, "Dwight", "Eisenhower", 1953, 1956, "1st term");
INSERT INTO presidencies VALUES (NULL, "Dwight", "Eisenhower", 1957, 1960, "2nd term");
INSERT INTO presidencies VALUES (NULL, "John", "F. Kennedy", 1961, 1963, "");
INSERT INTO presidencies VALUES (NULL, "Lyndon", "Johnson", 1964, 1964, "Took over when JFK was assassinated");
INSERT INTO presidencies VALUES (NULL, "Lyndon", "Johnson", 1965, 1968, "2nd term");
INSERT INTO presidencies VALUES (NULL, "Richard", "Nixon", 1969, 1972, "1st term");
INSERT INTO presidencies VALUES (NULL, "Richard", "Nixon", 1973, 1974, "2nd term");
INSERT INTO presidencies VALUES (NULL, "Gerald", "Ford", 1975, 1976, "");
INSERT INTO presidencies VALUES (NULL, "Jimmy", "Carter", 1977, 1980, "");
INSERT INTO presidencies VALUES (NULL, "Ronald", "Reagan", 1981, 1984, "1st term");
INSERT INTO presidencies VALUES (NULL, "Ronald", "Reagan", 1985, 1988, "2nd term");
INSERT INTO presidencies VALUES (NULL, "George", "H. W. Bush", 1989, 1992, "");
INSERT INTO presidencies VALUES (NULL, "Bill", "Clinton", 1993, 1996, "1st term");
INSERT INTO presidencies VALUES (NULL, "Bill", "Clinton", 1997, 2000, "2nd term");
INSERT INTO presidencies VALUES (NULL, "George", "W. Bush", 2001, 2004, "1st term");
INSERT INTO presidencies VALUES (NULL, "George", "W. Bush", 2005, 2008, "2nd term");
INSERT INTO presidencies VALUES (NULL, "Barack", "Obama", 2009, 2012, "");
```

Let's see how many records our table contains now:

``` sql
SELECT COUNT(*) FROM presidencies;
```

The table has 30 rows.

We want to generate a deduplicated list of all persons who held office.

The SQL keyword `DISTINCT` performs the deduplication magic, and we are going
to make use of the SQL text concatenation operator `||`:

``` sql
SELECT DISTINCT first_name || ' ' || other_names AS full_name FROM presidencies;
```

We want to find out how many presidencies began in the second half of the 20th century.

``` sql
SELECT COUNT(*) FROM presidencies WHERE year_from BETWEEN 1950 AND 1999;
```

The answer to that question is 14 records.

In 1941, the surprise attack at Pearl Harbor happened.

We would like to know who was president of the United States at that time.

``` sql
SELECT * FROM presidencies WHERE 1941 BETWEEN year_from AND year_to;
```

The expected output:

    12|Franklin|D. Roosevelt|1941|1944|3rd term

We stored some notes on when both world wars began and ended. Let's query that
information by asking for all records that contain the string " WW " inside
the `notes` column.

To perform text matching we need to use the `LIKE` predicate.

Note: The percent sign has wildcard semantics in SQL.

``` sql
SELECT * FROM presidencies WHERE notes LIKE '% WW %' ORDER BY year_from;
```

The expected output:

    4|Woodrow|Wilson|1913|1916|1st term; WW 1 begins
    5|Woodrow|Wilson|1917|1920|2nd term; WW 1 ends
    11|Franklin|D. Roosevelt|1937|1940|2nd term; WW 2 begins
    13|Harry|Truman|1945|1948|1st term; WW 2 ends

We want a breakdown of how many presidencies were full-term (lasted the full 4
years) versus how many lasted 1 or 2 or 3 years.

That can also be done in SQL, however the syntax is slightly more convoluted.

``` sql
SELECT
  1 + year_to - year_from AS duration, COUNT(*) AS cnt
FROM
  presidencies
GROUP BY
  duration
ORDER BY cnt DESC;
```

The expected output:

    4|24
    2|4
    1|1
    3|1

That means there were 24 presidencies which lasted the full 4 years, 4 which
lasted 2 years etc.

**Important side note**

Being able to get predictable, consistent performance is an important part of
software development, and there are some performance considerations to take
into account when using `WHERE` clauses.

The database engine performs these `WHERE` lookups significantly faster when
it can rely on indexes.

Let's say we know ahead of time that the most frequent queries are going to
filter by `year_from` and `year_to`.

In that case we need to create indexes on both fields:

``` sql
CREATE INDEX index_on_year_from ON presidencies (year_from);
CREATE INDEX index_on_year_to   ON presidencies (year_to);
```

Indexes do not make a big difference on a table with 30 records, but it is
recommended to get into the habit of thinking about indexes before SQL
execution times start to grow out of control.

### Foreign key relationships: the ONE-TO-MANY case

Let us now explore how we can build database models for information entities
which are related to one another.

We are going to use the geographical hierarchy of continents `->` countries
`->` cities to illustrate this.

- A continent has many countries
- A country has many cities

Conversely:

- A city belongs to only one country
- A country belongs to only one continent

To create tables for each of the three information entities, run the following
commands:

``` sql
CREATE TABLE continents (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  continent_name VARCHAR(255)
);
CREATE TABLE countries (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  continent_id INTEGER NOT NULL,
  country_name VARCHAR(255)
);
CREATE TABLE cities (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  country_id INTEGER NOT NULL,
  city_name VARCHAR(255)
);
```

Note: SQL experts will notice something is missing from the table definitions
above. I have deliberately chosen not to cover foreign key constraints in this
beginner-level tutorial in order not to place too much of a cognitive burden
on the reader.

Let's populate these tables with some data I have prepared for you in a file.

Quit SQLite with Ctrl-D, download and inspect the file as shown below:

    wget -O /tmp/geography.sql http://dserban.github.com/introduction-to-sql/geography.sql
    less /tmp/geography.sql

Load the data into our database `dev.db` (this operation may take a while on
slower computers):

    sqlite3 /tmp/dev.db < /tmp/geography.sql

Now open the database again and let's write some queries.

    sqlite3 /tmp/dev.db

The query:

``` sql
SELECT * FROM continents;
```

behaves the way you expect.

Next, go ahead and run the query below, and see what happens:

``` sql
SELECT * FROM continents AS c1, continents AS c2;
```

The above is definitely a valid SQL statement, and I'm just showing it to you
in order for you to get comfortable with the idea that SQL statements may
reference more than one database table, and if you reference the same table
twice, the output will be the cartesian product of the table's row set with
itself.

Next, try this:

``` sql
SELECT
  country_name, continent_name
FROM
  countries, continents
WHERE
  countries.continent_id = continents.id
ORDER BY
  country_name;
```

What we just did is formally called "traversing a foreign key relationship".

We retrieved some records from the `countries` table, found some "pointers" in
the `continent_id` column, went to the `continents` table, retrieved the
continent descriptions pertaining to those pointers, and then brought back
those results and associated them with each row in the `countries` table.

Congratulations, you have just learned about the "R" in "RDBMS", this is what
we mean by "relational".

Notice one thing: the query above will start suffering from performance issues
for tables beyond a certain size.

Before we go on, we want to take care of performance-by-design.

The lookups which happen during foreign key relationship traversal would be
much more efficient when supported by the appropriate indexes.

Let's create indexes on those foreign key columns.

``` sql
CREATE INDEX index_on_country_id   ON cities    (country_id);
CREATE INDEX index_on_continent_id ON countries (continent_id);
```

Moreover, `country_name` and `city_name` are also worth indexing, since in the
future we are very likely to need to locate records based on them.

``` sql
CREATE INDEX index_on_city_name    ON cities    (city_name);
CREATE INDEX index_on_country_name ON countries (country_name);
```

Let us now write a query that performs a three-way join:

``` sql
SELECT
  city_name, country_name, continent_name
FROM
  cities, countries, continents
WHERE
  cities.country_id = countries.id
  AND
  countries.continent_id = continents.id
ORDER BY
  city_name;
```

In order to avoid repeated typing of complex queries, SQL allows us to store a
given `SELECT` statement under a specific name.

The resulting database object is called a view.

Let's create a view for the previous `SELECT` statement:

``` sql
CREATE VIEW augmented_cities AS
SELECT
  city_name, country_name, continent_name
FROM
  cities, countries, continents
WHERE
  cities.country_id = countries.id
  AND
  countries.continent_id = continents.id;
```

Using the newly created view, we can more comfortably find out all of the
information about e.g. the city of London which our database has to offer:

``` sql
SELECT * FROM augmented_cities WHERE city_name = 'London';
```

The output:

    London|United Kingdom|Europe

Next, let's look at an example of a subquery. Try this:

``` sql
SELECT
  continent_name, how_many_countries
FROM
  continents,
  (SELECT continent_id, COUNT(*) AS how_many_countries FROM countries GROUP BY continent_id) AS breakdows
WHERE
  continents.id = breakdows.continent_id;
```

The output:

    North America|4
    South America|9
    Europe|32
    Africa|15
    Asia|17
    Oceania|2

What we did here is we joined a real table (`continents`) with a virtual one
called a subquery (the result of a `SELECT` statement).

We can do this because SQL doesn't really join tables, it joins rectangular
result sets consisting of rows and columns.

Let's store this query in a view, we are going to make use of it in a future
tutorial.

``` sql
CREATE VIEW continent_statistics AS
SELECT
  continent_name, how_many_countries
FROM
  continents,
  (SELECT continent_id, COUNT(*) AS how_many_countries FROM countries GROUP BY continent_id) AS breakdows
WHERE
  continents.id = breakdows.continent_id;
```

### Foreign key relationships: the MANY-TO-MANY case

Up to this point, we have explored SQL statements on a single table, as well
as one-to-many relationships.

How do we model many-to-many relationships?

How do we model people upvoting news stories on social networks?

- One person may upvote more than one story

Conversely:

- One story may get upvoted by several people

How do we model project communities which collaborate on Github?

- One person may contribute to more than one project

Conversely:

- One project may get contributions from several people

The answer is that we need to define a separate information entity which is
going to track those complex relationships for us.

In Github's case, the core information entities are User and Project, and we
are going to name the third one Contributorship.

Therefore:

- A user has many contributorships
- A contributorship belongs to a user
- A project has many contributorships
- A contributorship belongs to a project

Let's go ahead and create tables for those three information entities:

``` sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  user_name VARCHAR(255)
);
CREATE TABLE projects (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  project_name VARCHAR(255)
);
CREATE TABLE contributorships (
  id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
  user_id INTEGER NOT NULL,
  project_id INTEGER NOT NULL
);
```

And let's populate the tables with some sample data (5 users, 9 projects):

``` sql
INSERT INTO users VALUES (1, "alex-morega");
INSERT INTO users VALUES (2, "gvoicu");
INSERT INTO users VALUES (3, "igstan");
INSERT INTO users VALUES (4, "dserban");
INSERT INTO users VALUES (5, "torvalds");
INSERT INTO projects VALUES (1, "torvalds/linux");
INSERT INTO projects VALUES (2, "rosedu/WebDev");
INSERT INTO projects VALUES (3, "rosedu/wouso");
INSERT INTO projects VALUES (4, "rosedu/techblog");
INSERT INTO projects VALUES (5, "rosedu/StartTheDark");
INSERT INTO projects VALUES (6, "gvoicu/miniflow");
INSERT INTO projects VALUES (7, "rails/rails");
INSERT INTO projects VALUES (8, "sinatra/sinatra");
INSERT INTO projects VALUES (9, "mitsuhiko/flask");
INSERT INTO contributorships VALUES ( 1, 1, 2);
INSERT INTO contributorships VALUES ( 2, 1, 3);
INSERT INTO contributorships VALUES ( 3, 1, 9);
INSERT INTO contributorships VALUES ( 4, 2, 2);
INSERT INTO contributorships VALUES ( 5, 2, 6);
INSERT INTO contributorships VALUES ( 6, 2, 7);
INSERT INTO contributorships VALUES ( 7, 2, 8);
INSERT INTO contributorships VALUES ( 8, 3, 2);
INSERT INTO contributorships VALUES ( 9, 3, 6);
INSERT INTO contributorships VALUES (10, 4, 1);
INSERT INTO contributorships VALUES (11, 4, 2);
INSERT INTO contributorships VALUES (12, 4, 3);
INSERT INTO contributorships VALUES (13, 4, 4);
INSERT INTO contributorships VALUES (14, 4, 5);
INSERT INTO contributorships VALUES (15, 4, 6);
INSERT INTO contributorships VALUES (16, 4, 7);
INSERT INTO contributorships VALUES (17, 4, 8);
INSERT INTO contributorships VALUES (18, 5, 1);
```

Now before we start issuing queries, let's take care of the
performance-by-design side of things.

We anticipate that we'll need to do heavy querying by `user_name` and by
`project_name`.

Therefore we need indexes on those columns:

``` sql
CREATE INDEX index_on_user_name    ON users    (user_name);
CREATE INDEX index_on_project_name ON projects (project_name);
```

It goes without saying that we need to index the foreign key columns:

``` sql
CREATE INDEX index_on_user_id    ON contributorships (user_id);
CREATE INDEX index_on_project_id ON contributorships (project_id);
```

We also need to make sure that duplicate contributorships cannot exist.

We achieve this by creating a unique index on the combination of `user_id` and
`project_id`:

``` sql
CREATE UNIQUE INDEX unique_index_on_user_id_and_project_id ON contributorships (user_id,project_id);
```

First, let's list all contributorships in human readable format:

``` sql
SELECT
  user_name || " contributes to " || project_name
FROM
  contributorships,
  users,
  projects
WHERE
  users.id = contributorships.user_id
  AND
  projects.id = contributorships.project_id
ORDER BY
  user_name, project_name;
```

The output you should get is:

    alex-morega contributes to mitsuhiko/flask
    alex-morega contributes to rosedu/WebDev
    alex-morega contributes to rosedu/wouso
    dserban contributes to gvoicu/miniflow
    dserban contributes to rails/rails
    dserban contributes to rosedu/StartTheDark
    dserban contributes to rosedu/WebDev
    dserban contributes to rosedu/techblog
    dserban contributes to rosedu/wouso
    dserban contributes to sinatra/sinatra
    dserban contributes to torvalds/linux
    gvoicu contributes to gvoicu/miniflow
    gvoicu contributes to rails/rails
    gvoicu contributes to rosedu/WebDev
    gvoicu contributes to sinatra/sinatra
    igstan contributes to gvoicu/miniflow
    igstan contributes to rosedu/WebDev
    torvalds contributes to torvalds/linux

Now let's determine all projects for a given user, as well as the list of all people who contribute to a given project.

``` sql
SELECT
  project_name
FROM
  contributorships,
  users,
  projects
WHERE
  users.user_name = 'gvoicu'
  AND
  users.id = contributorships.user_id
  AND
  projects.id = contributorships.project_id
ORDER BY
  project_name;
```

The output you should get is:

    gvoicu/miniflow
    rails/rails
    rosedu/WebDev
    sinatra/sinatra

To list a given project's contributors:

``` sql
SELECT
  user_name
FROM
  contributorships,
  users,
  projects
WHERE
  projects.project_name = 'rosedu/WebDev'
  AND
  users.id = contributorships.user_id
  AND
  projects.id = contributorships.project_id
ORDER BY
  user_name;
```

The output you should get is:

    alex-morega
    dserban
    gvoicu
    igstan

Finally, let's create a view which will make it easier for us to go back and
forth between users and projects:

``` sql
CREATE VIEW augmented_contributorships AS
SELECT
  user_name, project_name
FROM
  contributorships,
  users,
  projects
WHERE
  users.id = contributorships.user_id
  AND
  projects.id = contributorships.project_id;
```

### Closing words

This short overview of SQL ends here.

If you want to learn more SQL tips and tricks, I highly recommend [Learn SQL
The Hard Way](http://sql.learncodethehardway.org/book/)

