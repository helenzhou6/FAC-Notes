# Week Six

_Resource:_ [_week five schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-6) - [our condensed version](hhttps://github.com/foundersandcoders/london-programme/blob/master/current-cohort/week6.md) due to Bank Holiday, _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-6/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-6/resources.md).

It's Database themed week!

## Day One

### Introduction
Database is a systematic collection of data.
* Data can have different relationships to each other including one to many, many to one and many to many.
There are different **Database Management Systems (DBMS)** including:
* > [**Relational DBMS**](https://www.guru99.com/introduction-to-database-sql.html) - this type of DBMS defines database relationships in form of tables, also known as relations. It does not support many to many relationships. Relational DBMS usually have pre-defined data types that they can support.
    * It saves data in table formats. Each row of data has an `id` (that is a **PRIMARY KEY** and unique to each entry in that table), and **FOREIGN KEY**s in a table (can be unique or not depending on one to many or many to one relationship) that acts to reference the `id` of another table.
* _PostgreSQL_ is an example of a RDBMS (that we will be using).
* _Structured Query language (SQL)_ is the standard language for dealing with Relational Databases. There are a few different syntaxes when using SQL for databases.

### PostgreSQL Setup
_Resources:_ [_FAC setup instructions_](https://github.com/macintoshhelper/learn-sql/blob/master/postgresql/setup.md)

* `CLI` MAC instructions:
```bash
# install postgresql
brew install postgresql

# uncomment and run below if you experience problems:
#initdb /usr/local/var/postgres

# start the postgres server
brew services start postgresql

# Set logged in username to environment variable (leave who am i as it is)
PSQL_MY_USERNAME=$(whoami)

# create database with your username (from environment variable)
createdb $PSQL_MY_USERNAME
```

Then to test that you can login to the database, use `psql`.
Running `psql` is equivalent to `psql <your computer username>`

* As a last resort can use [the Postgres Mac GUI app](https://postgresapp.com/)
* *Pgcli* is an alternative command line interface (client) for Postgres with auto-completion and syntax highlighting.
    * Login to databases with `pgcli` instead of `psql`.
* Be aware of [user permissions](https://github.com/macintoshhelper/learn-sql/blob/master/postgresql/setup.md#user-permissions)

#### Creating databases
To create a database, run:
```sh
createdb <database name>
```

And to connect to it, run:
```sh
psql <database name>
```
* OR look at example below:

### SQL Commands Intro
_Resources:_ [_FAC workshop_](https://github.com/foundersandcoders/sql-commands-intro/) _and_ [_ws3 schools SQL_](https://www.w3schools.com/sql)

* Task: Using basic SQL in a CLI to work with an existing blog database. 
* So in the directory on your terminal, using `psql` or `pgcli` then:
```sql
CREATE DATABASE blog_workshop;
```

`\c blog_workshop`  - connect to the database

`\i init.sql` - run SQL build file in blog_workshop (You can also do `psql --file init.sql` in bash)

* **Schema diagrams** show the different tables and relationships to them e.g. for _books_ table:
--- | --- | ---
id | integer | not null default
name | character varying(100) | not null
release_date | date | not null
publisher_id | integer | foreign key (publishers.id)

#### Retrieving data
* Using [`SELECT`](https://www.w3schools.com/sql/sql_select.asp), to retrieve all the information from the a table - `SELECT [* for everything or column name, column 2 name] FROM [table name]`
* Using `SELECT`, retrieve a list of *only* _column 1 name_ and _column 2 name_ from the _table name_ - `SELECT [column 1 name, column 2 name] FROM [table name]`
* Using [`WHERE`](https://www.w3schools.com/sql/sql_where.asp), to retrieve data based on a conditional statement - `SELECT…FROM… WHERE [conditional statement e.g. age < 20]`. Text needs single quotations and equals is `=` not two symbols.
    * For two conditions can use [`AND`](https://www.w3schools.com/sql/sql_and_or.asp)`SELECT…FROM… WHERE... AND [second conditional statement` - also `WHERE...OR...` and `WHERE NOT`.
* Using [`LIKE`](https://www.w3schools.com/sql/sql_like.asp), to retrieve data where a specific string of text contains a certain word - `SELECT…FROM… WHERE … LIKE [e.g. '%word%']`
    * Using `%` is the **wild card** like regex that allows for characters either side.
    * For different cases can use `LOWER()` (e.g. `WHERE LOWER(surname) LIKE LOWER('%Wistle%');`) or just use `ILIKE` instead 
* Using [`IN`](https://www.w3schools.com/sql/sql_in.asp) to combine two conditional statements that use `=` for the same column: `SELECT …FROM… WHERE…[column name, column 2 name] IN ([specific value of column 1,  specific value of column 2])` - e.g. `SELECT user_id, text_content FROM blog_posts WHERE user_id IN (3, 6);`
* Using [`CASE WHEN`](https://www.postgresql.org/docs/7.4/static/functions-conditional.html) (to generate new temporary data from the existing data based on logic) and [`AS`](https://www.w3schools.com/sql/sql_alias.asp) (to display it in a temporary new column). E.g. show a list of _users_ by their `id` and a new column named _newname_ with values `true` or `false` based on the `CASE WHEN` statement (_age_).
    * `SELECT [column name(s) also to be printed], [remember comma!], CASE WHEN [condition e.g. age < 20] THEN [set value e.g. true] ELSE [other value e.g. false] END [to end the case] AS [temporary column name to put the case true/false in] FROM [table name]` e.g. 
    ```bash
    SELECT id,
       CASE WHEN age<20 THEN 'true'
            ELSE 'false'
       END AS teenager
    FROM users;
    ```
#### Adding data
* Using [`INSERT INTO`](https://www.w3schools.com/sql/sql_insert.asp) to add data - `INSERT INTO [table name] ([column 1 name, column 2 name]) VALUES ([new column 1 value, new column 2 value)]`
* Using [`UPDATE`](https://www.w3schools.com/sql/sql_update.asp), update data = `UPDATE [table name] SET [what to change e.g. user_id=5] WHERE [condition e.g. text_content=‘Hello world’]`

#### General
* Can use **subqueries** (nesting queries inside) e.g.:
    ```bash
    INSERT INTO post_comments (post_id, reply_to, user_id, text_content) VALUES
        (
            (
            SELECT id FROM blog_posts WHERE text_content LIKE '%Peculiar%'
            ),
            NULL,
            3,
            'Interesting post'
        )
    ;
    ```
* `!=` or can use `<>`
* Can use `WHERE [column name] IS NOT NULL` or `[column name] IS NULL`

### PostgreSQL Workshop
_Resources:_ [_PostgreSQL Workshop_](https://github.com/foundersandcoders/postgres-workshop) 

#### Set up
* Workshop using a **server-client model** where we're running both on the same machine but if we wanted to we could have the server running on a different computer and connect to it via the client.
* > We will be working with the dataset in the `data.sql` file. This file contains a set of SQL commands that will create a set of tables and fill them with data. We will connect to the PostgreSQL server running locally on our individual computers and tell it to run the file.
* If you run into problems, on a Mac using Homebrew, run `brew services restart postgresql` and try to connect again.
* On terminal within directory:
    * `psql --file=data.sql` (or if doesnt work try `psql -f data.sql`). This will connect to your PostgreSQL server and run all of the SQL in `data.sql`, setting up our database for us.
    * We can connect to our newly-created database by running `psql` (if it gives an 'access denied' error, try `psql -U [your-user-name]`).

#### Challenges
* Can use [`ORDER BY surname ASC|DESC`](https://www.w3schools.com/sql/sql_orderby.asp)
* Using [`LIMIT`](https://www.techonthenet.com/sql/select_limit.php) to limit number of row displayed
* For many to many relationships (where two tables can't be merged but temporarily joined) - use [`JOIN`](https://www.w3schools.com/sql/sql_join.asp) and specify using `ON` which `id` primary key from the first table is referred to as a foreign key in the second table to link the tables (e.g. `SELECT publishers.name FROM publishers INNER JOIN books ON books.publisher_id = publishers.id`). 
    * Differences between the joins - where `INNER JOIN` only lists the data that is populated on both tables (matches on both tables), whilst `LEFT JOIN` or `RIGHT JOIN` will contain entries that are never referenced in the second table (that column is left blank)
    ![join image](https://i.imgur.com/LUejO6el.jpg)
    * After joining tables, use  object-like syntax to reference column (e.g. `authors` and `books` tables have been joined - so need to use `books.name` and `authors.first_name`)
    * Can join on multiple tables to each other, and have the `WHERE` etc at the end - `SELECT...FROM...JOIN...WHERE...`
* After joining tables, there could be duplicate rows so use[`GROUP BY`](https://www.w3schools.com/sql/sql_groupby.asp) to combine duplicate rows together (rather than have them displayed multiple times for each row)
    * After this, the `COUNT()` refers to the number of rows within each grouping.
    * So can use a combination of the [`HAVING`](https://www.w3schools.com/sql/sql_having.asp) a condition and maths operators including [`COUNT()`, `AVG()` and `SUM()`](https://www.w3schools.com/sql/sql_count_avg_sum.asp).
    * E.g. `SELECT...FROM...JOIN...GROUP BY authors.id HAVING COUNT(book_authors.author_id) > 2;`
* Can use `COUNT()` as part of a column selected in `SELECT` and then use it later to `ORDER BY` - e.g.
    ```bash
    SELECT books.name, COUNT(book_authors.book_id) FROM books
    INNER JOIN book_authors
    ON books.id = book_authors.book_id
    WHERE books.release_date > '01-Jan-96'
    GROUP BY books.name
    ORDER BY COUNT(book_authors.book_id) DESC LIMIT 1;
    ```
    * To display the count as well:
    ```bash
    SELECT authors.first_name, authors.surname, COUNT(book_authors.author_id) AS total FROM authors
    INNER JOIN book_authors
    ON authors.id = book_authors.author_id
    GROUP BY authors.first_name, authors.surname
    ORDER BY total DESC LIMIT 1;
    ```
* Can use `ALTER TABLE` 
    ```bash
    ALTER TABLE book_authors ADD book_author_id VARCHAR(20);
    UPDATE book_authors SET book_author_id = (Cast(book_id AS VARCHAR(20)) || '-' || Cast(author_id AS VARCHAR(20)));
    ALTER TABLE book_authors ADD PRIMARY KEY (book_author_id);
    ```


#### Other tips
* > Slightly confusingly, `psql` has its own set of commands that are entirely different from SQL. You can identify them because they start with a backwards slash (\) and **don't** end in a semicolon.
* `psql` commands:
    * `\d` - list all tables (know as 'relations' in psql)
    * `\d [table name]` - give information on a given table
    * `\l` - list all databases
* Don't forget to use semicolons at the end of SQL commands. If you hit enter and you just get empty lines this is probably what you're missing.
* In PostgreSQL words in double quotes mean identifiers like the names of tables and columns. Single quotes are used for values. You can optionally put double quotes around identifiers (but not necessary)
* Note that a single equals is used for equality testing, not assignment.
* > SQL keywords like `SELECT`, `WHERE` etc can be in upper or lower case. The convention is upper case to distinguish them from identifiers and values but PostgreSQL will understand either way.
* > SQL is pretty flexible with whitespace so you can spread your statements out on to as many lines as you want. Keeping things aligned can help make big statements easier to read. Just remember to end with a semicolon!