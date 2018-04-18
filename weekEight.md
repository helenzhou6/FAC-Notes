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
    ![library vs framework](https://github.com/foundersandcoders/introduction-to-express/raw/master/images/library-framework.png)
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
    ![middleware flow](https://github.com/foundersandcoders/express-workshop/blob/master/images/middleware.jpg?raw=true)
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

### Handlebars
_Resource:_ [_Express Handlebars workshop_](https://github.com/foundersandcoders/express-handlebars-workshop)

* **HTML templating** - serving dynamic files from the backend using `Handlebars` (thus avoid duplication)
* `Handlebars` is a **template engine** designed to combine reusable text (i.e. templates) with dynamic data in order to generate HTML.
    * It builds on top of [Mustache](https://github.com/janl/mustache.js).
    * Can be used for client-side and server-side **rendering**.
* It minimises writing JS logic inside the templates, and have embedded placeholders that "hold" data.


#### Model-View-Controller (MVC) model

> This is a common architectural design pattern to separate the data, their representation in the browser and the logic of your application. Think of your application as three separate parts talking to each other:
1. **Model**: data stored in a database // directory - for data processing, database builds and SQL queries.
1. **View**: the aesthetics/representation of data in your application // directory  holds the `.hbs` (handlebars) files (i.e. the html templates to 'view'). At the root there is a `.hbs` for each page that the client will view, and there is a folder for helpers, layouts and partials.
1. **Controller**: the *brain* of your application // directory - for routes, linking

![A diagram of the MVC model](https://cdn-images-1.medium.com/max/1600/1*4SxbmCrI5YVp1Uyj1Jstsg.png)

Image Cred: Real Python (from [this medium post](https://medium.freecodecamp.com/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053#.3autr7o1d))

![handlebars type of templates](https://i.imgur.com/LUPgUxp.png)

> It is generally good practice to separate the view (presentation/ aesthetics) and the model (actual data), as this encourages clean code organisation. More information on the MVC model [here](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) and [here](https://medium.freecodecamp.com/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053#.3autr7o1d).




#### Using it with Express
* Express has extensive support for template rendering and can load and leverage Handlebars.js.

* **Rendering** in the context of express refers to the [`render`](http://expressjs.com/en/api.html#res.render) function - where we tell express which rendering engine to use (in this case handlebars) in order to take various pieces of input and compose them into an html string to be sent to the client.
    * We do this in `./src/app.js` - `app.engine` tells express to use the **view engine** (express-handlebars) to render hbs files. We are also configuring express-handlebars by giving the various directories that the engine will search for, and the default layout.
    ```js
    const exphbs = require("express-handlebars");

    const controllers = require('./controllers/index');
    const helpers = require('./views/helpers/index');

    const express = require('express');
    const app = express();

    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'hbs');
    app.engine(
        'hbs',
        exphbs({
            extname: 'hbs',
            layoutsDir: path.join(__dirname, 'views', 'layouts'),
            partialsDir: path.join(__dirname, 'views', 'partials'),
            defaultLayout: 'main',
            helpers,
        })
    );

    app.set('port', process.env.PORT || 3000);
    app.use(favicon(path.join(__dirname, '..', 'public', 'favicon.ico')));
    app.use(express.static(path.join(__dirname, '..', 'public')));
    app.use(controllers);
    ```

##### Main home page
* `./src/views/layouts` directory holds **layouts** (i.e. base templates). It includes:
    * `main.hbs` that has the default 'view', with 
    ```html
    <main>
        {{{ body }}}
    </main>
    ```
    * `{{{ body }}}` will be the hbs file we are rendering.
* In the root `./src/views` contains `home.hbs` - the content specific to the 'home' page (that will replace the `{{{ body }}}`)

* `./src/controllers` directory - the routes
    * `home.js` has `res.render()` that renders a view - specifically it renders the `home.hbs` HTML inside of the `{{{ body }}}` within `main.hbs` (it does this by default/magic) - and sends the rendered HTML string to the client.
        * And `exports.get` is similar to `module.exports()`
    ```js
    exports.get = (req, res) => {
        res.render("home");
    };
    ```
    * `index.js` - Express looks for this file that contains the routes/endpoints and links them with the appropriate imported `res.render()`.
    ```js
    router.get("/", home.get);
    ``` 
* And `./src/views/partials` directory that holds **partials** (smaller templates within templates). Partials allow us to reuse snippets of HTML preventing duplication - to put into the partials anything that is repeated across multiple pages/layouts.
    *   `htmlHead.hbs` contains the `<head>[content]</head>`
        * Link it inside `main.hbs` using the syntax `{{>htmlHead}}`

##### Other pages
* So now we have the main homepage set up, but for other 'view' pages, for each page have a seperate `.hbs` file within the `./src/views` root.

* `./src/controllers/index.js`:
    ```js
    const express = require('express');
    const path = require('path');
    const router = express.Router();

    const fruits = require('./fruits');
    router.get('/fruits', fruits.get);
    ```

* `./src/model/index.js` has `module.exports = ['watermelon', 'strawberry'];`

* `./src/controllers/fruits.js`
    ```js

    const fruits = require('./../model/index');

    exports.get = (req, res) => {
        res.render('fruits', { activePage: { fruits: true }, fruits });
    };
    ```
    * `res.render()`/ the relevant controller also takes an object as an optional second argument (in this case the `fruits` **model** array from `./src/model/index.js`). Within the view (e.g the hbs file being rendered) values passed in on the options object will be available as variables named after the object keys. This allows us to insert values or use values along with handlebars helpers (see below!) to otherwise customise the view.
    * To obtain things passed in using the url - `const { fruit } = req.params;` 

* `./src/views/fruits.hbs` - has the template for the page (including partials where needed), which will be rendered as `{{{ body }}}` in the `main.hbs` layout.
    * It can access information passed into the second argument of `res.render()`
    * We can now use handlebars' built in **helpers** to dynamically generate HTML, such as:

    ```html
    {{#each fruits}}
        <li class="fruit__item">
            <a class="fruit__link" href="/fruits/{{this}}"><div class="fruit__image" style="background-image: url(/images/fruits/{{this}}.jpg)"></div></a>
            <div class="fruit__info">
                <h2 class="fruit__name">{{capitalize this}}</h2>
                <svg class="fruit__fav-btn"><use id="js-fav-btn" xlink:href="#icon-heart"></use></svg>
            </div>
        </li>

    {{/each}}
    ```

##### Helpers
* **helpers** (functions used in templates to manipulate data) to ultimately render **views**.
    * For CSS styling of the current page, can pass in `res.render([hbs file name], {active page: { home: true}})` and within `header.hbs` add `{{#if activePage.home}}navbar__link--active{{/if}}`

* We can create our own helpers in plain javascript; custom **helpers** are within `./src/views/helpers` directory.
    * This also has a require `index.js` file to refer to the other files - an index for all the helpers:
    ```js
    module.exports = {
        capitalize: require('./capitalize'),
        uppercase: require('./uppercase'),
    };
    ```
    * `./src/views/helpers/capitalize.js` contains `module.exports = str => str[0].toUpperCase() + str.slice(1);`
    * `./src/views/helpers/uppercase.js` contains `module.exports = str => str.toUpperCase();`
    * These can then be used within the `.hbs` files, such as `{{capitalize this}}` (within `{{#each fruits}}`)
* To link it, within the `./src/app.js`, the index file is imported - `.const helpers = require("./views/helpers/index")` and then referred to within `app.engine(...helpers...)`. 

##### Error pages
* `./src/views/layouts/error.hbs` - the error main page is different enough from a default page to warrant another main template page. 
    ```html
    <!DOCTYPE html>
    <html>
        {{>htmlHead}}
    <body>

        <main class="main">
        {{{body}}}
        </main>

    </body>
    </html>
    ```
* `./src/views/error.hbs`
   ```html
   <section class="error">
     <p class="error__desc">{{statusCode}}. <span class="error__msg">{{errorMessage}}</span></p>
     <img class="error__image" src="/images/error.png" alt="error">
   </section>
   ```
* `./src/controllers/error.js` - specifying `layout: 'error'` means the default layout (`./src/views/layouts/main.hbs` - as specified in `app.js > app.engine()`) is overridden and instead `./src/views/layouts/error.hbs` will be used. The other paramaters passed in (`statusCode: 404` and `errorMessage:[text]`) are just other second arguments that can be accessed in the `.hbs` file.
    ```js
    const path = require('path');

    exports.client = (req, res) => {
        res.status(404).render('error', {
            layout: 'error',
            statusCode: 404,
            errorMessage: 'Page not found',
        });
    };

    exports.server = (err, req, res, next) => {
        res.status(500).render('error', {
            layout: 'error',
            statusCode: 500,
            errorMessage: 'Internal server error',
        });
    };
    ```
* `./src/controllers/index.js`
    ```js
    const router = express.Router();
    const error = require('./error');

    router.use(error.client);
    router.use(error.server);
    ```
##### Other
* NB: `<script>` tags that pull in `js` files you've made yourself should usually be at the bottom of the `<body>`, so that they only load after all the HTML has been loaded.
    * So within `./src/views/layouts` add `{{#if activePage.fruits}}<script src="/js/fruit-fav.js"></script>{{/if}}`