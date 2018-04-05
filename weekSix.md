# Week Six

_Resource:_ [_week five schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-6) - [_our condensed version_](hhttps://github.com/foundersandcoders/london-programme/blob/master/current-cohort/week6.md) due to Bank Holiday, _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-6/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-6/resources.md).

It's Database themed week!

## Day One

### Introduction
Database is a systematic collection of data.
* Data can have different relationships to each other including one to many, many to one and many to many.
There are different **Database Management Systems (DBMS)** including:
* > [**Relational DBMS**](https://www.guru99.com/introduction-to-database-sql.html) - this type of DBMS defines database relationships in form of tables, also known as relations. It does not support many to many relationships. Relational DBMS usually have pre-defined data types that they can support.
    * It saves data in table formats. Each row of data has an `id` (that is a **PRIMARY KEY** and unique to each entry in that table), and **FOREIGN KEY**s in a table (can be unique or not depending on one to many or many to one relationship) that acts to reference the `id` of another table.
* _PostgreSQL_ is an example of a RDBMS (that we will be using)
* _Structured Query language (SQL)_ is the standard language for dealing with Relational Databases. There are a few different syntaxes when using SQL for databases.

### STEP 1) PostgreSQL Setup
_Resources:_ [_FAC setup instructions_](https://github.com/macintoshhelper/learn-sql/blob/master/postgresql/setup.md)

* **CLI** (command line interface) MAC instructions:
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

### SQL Commands Intro
_Resources:_ [_FAC workshop_](https://github.com/foundersandcoders/sql-commands-intro/) _and_ [_ws3 schools SQL_](https://www.w3schools.com/sql)

* **Schema diagrams** show the different tables and relationships to them e.g. for _books_ table:
--- | --- | ---
id | integer | not null default
name | character varying(100) | not null
release_date | date | not null
publisher_id | integer | foreign key (publishers.id)

#### Retrieving data using basic SQL
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
    ```sql
    SELECT id,
       CASE WHEN age<20 THEN 'true'
            ELSE 'false'
       END AS teenager
    FROM users;
    ```
#### Adding data using SQL
* Using [`INSERT INTO`](https://www.w3schools.com/sql/sql_insert.asp) to add data - `INSERT INTO [table name] ([column 1 name, column 2 name]) VALUES ([new column 1 value, new column 2 value)]`
* Using [`UPDATE`](https://www.w3schools.com/sql/sql_update.asp), update data = `UPDATE [table name] SET [what to change e.g. user_id=5] WHERE [condition e.g. text_content=‘Hello world’]`

#### General
* Can use **subqueries** (nesting queries inside) e.g.:
    ```sql
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
* naming convention — do it all lowercase

### PostgreSQL Workshop
_Resources:_ [_PostgreSQL Workshop_](https://github.com/foundersandcoders/postgres-workshop) 

#### Set up
* Workshop using a **server-client model** where we're running both on the same machine but if we wanted to we could have the server running on a different computer and connect to it via the client.
* > We will be working with the dataset in the `data.sql` file. This file contains a set of SQL commands that will create a set of tables and fill them with data. We will connect to the PostgreSQL server running locally on our individual computers and tell it to run the file.
* If you run into problems, on a Mac using Homebrew, run `brew services restart postgresql` and try to connect again.
* On terminal within directory:
    * `psql --file=data.sql` (or if doesnt work try `psql -f data.sql`). This will connect to your PostgreSQL server and run all of the SQL in `data.sql`, setting up our database for us.
    * We can connect to our newly-created database by running `psql` (if it gives an 'access denied' error, try `psql -U [your-user-name]`).

#### Challenges - SQL commands
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
    ```sql
    SELECT books.name, COUNT(book_authors.book_id) FROM books
    INNER JOIN book_authors
    ON books.id = book_authors.book_id
    WHERE books.release_date > '01-Jan-96'
    GROUP BY books.name
    ORDER BY COUNT(book_authors.book_id) DESC LIMIT 1;
    ```
    * To display the count as well:
    ```sql
    SELECT authors.first_name, authors.surname, COUNT(book_authors.author_id) AS total FROM authors
    INNER JOIN book_authors
    ON authors.id = book_authors.author_id
    GROUP BY authors.first_name, authors.surname
    ORDER BY total DESC LIMIT 1;
    ```
* Can use `ALTER TABLE` 
    ```sql
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

## Day Two

### STEP 2) Writing up a database in backend
_Resources:_ [_Code along/walkthrough_](https://github.com/foundersandcoders/pg-walkthrough) and [_the step by step guide_](https://github.com/foundersandcoders/pg-walkthrough/blob/master/walkthroughSteps/code-along-mentor-notes.md) and [_pg workshop_](https://github.com/foundersandcoders/pg-workshop)

* READ [the steps!](https://github.com/foundersandcoders/pg-walkthrough/blob/master/walkthroughSteps/code-along-mentor-notes.md)

#### Creating the database schema

* `database/db_build.sql`- This file serves to build your database structure (tables etc) once run. It is only run once. This file should never be used in production other than for initialisation. You only want to use this to reset your test database (and can add mock data for it).
    * Put a `BEGIN;` at the start of the file and a `COMMIT;` at the end of the file. **Init queries** should be written between these lines.
        * The code inside it is a transaction, so the multiple queries are run as one query/chunk. If an error occurs inside of here, it will rollback the previous commands, preventing messing up the database. Can think of it as SQL error handling
    * Inside write: `DROP TABLE IF EXISTS [tablename] CASCADE;` that drops the table and `CASCADE` will delete tables with relations (that have a `REFERENCE` defined towards) to `[tablename]` too.
    * Write the schema:
        ```sql
        CREATE TABLE [tablename] (
          id SERIAL PRIMARY KEY,
          name VARCHAR(100) NOT NULL,
          superPower TEXT NOT NULL,
          weight INTEGER DEFAULT 100
        );
        ```
        * All tables should have an integer `id` that is set as a `PRIMARY KEY` - this is used relate databased together (integer `PRIMARY KEY` helps with performance).
            * `PRIMARY KEY` also adds `UNIQUE` and `NOT NULL` (primary keys have to be unique).
        * `VARCHAR(LENGTH)`, `INTEGER`, `TEXT` (unlimited length, but larger data usage), etc are SQL data types.
        * `NOT NULL` tells the PostgreSQL to give an error if this is not set.
        * `DEFAULT 100` changes `NULL` values to be `100`.
    * Initialise some mock/hardcoded data:
        ```sql
        INSERT INTO [tablename] (name, superPower, weight) VALUES
          ('Wolverine', 'Regeneration', 300),
        ```
        OR
        ```sql
        INSERT INTO [tablename]
        VALUES
          (DEFAULT, Wolverine', 'Regeneration', 300),
        ```
        * Rows separated with commas and each bracket, `(comma-separated values inside here)`, has a row inside it with values

#### Connecting the database
* Pg is a non-blocking PostgreSQL client for node.js that lets you access SQL values as JavaScript data values. Translates data types appropriately to/from JS data types.
* `database/db_connection.js` -- This sets up your connection / makes the door of the database (the library as a metaphor). The **Pool** is like setting up the card reader (where the `DB_URL` in `config.env` is the keycard that can grant access)
    * Install and require `pg` and `env2` (node modules)
        ```js
        const { Pool } = require('pg');
        const url = require('url');
        require('env2')('./config.env');
        ```
        * `env2` allows you to access `DB_URL` set in `config.env`. `config.env` is a *gitignored* file with environment variables which can be accessed with `process.env.NAME_HERE` *OR* production environments with `Heroku`
        
        *  > [`Connection pooling`](https://en.wikipedia.org/wiki/Connection_pool) is a cache of multiple database connections that are kept open for a timeout period (`idleTimeoutMillis`) and reused when future requests are required, minimising the resource impact of opening/closing connections constantly for write/read heavy apps. Reusing connections minimises latency too. Debug/demo logging `Pool` might be helpful.
        ```js
        if (!process.env.DB_URL) throw new Error('Environment variable DB_URL must be set');
        ```
        * The if statement will deliberately crash the script if the _connection information_ variable is missing

    * Parse the URL and authentication info with this code:
        ```js
        const params = url.parse(process.env.DB_URL);
        const [username, password] = params.auth.split(':');
        ```
        
        * `url.parse(<url string here>)` will split a URL/HREF string into an object of values like `protocol`, `auth`, `hostname`, `port`: [URL split documentation](https://nodejs.org/api/url.html#url_url_strings_and_url_objects)
    * Create a [`pg options`](https://node-postgres.com/features/connecting#programmatic) object:
        ```js
        const options = {
        host: params.hostname,
        port: params.port,
        database: params.pathname.split('/')[1],
        max: process.env.DB_MAX_CONNECTIONS || 2,
        user: username,
        password,
        ssl: params.hostname !== 'localhost',
        }
        ```
        * The information to generate the key reader on the door is based on the `DB_URL` within the `config.env` file (so that the sensitive information will not be uploaded onto github).
        * > Use an appropriate number for `max`. More connections mean more memory is used, and too many can crash the database server. Always return connections to the pool (don't block/freeze query callbacks), or the pool will deplete. More connections mean more queries can be run at once and more redundancy incase connections are blocked/frozen.
        * `ssl` will enable SSL (set to true) if you're not testing on a local machine.
            * > TLS / SSL (Secure Sockets Layer) ensures that you are connecting to the database from a secure server, when set to `true`, preventing external networks from being able to read/manipulate database queries with MITM attacks
    * Export the Pool object with options with:
    ```js
    module.exports = new Pool(options);
    ```
#### Setting up the intial build of the database
* `database/db_build.js` -- The file opens the connection to the database using `.query`, and uses the SQL commands within `db_build.sql` to build the initial structure of the database. It is run once and after the `config.env` file has been set up.
    ```js
    const fs = require('fs');

    const dbConnection = require('./db_connection');

    const sql = fs.readFileSync(`${__dirname}/db_build.sql`).toString();

    dbConnection.query(sql, (err, res) => {
      if (err) throw err;
      console.log("Super heroes table created with result: ", res);
    });
    ```
    * Where `dbConnection` is the exported `Pool` (the exported Pool constructor/object with the previously set options object) so that this file can use this connection pool with `dbConnection.query`.
    * The `.query` opens the door to the database.
    * `sql` is a string of the build script. Think of it as a single query (transaction / collection of queries compiled into one).
    * This file should only be run separately. NEVER run this in a production after setup, or from other files (unless you know what you're doing)

#### Connecting the dots - locally
* SO you connect the dots by creating the database > setting up your username/password of the user you are logged into to match DB_URL > running the database build (to populate database with your defauly schema) > connecting to it within `pgcli`.

##### Connecting and having local database
* In the directory on your terminal, run `psql` or `pgcli` (a Postgres CLI client)
* Create the database by typing `CREATE DATABASE [tablename];`
* Create a user specifically for the database with a password (though not necessary?)
    * Type `CREATE USER [the new username] WITH PASSWORD '[the password of the database]'`;
        - The password needs to be in single-quotes, otherwise you get an error
        - For security: In production/public facing server, clear command history and use a password manager with 25+ random characters - and use a firewall
        - A new user is made specifically for the application/database, so if this user is compromised, other databases should remain safe. (security: avoid use of superusers/root + give minimum permissions needed)
    * Change ownership of the database to the new user by typing `GRANT ALL PRIVILEGES ON DATABASE [name of the database] TO [the new username];`.
* Add a `config.env` file and add the database's url in this format: `DB_URL = postgres://[username]:[password]@localhost:5432/[database]`. The username and password that has been generated previously and that you are logged in with locally (the library card) _need to match_ the `[username]:[password]` of `DB_URL` --> since if not you can not access the library since it needs to match the Pool options/the door card reader (that has been generated from the `DB_URL` in `config.env`). If it matches, then it allows the user to connect to the database once within `pgcli`.
    * The `config.env` has the sensitive information (`DB_URL`) that is structured in a way to obtain certain bits of information needed to make the door key card reader.
    - Don't use semi-colons or apostrophes for strings in `config.env`, or use alternative JSON notation
    - This is your keycard to access the database
* Now we build the tables we set out in `db_build.sql` by running our `db_build.js` file by running: `node database/db_build.js` in command line. OR `psql --file init.sql` in bash OR within `pgcli` do `\i db_build.sql` to run the SQL build.

* Connect to the database by typing `psql postgres://[username]:[password]@localhost:5432/[database]` OR when within `pgcli` use `\c [databasename]`
    * If you experience permission problems, try running `psql superheroes` then `GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO [the new username];`

* OR can also do:-
    To create a database, run:
    ```sh
    createdb <database name>
    ```
    And to connect to it, run:
    ```sh
    psql <database name>
    ```

##### Connecting to a Heroku database
_Resources:_ [_Morning challenge (SQL commands)_](https://github.com/foundersandcoders/db-morning-challenge)

* Within repo and using [Heroku CLI]((https://devcenter.heroku.com/articles/heroku-cli)
* Create an app on Heroku `heroku create app-name-here --region eu`
* Push to Heroku `git push heroku master`
* Create a new database on Heroku using `heroku addons:create heroku-postgresql:hobby-dev --as USERS_DB`
* Find the database url, either on the heroku dashboard for your project, under settings (click reveal config vars), or by using `heroku config -s | grep USERS_DB` (note: make sure to remove the quotations around the url when using this method).
* Back in your command line, create a `config.env` file with the url of your new database. You can do that like this
  `$ echo "export DATABASE_URL = {YOUR_COPIED_DATABASE_URL}" >> "config.env"`
* Build your database by running: `$ node database/db_build.js`
* So now can access your database from the command line with `psql [YOUR_COPIED_DATABASE_URL]`

#### Connecting to our database from the backend server
* `src/dynamic.js` -- this file connects to the database (using `.query`, queries the database using the SQL command passed in as a string, and the response (`res`, and also `err`) is an array of objects from the database. The `res` could be the data needed to be passed to the front end via an endpoint.
    * It is similar to using the `request` module to make an API request from the backend to an external API.
 * Write an asynchronous `getData` function that takes and returns a callback, and is a module that is exported. Since an error first callback is used, the `getData` function serves to only get data from the databased based on the sql command (and the callback is where it manipulates/sends the data etc)
    ```js
    const getData = (cb) => {
    dbConnection.query('SELECT * FROM superheroes;', (err, res) => {
        if (err) return cb(err);
        console.log('res.rows: ' + res.rows);
        cb(null, res.rows);
    });
    };
    ```
    * For getting data, `dbConnection.query` takes the arguments `dbConnection.query(<sql query string goes here>, <callback function with (err, res)>)`
    * If there's an error, return `cb(err)` - the return prevents the success code running.
    * `res.rows` is an array of objects, where the objects are columns and values.
* Write a post data function where `dbConnection.query` takes the arguments `dbConnection.query('INSERT INTO table_name (name) VALUES ($1) TO ', [name], <callback function with (err, res)>`
    ```js
    const postData = (topicTitle, description, cb) => {
        dbConnection.query('INSERT INTO users (topicTitle, description) VALUES ($1, $2)', [topicTitle, description], (err, res) => {
            if (err) {
            return cb(err);
            }
            else {
            cb(null, res);
        }
        })
        }
    ```
* Inside `handler.js`, import `getData` and `postData` and pass in the callback function. The `handlers` functions is where it receives the data from the database (the `res`) OR assets from the server, and manipulates it/handles it in response to the endpoint that the specific handler function is called (in `router.js`).
    * For `getData`:
        ```js
        const getDataHandler = (response) => {
            const query = "SELECT * FROM users";
            getData(query, (err, res) => {
                    if (err) {
                    console.log(err);
                    response.writeHead(500, { "content-type": "text/plain" });
                    response.end("server error");
                } else {
                    let output = JSON.stringify(res);
                    response.writeHead(200, { "content-type": "application/json" });
                    response.end(output);
                }
            });
        };

        ```
        * `getData` is asynchronous, so `response.end` should be inside it, so it doesn't run before the data comes back from the database request (same as an API request).
    * For `postData`:
        ```js
        const postDataHandler = (request, response) => {
            let body = "";
            request.on("data", chunk => (body += chunk));
            request.on("end", () => {
                const data = querystring.parse(body);
                const topicTitle = data.topic_title;
                const description = data.description;
                postData(topicTitle, description, (err, res) => {
                if (err) {
                    console.log(err);
                    response.writeHead(303, { Location: "/" });
                    response.writeHead(500, { "content-type": "text/plain" });
                    response.end("Something went wrong");
                } else {
                    response.writeHead(200, { "content-type": "text/plain" });
                    response.writeHead(303, { Location: "/" });
                    response.end(`Successfully added ${topicTitle}`);
                }
                });
            });
        };
        ```
        * Where `topicTitle` and `description` are two values that have been submitted from the front end (via `<form method="POST" action="/[appropriate-endpoint]">` with a `submit` button that has two inputs with `name="description"` etc) that need adding to the database.
        * The posted data is coming in as a stream - so need to collect the chunks
* Inside `router.js`, call the relevant handlers.

### Other tips
* Within `pgcli`
    * Can use `\d` to list databases, `\l` for tables and `dt` for ...??


## Day Two

### STEP 3) Testing databases
_Resources:_ [_DB testing workshop_](https://github.com/foundersandcoders/ws-database-testing/)