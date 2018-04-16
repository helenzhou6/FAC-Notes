# Week Eight

_Resource:_ [_week eight schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-8) _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-8/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-8/resources.md).

It's Handlebars and Express themed week!

## Day One

### Express
_Resources:_ [_Intro to express_](https://github.com/foundersandcoders/introduction-to-express/blob/master/README.md)

* A lot of information [here](https://github.com/foundersandcoders/introduction-to-express/blob/master/README.md)
* Express **handles low level functionality of the Node web server** so that you can focus on the business logic of your app.
* > Express gives you an API, submodules, and methodology and conventions for quickly and easily tying together all the components necessary to set up a functional web server with all the conveniences necessary (static asset hosting, templating, CORS, cookie parsing, POST parsing etc.)
* It is:
    *  is a **framework** - i.e. you need to insert your behaviour into various places in the framework (so it calls upon your code at these points). VS a **library** which is a set of functions that you can reuse and call.
    ![library vs framework](./images/library-framework.png)
    * is **unopinionated**  - has less restrictions than **opinionated** frameworks (e.g. you can any directory structure and insert almost any compatible middleware you like into the request handling chain)
    * provides only a thin layer of **abstraction** over vanilla Node - i.e. interacting with Node without directy working with complex details **"under the hood"**.

#### Setting up express
_Resources:_ [_Intro to express_](https://github.com/foundersandcoders/introduction-to-express/blob/master/README.md) _and_ [_express workshop_](https://github.com/foundersandcoders/express-workshop)

```js
const express = require('express');

const app = express();

app.get('/', (req, res) => {
  res.send('Hello world');
});

app.listen(3000, () => {
  console.log('App running on port 3000');
});
```
* `app` (an Express application) is an object that has methods for "routing HTTP requests, configuring middleware, rendering HTML views, registering a template engine, and modifying application settings that control how the application behaves (e.g. the environment mode, whether route definitions are case sensitive, etc.)"
    * E.g. `app.get()` section shows a **route definition** - and `app.get()` is a method that calls a callback function when there is an HTTP GET request to '/'.

#### 1) Route handlers

* The **route handler** callbacks for GET requests has _two_ arguments: `(req, res)`

* GETTING DATA:
* Custom endpoints
    ```js
    app.get('/:city', (req, res) => {
        const city = req.params.city;
        res.send('Hello ' + city[0].toUpperCase() + city.slice(1));
    });
    ```
    * Sending data:
        * Use `res.json({key: value})` to send JSON.
        * `res.sendFile(path.join(__dirname, [the path from current file]))` to send a file
    * The endpoint:
        * Note the use of `/:city` to get specific parts of the url
        * Can also use `'/new-?york'` for `/newyork` or `/new-york` - more examples [in the express docs](http://expressjs.com/en/guide/routing.html)
        * Or `/*` and `req.params[0]` to get the url

* For favicon and static file handling:
    ```js
    const favicon = require('serve-favicon');
    app.use(favicon(path.join(__dirname, '..', 'public', 'favicon.ico')));
    app.use(express.static(path.join(__dirname, '..', 'public')));
    ```

* POSTING DATA
    ```js
    app.post('/fruit', (req, res) => {
        <!-- do stuff with req.body -->
        res.redirect('/fruit');
    });
    ```
    * Use `const bodyParser = require('body-parser');`, then `app.use(bodyParser.json());` to parse incoming JSON and `app.use(bodyParser.urlencoded({ extended: false }));` to parse incoming data sent via HTML form POST method (the `extended: false` dictates whether the data is parsed using `qs` or `querystring`)
        * This ensures that all the data received has been decoded, and deals with the streams of data incoming too (i.e. `data += chunk` etc.)

* 404 middleware function: no file found
    ```js
    app.use((req, res) => {
        res.status(404).sendFile(path.join(__dirname, '..', 'public', '404.html'));
    });
    ```
* 500 middleware function: server error - Express uses promises, and an error that occur are caught using the error-handling middleware function. Express will know to use this function when errors occur since it has _four_ arguments `(err, req, res, next)`
    ```js
    app.use((err, req, res, next) => {
        console.log(err.message);
        res.status(500).sendFile(path.join(__dirname, '..', 'public', '500.html'));
    });
    ```
* Need to add `.catch(err => next(err));` at the end of all the handlers

#### 2) Middleware functions
* [**Middleware functions**](http://expressjs.com/en/guide/writing-middleware.html) has _three_ arguments: `(req, res, next)` (which is the only difference between a middleware function and a route handler callback)
* Middleware and routing functions are called in the order that they are declared - so ensure middleware is called before setting routes.
    * `next()` is called if the middleware does not complete the request cycle - since it is the next function (that could be another middleware) in the application’s request-response cycle.
    ![middleware flow](./images/middleware.jpg)
* Use `app.use()` to call the middleware function. It runs the functions sequentially (NB the order) - as long as use `next()` to skip to the next function.
    ```js
    app.use((req, res, next) => {
        console.log(Date.now(), 'before');
        next();
    });

    app.get('/', (req, res, next) => {
        res.send('Hello world');
        next();
    });

    app.use((req, res) => {
        console.log(Date.now(), 'after');
    });
    ```

##### Morgan Middleware
* `const morgan = require('morgan');` - `morgan` middleware is an HTTP request logger middleware function 
    ```js
    const accessLogStream = fs.createWriteStream(
    path.join(__dirname, '..', 'logs-demo', 'access.log'),
    { flags: 'a' }
    );
    ```
    * This creates a write stream in append mode. The `a` in `flags` refers to append (so appends text to the bottom of the file, if there is no file it creates one)
    ```js
    app.use(morgan('combined', { stream: accessLogStream }));
    ```
    * This sets up morgan logger in `'combined'` (i.e. defines what information to be logged) and stream data to the write stream using `accessLogStream`. This logs data (as the standard Apache combined server log output) to the `access.log` file.

##### Modularisation
* Look [here](https://github.com/foundersandcoders/express-workshop/tree/solution/13-split-into-modules/src) for the file structure AND [here](https://github.com/foundersandcoders/express-and-testing-workshop/tree/solution-branch) for another structure (that also includes database SQL queries)
* `index.js` (is like `server.js`) that has the app entry point/is the starting point (NB that should be listed on the `package.json`:
    ```js
    const app = require('./app');
    app.set('port', process.env.PORT || 3000);

    app.listen(app.get('port'), () => {
        console.log('App running on port', app.get('port'));
    });
    ```
    * We split the server out into a different file so that our tests don't hang because the server is listening still
* `app.js` (is like `router.js`, but also called `server.js` 🙄)
    ```js
    app.disable('x-powered-by');
    app.use(compression());
    app.use(favicon(path.join(__dirname, '..', 'public', 'favicon.ico')));
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: false }));
    app.use(
        express.static(path.join(__dirname, '..', 'public'), { maxAge: '30d' })
    );
    app.use(controllers); // where const controllers = require('./controllers/index');

    ```
    * `app.disable('x-powered-by');` disables the `Powered by express` header'
    * And sets the Set port number
    * And can use `app.use('/v1/api/', routes);` where the endpoints within routes from `./routes.js` automatically is `/v1/api/[endpoint]`.
    * `bodyParser.urlencoded` is to decode data sent using the default HTML form POST method (it is urlencoded rather than JSON)
* And `controllers/index.js` (like the `handlers.js` file that links other handler files).
    ```js
    const router = express.Router();
    const fruit = require('./fruit');
    const error = require('./error');

    router.get('/fruit', fruit.get);
    router.post('/fruit', fruit.post);
    router.use(error.client);
    router.use(error.server);

    module.exports = router;
    ```
    * To create routes in express use: `express.Router()`
    * And within `error.js` has `exports.client = (req, res) => {};` and `exports.server = (req, res) => {};`, and `fruit.js` has `exports.get = (req, res) => {};` and `exports.post = (req, res) => {};`.

* NB: from the second structure, that includes database queries:
    * `dbConnection.js`
        ```js
        const pgp = require('pg-promise')();

        const herokuDB = {
            host: process.env.HEROKU_HOST,
            user: process.env.HEROKU_USER,
            password: process.env.HEROKU_PW,
            database: process.env.HEROKU_DB,
            ssl: true,
        };

        const localDB = {
            host: 'localhost',
            port: 5432,
            database: 'fac-express',
        };

        const connection = process.env.NODE_ENV === 'production' ? herokuDB : localDB;

        const db = pgp(connection);
        module.exports = db;
        ````
    * `dbBuild.js`
        ```js
        const { QueryFile } = require('pg-promise');
        const path = require('path');
        const db = require('./dbConnection');

        const sql = file => QueryFile(path.join(__dirname, file), { minify: true });

        const build = sql('./build.sql');

        db
            .query(build)
            .then(res => console.log('res', res))
            .catch(e => console.error('error', e));
        ```
        * NB: the `pg-promise` module returns `db.query` as a promise, so that can do:
        ```js
        router.get('/facsters/:name', ({ params: { name } }, res, next) => {
        queries
            .getSingleFacster(name)
            .then(person => res.status(200).json(person))
            .catch(err => next(err));
        });
        router.post('/facster/new', ({ body }, res, next) => {
        queries
            .addFacster(body)
            .then(userID => queries.getFacsterById(userID))
            .then(user => res.status(201).json(user))
            .catch(err => next(err));
        });

        ```
        * Where `queries.getSingleFacster` is a function imported from `./queries` that queries the SQL database using SQL.
        
### Integrated testing
* **Integration tests** for a backend server (i.e. tests that check the correct functioning of several interconnected functions all working together)
    * Can do this using `Supertest` and `Tape` to ensure Server and Database are communicating properly.
        * Tape allows you to make assertions and check that things are equal etc. `supertest` will allow you to make requests to your server and expect certain results, and also has some limited assertion/testing functionality.
* What to test:
    * What response you would want from the API.
    * The status code, the content type, and the contents of the body.
        * Can use `.expect()`, though to catch any error from these test need to add `if(err) throw err` OR `t.error(err, [error message for all the .expect()s])` and make the test fail.
    * Make sure you write tests for each route.
```js
test("All tests for facsters/:name endpoint", t => {
  request(app)
    .get("/facsters/amelie")
    .expect(200)
    .expect('Content-Type', /json/)
    .end((err, res) => {
      t.equals(Array.isArray(res.body), true, "Returns an array");
      t.equals(typeof res.body[0], "object", "Object at index 0");
      t.deepEquals(Object.keys(res.body[0]).length, 4, "Array length is 4");
      t.deepEquals(
        Object.keys(res.body[0]),
        ["id", "firstname", "surname", "cohort"],
        "Array keys correct"
      );
      t.deepEquals(
        res.body[0],
        { id: 2, firstname: "Amelie", surname: "Mystery", cohort: 11 },
        "Object contents are correct"
      );
      t.error(err, 'No error');
      t.end();
    });
});

test("Test /new endpoint", t => {
  request(app)
    .post("/facster/new")
    .send({ firstname: "Helen", cohort: 13, surname: "Zhou" })
    .expect(201)
    .expect('Content-Type', /json/)
    .expect({ data: { firstname: "Helen", cohort: 13, surname: "Zhou" } }) // or use res.body like below
    .end((err, res) => {
      t.error(err, 'No error');
      t.same(res.body[0].firstname, 'jason', 'Should add JSON bourne to FAC');
      t.end();
    });
});

test('Should be able to get a facster by their name', t => {
  const names = ['Aseel', 'Bart', 'Amelie', 'Abdullah'];
  names.forEach((name, index) => {
    request(app)
      .get(`/v1/api/facsters/${name}`)
      .expect(200)
      .expect('Content-Type', /json/)
      .end((err, res) => {
        t.same(res.statusCode, 200, 'Status code is 200');
        t.error(err, 'No error');
        t.same(res.body[0].firstname, name, `Name is ${name} as expected`);
        if (names.length - 1 === index) {
          t.end();
        }
      });
  });
});
```

