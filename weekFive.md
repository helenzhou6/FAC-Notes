# Week Five

_Resource:_ [_week five schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-5) - [our condensed version](https://github.com/foundersandcoders/london-programme/blob/master/current-cohort/week5.md) due to Bank Holiday, _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/resources.md).

It's `Node.js` (2) themed week!

## Day One

### TDD - testing server side

_Resource:_ [_TDD workshop_](https://github.com/foundersandcoders/ws-tdd-node-server)

*   > [Tape](https://github.com/substack/tape) is an npm module used for testing server side code written in Node.js.

#### Supertest
*   > [Supertest](https://github.com/visionmedia/supertest) is used in this workshop to simulate fake server requests without the need to have the server listening via a socket connection to respond to the requests. Fake requests are simply objects passed to your routes;

    *   > Supertest introduces the `expect` API, which does some of the work `tape` was doing for us (eg: `res.statusCode` assertions). The [documentation](https://www.npmjs.com/package/supertest#api) indicate that we can use `expect` for testing status codes, header fields, response body, or to pass an arbitrary function to. Combined with [tapes testing methods](https://github.com/substack/tape) you can build a robust set of tests to ensure all your server endpoints are tested.
    *   More notes on `expect` API [here](https://dzone.com/articles/testing-http-apis-with-supertest)
    *   > In examples below - Supertest is given the argument of router. We then define the type of request (here we are saying we want to make a GET request to the home route '/' and the content type), and end with a callback function with the error and response.

    ```
    test('Blog route - post - password - payload', (t) => {
    supertest(router)
        .post("/")
        .send(['a', 'b']) // to send a payload
        .set({ [Headers] }) //setting headers
        .expect(200)
        .expect('Content-Type', 'application/json')
        .end((err, res) => {
            t.error(err)
            t.deepEqual(res.body, ['a', 'b'], 'Should return payload');
            t.end();
        });
    });

    test('Home route returns a status code of 200', (t) => {
    supertest(router)
        .get("/")
        .expect(200)
        .expect('Content-Type', /html/) // using regex
        .end((err, res) => {
            t.error(err);
            t.equal(res.statusCode, 200, 'Should return 200'); // note we have used .expect(200) above so this assertion is not neccesary. This is to show you how to check the statusCode in the res.
            t.end();
        });
    });
    ```

    *   In `router.js`

    ```
    const router = (req, res) => {

    if (req.url === '/') {
        res.writeHead(200, { 'content-type': "text/html" });
        res.end('Hello');

    } else if (req.url === '/blog' && req.method === 'GET') {
        res.writeHead(200, { "content-type": "application/json" });
        var arrayObject = JSON.stringify(['cat', 'dog', 'bird']);
        res.end(arrayObject);

    } else if (req.url === '/blog' && req.method === 'POST' && req.headers.password === 'potato') {
        let data = '';
        req.on('data', (chunk) => {
        data += chunk;
        });

        req.on('end', () => {
        if (data) {
            res.writeHead(200, { "content-type": "application/json" });
            res.end(data);
        } else {
            res.writeHead(302, { 'Location': '/blog' });
            res.end();
        }
        });

    } else if (req.url === '/blog' && req.method === 'POST') {

        res.writeHead(403, { 'content-type': 'text/html' });
        res.end('Forbidden');

    } else {
        res.writeHead(404, { "content-type": "text/html" });
        res.end("unknown uri");
    }
    }

    module.exports = router;
    ```

*   The code in `test.js` (supertest code) is essentially mimicing what the front end will do (sending over data etc)
*   To pass anything around the internet, essentially the data has been subject to `JSON.stringify` on the front end, and then on the backend when receiving the data, need to do `JSON.parse` to revert back to code (even if it is an array).
    *   So in the backend need to do: `JSON.parse(res.text)` BUT can use `res.body` instead where it doesn't need to be parsed (since referring to the whole body not just the text).
*   In the `router.js` backend
    *   Information about the request:
        *   Can use `req.method === 'POST'` to find out the method
        *   `req.body` does not exist when trying to receive the data that been send over, since the data has been sent in chunks. Hence need to do `req.on(‘data’, (dataChunk)` (see above code)
    *   The response given back
        *   You can use `res.end(‘text’)` to immediately send over small amounts of information (shortcut method), whilst use `res.write('var text')` then `res.end()` for large amounts (or when want other steps between selecting the data and ending the response).
        *   `res.end()` is also the same thing as `res.on(‘end’, function(){});`


#### Nock
* [Nock](https://github.com/node-nock/nock) is an npm module that facilitates the 'mocking' of HTTP requests.
* It will intercept any outgoing requests to a defined url, and respond with the dummy data which you give it. (Stub testing)

    [For example](https://github.com/foundersandcoders/prereq-check/blob/master/README.md), in the test below *nock* intercepts any request to the defined domain passed to ```nock```, and responds with the contents of the file passed to ```replyWithFile()```.
    ```js
    tape('Codewars API: getCodewars invalid username', (t) => {
    const username = 'astroashaaa';
    nock('https://www.codewars.com/')
        .get(`/api/v1/users/${username}`) # <--- MUST start with a /
        .replyWithFile(404, path.join(__dirname, 'dummy-data', 'codewars-response-fail.json'));
    getCodewars(username)
        .then((response) => {
        t.deepEqual(response, {
            success: false,
            statusCode: 404,
            message: 'User not found',
        }, 'getCodewars for invalid username returns correct object');
        t.end();
        });
    })
    ```
* `nock` in combination with `supertest` would be integrated testing otherwise it is unit testing.
* NB `nock` requires the URL passed in in the test to match the API URL _exactly_ - so make sure to include API keys etc.
* Look at the documentation but it does handle query string handling
* `nock` is **mocking** type test - since you are interjecting functions that mimic something (in this case returning an API call yourself)

#### nock with supertest - integrated testing
* `supertest` tests what happens when you reach a specific end point VS `nock` is mocking the external API call - and sending back data that we specify (so this is what is `expected` in the test (the `res.body`)).

```js
test.only('Integration test of API call', (t) => {
  nock('https://api.nasa.gov/planetary/apod')
    .get('?date=2018-03-29&api_key=undefined') // URL to match exactly
    .reply(200, { planet: 'Jupiter', photographer: 'Lawrence' }); // or this can be linked to JSON in a dummy file

  supertest(router)
    .get('/api/search?2018-03-29') // your endpoint
    .expect(200)
    .end((err, res) => {
      t.deepEqual(res.body, { planet: 'Jupiter', photographer: 'Lawrence' });
      t.end();
    });
});
```

#### npm test scripts
* If you want to have two suites of tests running with independent commands in the terminal (e.g. `npm run test-router` and `npm run test-logic`, while also having them run together with `npm test`, you can use the structure below - note this also allows Travis to run both test suites whenever it's doing it's CI.
    ```js
        "test": "npm run test-router && npm run test-logic ",
    "test-router": "nyc tape ./test/router.test.js | tap-spec",
    "test-logic": "nyc tape ./test/logic.test.js | tap-spec",
    ```
    * OR `"test": "nyc tape ./test/*.test.js"` where this is neater, and will work the same, provided you are consistent with your file naming (it will run all files in the /test folder that end in `.test.js`)

### Error handling

_Resource:_ [_Error Handling Workshop_](https://github.com/foundersandcoders/error-handling-workshop)

> *   JavaScript is a dynamically typed language. You can call a function with any types of arguments passed to it, and the function will try to execute
>     *   E.g. trying to add a number to an object, the JavaScript interpreter coerces them both to strings and concatenates them together to produce `'[object Object]10'`
> *   Two kinds of errors:
>     *   **Programmer errors**: These are **bugs**; they are unintended and/or unanticipated behaviour of the code, and they can only be fixed by changing the code (e.g. calling a function with the wrong number of arguments)
>     *   **Operational errors**: These are runtime errors that are usually caused by some external factor (e.g. any kind of network error, failure to read a file, running out of memory, etc.)

* Operational errors (such as internal server down or API down) - should be displayed on page for user to comprehend. Rather than reload the page, send back `500` status code from back end to front end:
```js
  request(options, (err, res, body) => {
    if (err) {
      console.log(err);
      response.writeHead(500, { 'Content-Type': 'text/html' });
      response.end('Internal Server error');
    } else {
      // do stuff to DOM
    }
  });
  ```
  * NB: the `response` here refers to the `response` of the back end server returning information back to the front end, whilst `res` that is part of `request` is the response back from the API (used to access `res.headers` etc), and the `body` from request is the data within `res`.
  * And then in your `callback(err, func)` function passed into the `XHR` function -- use `(xhr.readyState === 4 && xhr.status === 500)` to catch the error in the front end and do DOM manipulation. The following is a generic of an **error-first callback**
  ```js
    function console(text, cb) {
        if (typeof text !== 'string'){
            cb (new TypeError) // second argument not passed in = undefined
        } else {
            cb(null, text)
        }
    }
    console ('hello', function(err, text){
        if (err){
            // do error stuff
        } else {
            console.log(text);
        }
    })
  ```
  * In this sense, the `cb` function has two different functions depending on whether an error is passed in or not - if there is an error in the first argument then the function will perform actions based on the error (due to the `if` statement), otherwise (if the error is `null`), it will perform the typical callback functions as expected.

> *   How you should handle any given error depends on what kind of error it is. Operational errors are a normal part of the issues a program must deal with. They typically should not cause the program to terminate or behave unexpectedly. By contrast, programmer errors are by definition unanticipated, and may potentially leave the application with unpredictable state and behaviour. In this case it is usually best to terminate the program.
>     *   There is, however, no blanket rule for what to do; each error represents a specific problem in the context of an entire application and the appropriate response to it will be heavily context dependent.

#### General Principles

> Regardless of the chosen approach, there are some principles which can be generally applied:
>
> 1.  **Be consistent, not ad-hoc.**
>
> *   Inconsistent approaches to error handling will complicate your code and make it much harder to reason about.
>
> 2.  **Try to trip into a failure code path as early as possible.**
>
> *   A _code path_ is the path that data takes through your code. A _success code path_ is the path data takes if everything goes right. A _failure code path_ is the path data takes if something goes wrong.
> *   For example, it may be tempting to return default values in the case of an error and allow the application to continue as normal.
> *   This may be appropriate in some cases, but can often cover up the root cause of an error and make it difficult to track down, or result in unhelpful error messages.
>
> 3.  **Propagate errors to a part of the application that has sufficient context to know how to deal with them.**
>
> *   Many times, we will write generic functions to perform common actions, like making a network request.
> *   If the network request fails, the generic function cannot infer the appropriate response, because it doesn't know which part of the application it has been called from.
> *   It should therefore try to propagate the error to its caller instead of trying to recover directly.

#### Approach 1. Error-First Callbacks

_Resource:_ [_approach 1_](https://github.com/foundersandcoders/error-handling-workshop/blob/master/docs/approach_1.md)

> *   Used as convention for handling errors when using callbacks for asynchronous control flow in Node. Try not to deviate from this pattern when writing async callback-based code.
>     *   A callback argument is passed as an additional argument to a function, which will be executed both in the case where an operational error has occurred, _and_ when the operation completes successfully.
>     *   In the error case, the callback is executed with an `Error` object as its only argument. In the success case, the callback is executed with `null` as its first argument and the result as its second argument. The callback can then check if the first argument is not null and proceed accordingly.
>     *   Structurally similar to returning errors in that it requires checks in the calling code.
>     *   Maintain consistent interfaces; if a function requires a callback, do not also `throw` or `return` errors to the caller

```js
const applyToInteger = (func, integer, callback) => {
    if (typeof func !== "function") {
        callback(
            new TypeError("Invalid argument: First argument is not a function")
        );
    } else if (!Number.isInteger(integer)) {
        callback(
            new TypeError(
                `Invalid argument: Second argument ${integer} is not an integer`
            )
        );
    } else {
        callback(null, func(integer));
    }
};

const applyAndPrintResult = (func, integer) => {
    applyToInteger(func, integer, (err, result) => {
        if (err) {
            console.log("Sorry, result could not be calculated:");
            console.log(err.message);
        } else {
            console.log("Result successfully calculated:");
            console.log(`Applying ${func.name} to ${integer} gives ${result}`);
        }
    });
};
```

#### Approach 2. Throwing and Catching Errors

_Resource:_ [_approach 2_](https://github.com/foundersandcoders/error-handling-workshop/blob/master/docs/approach_2.md)

* Use `try` and `catch` for synchronous code (such as logic based code), since only the `callback` method can be used for asynchronous code (you could try using `try` and `catch` for API calls etc except the `catch` part of the code will run before the API call response is received)
* The function(s) you are trying to test within the `try` block that you have writtin could `throw` an error - using `throw new TypeError("message");` (or with callbacks use `callback(new TypeError("message"))`) - but some functions (or the JS interpreter) can throw errors by default (look into the documentation - for example some functions throw a syntax error if `typeof` input is incorrect).
* The **exception** is raised and an error object is passed, the thrown error will stop further execution of the code and instead go up the stack until it finds the closest `catch` and run that instead.

> *   Throwing
>     *   During runtime, errors can be thrown in our application unexpectedly by computations acting on faulty computations produced earlier (like the first example above). We can also manually throw errors ourselves by using the [`throw`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/throw) keyword. This will immediately terminate the application, unless there is a [`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) block in the call stack.
> *   Catching
>     *   Errors that have been thrown can be caught using a [`try...catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) block.
>     *   The try block tries to execute the code. If no error is thrown during the try block, the catch block will not run. However if the try block throws an error, the catch block will catch the error (even if they are programmer errors) and do something with it.
>     *   Ideally there would be sufficient logic in the `catch` block to differentiate these cases so that we are not at risk of recovering from a programmer error as though it is an operational error.
> *   Guidance
>     *   Throwing can be useful for making critical assertions about the state of your application, especially during startup (e.g. database connection has been established).
>     *   It's not possible to wrap an asynchronous function in a try/catch block, so throwing should only be used with synchronous code. Errors thrown from asynchronous functions will not be caught. To understand why, learn about the javascript [call stack](https://www.youtube.com/watch?v=8aGhZQkoFbQ).
>     *   Remember to use `catch` blocks to avoid inappropriate program termination. (e.g. a server should usually not crash in the course of dealing with a client request). / Without `catch` blocks codebases that `throw` errors extensively will be very fragile.
>     *   Do not simply log the error in a `catch` block.
>     *   Note that `catch` will trap errors that are thrown at any point in the call stack generated by the `try` block.

```js
const applyToInteger = (func, integer) => {
    if (typeof func !== "function") {
        throw new TypeError(
            "Invalid argument: First argument is not a function"
        );
    }
    if (!Number.isInteger(integer)) {
        throw new TypeError(
            `Invalid argument: Second argument ${integer} is not an integer`
        );
    }

    return func(integer);
};

const applyAndPrintResult = (func, integer) => {
    try {
        const result = applyToInteger(func, integer);

        console.log("Result successfully calculated:");
        console.log(`Applying ${func.name} to ${integer} gives ${result}`);
    } catch (e) {
        console.log("Sorry, result could not be calculated:");
        console.log(e.message);
    }
};
```

#### Approach 3. Returning Errors to the Caller

_Resource:_ [_approach 3_](https://github.com/foundersandcoders/error-handling-workshop/blob/master/docs/approach_3.md)

> *   Rather than throwing the error, another approach you might consider is simply to return it to the caller. Instead of a `try/catch` block, we have an `if/else` that checks the return value using the [`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof) operator.
> *   Guidance
>     *   Requires more granular error checking throughout the codebase, as the returned value from a single function is checked.
>         *   A single `catch` block can catch errors that are thrown in multiple functions if it is placed high in the call stack (which it usually should be). No similar mechanism exists if errors are simply returned.
>         *   This places a burden on the developer(s). If they forget to place error checks on return values that might be `Error` objects, they are effectively introducing a programmer error that may result in other errors being thrown elsewhere in the application.
>     *   Does not catch programmer errors.
>     *   This is a specific example of a more general pattern, namely, returning an object which indicates whether the operation has been successful or not. Alternatives include returning an object with a `success` or `isValid` field.

```js
const applyToInteger = (func, integer) => {
    if (typeof func !== "function") {
        return new TypeError(
            "Invalid argument: First argument is not a function"
        );
    }

    if (!Number.isInteger(integer)) {
        return new TypeError(
            `Invalid argument: Second argument ${integer} is not an integer`
        );
    }

    return func(integer);
};

const applyAndPrintResult = (func, integer) => {
    const result = applyToInteger(func, integer);

    if (result instanceof Error) {
        console.log("Sorry, result could not be calculated:");
        console.log(result.message);
    } else {
        console.log("Result successfully calculated:");
        console.log(`Applying ${func.name} to ${integer} gives ${result}`);
    }
};
```


#### Real life example
##### Front end handling
* When writing error handling within our projects:
```js
  const xhrRequest = function (url, callback) {
    const xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
            try {
                const response = JSON.parse(xhr.responseText);
            } catch (e) {
                console.log(e);
            }
            callback(null, response);
        } else if (xhr.status === 500) {
          callback(new TypeError('500 error'));
        } else {
          callback(new TypeError('error: ' + xhr.readyState));
        }
      } else {
        console.log('XHR error', xhr.readyState);
      }
    };
    xhr.open('GET', url, true);
    xhr.send();
  };

  // Somewhere in the code to call the request
  fetch(buildURL(), displayResults);

  displayResults = function(err, result){
      if (err) {
          // DOM manipulation to notify user of error - USE the err.message
      } else {
          // do generic DOM manipulation stuff with result
      }
  }
```
* Functions such as `xhrRequest` should just do an XHR request and nothing else -- it should be unaware of the DOM/the environment in which it is called - these should be referred to in the `callback` (since it is the code you use to call `xhrRequest` that should be aware of what it is doing/how it relates to its environment)
* `JSON.parse` will throw an exception if it's given a string that isn't valid JSON. This may be rare, but it's best to be sure, especially if that error will prevent you from responding to the client, or take down your server entirely. Or prevent your client-side code from updating the UI to let your user know what's happened.

##### Back end handling
* Using the `request` module:
```js
  request(options, (err, res, body) => {
    if (err || response.statusCode !==200) {
      console.log(err);
      response.writeHead(500, { 'Content-Type': 'text/html' });
      response.end('API error :' + err.message); // if there is an err.message provided by API
    } else {
        try {
            const bod = JSON.parse(body);
            const result = filter(bod); // NB filter is not a good name
            response.writeHead(200, { 'Content-Type': 'application/json' });
            response.end(JSON.stringify(result));
        } catch (err) {
            response.writeHead(500, { 'Content-Type': 'text/html' });
            response.end('Error, could not initiate API request, error: ' + err.message);
        }
    }
```
* Look at the API documentation - but if there is an error coming from the API it should come with an `err.message`. This can then be passed back to the frontend with the appropriate status code and dealt with in the frontend (to notify the user of what has happened).
* The `try` and `catch` can be used here for the `JSON.parse` and `filter` function.

## Day Two

### Linting
_Resource:_ [_linting FAC article_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/linter.md)
* Linters allows you to check syntax and fix formating, and also allows you to share a config file between your team to avoid merge conflicts.
1. Install `eslint`
2. `./node_modules/.bin/eslint --init` - initialises eslint on your project
After this pick the following inputs:
    - [How would you like to configure ESLint?] `Use a popular style guide`
    - [Which style guide do you want to follow?] `Airbnb`
    -   [Do you use React?] `No`
    - [What format do you want your config file to be in?] `JSON`
* Can now run linter on your files using 
`/node_modules/.bin/eslint [yourfile.js]` OR install the `eslint` package on VS Code.
* Also - to disable eslint for your client side files - add this to the top of your files: `/* eslint-disable */`

### Research afternoon
_Resource:_ [_the topics_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/research-afternoon.md)

#### Streams and Buffers
_Resource:_ [_research notes_](https://hackmd.io/47c_--FATayzYyC9ngDjFg)
* `fs.readfile` is what is needed to send over files over the internet, since only `strings` or `buffers` can be sent over the internet (where the `Buffer` class - a global within `Node.js` is uesd for reading or manipulating streams of binary data)
* > By default, no encoding is assigned and stream data will be returned as Buffer objects. Setting an encoding causes the stream data to be returned as strings of the specified encoding rather than as Buffer objects. For instance, calling `readable.setEncoding('utf8')` will cause the output data to be interpreted as `UTF-8` data, and passed as strings. Calling `readable.setEncoding('hex')` will cause the data to be encoded in hexadecimal string format.

#### Continuous Integration (CI) & Travis
_Resource:_ [_research notes_](https://hackmd.io/vj9FRHykQ1y68Nfk0l3PWQ), [_dwyl guide_](https://github.com/dwyl/learn-travis)

* [**Travis**](https://travis-ci.org/) is an automatic verification system to run tests automatically and continuously (on pull requests etc)
* Link your Github account to Travis & enable Travis on your selected project (using Travis interface).
* Add a `.travis.yml` file to the root of your project, containing:
    ```js
    notifications:
    - email: false
    language: node_js
    node_js:
    - "node"
    ```
* More detailed information/guide by dwyl [here](https://github.com/dwyl/learn-travis)

#### Make a request from the server
_Resource:_ [_the example project_](https://github.com/i2xzy/request-research)

### Build the Request module
_Resource:_ [_the morning challenge_](https://github.com/foundersandcoders/mc-request-module-workshop)

#### `request` module
* Can use `request` module to help make HTTP requests - to make API calls from the backend
* Instructions on how to use [here](http://stackabuse.com/the-node-js-request-module/)
    ```js
    const request = require('request');

    const options = {  
        url: 'https://www.reddit.com/r/funny.json',
        method: 'GET',
        headers: {
            'Accept': 'application/json',
            'Accept-Charset': 'utf-8',
            'User-Agent': 'my-reddit-client'
        }
    };

    request(options, function(err, res, body) {  
        let json = JSON.parse(body);
        console.log(json);
    });
    ```

#### Under the hood
* The module is doing the `HTTP` request for you - [HTTP request -- Node.js](https://nodejs.org/api/http.html#http_http_get_options_callback)
    ```js
    const myRequest = (url, cb) => {
        http.get(url, (response) => {
            response.setEncoding('utf8');

            let body = '';
            response.on('data', (data) => {
                body += data;
            });

            response.on('end', () => {
                cb(null, response, body);
            });

        }).on('error', (err) => {
            cb(err);
        });
    };
    ```
    * And where `cb` is essentially the below:
    ```js
    cb = function (error, response, body) {
        console.log('error:', error);
        console.log('statusCode:', response && response.statusCode);
        console.log('body:', body);
    });
    ```

    * NB: The JSON Fetching Example found in [this Node.js article](https://nodejs.org/api/http.html#http_http_get_options_callback) includes more error handling (for when `statusCode !== 200` and `!/^application\/json/.test(contentType)`), where it states 
    ```js
    error = new Error('Invalid content-type.\n'+`Expected application/json but received ${contentType}`);
    ```

    * ... And it also does a `try` and `catch` on `res.on('end' [...])`
    * More on `res.resume()`  - [here](https://nodejs.org/api/stream.html#stream_readable_resume) - where "The `readable.resume()` method causes an explicitly paused Readable stream to resume emitting 'data' events, switching the stream into flowing mode."
    * And it uses `console.error` - more [here](https://developer.mozilla.org/en-US/docs/Web/API/Console/error)

### Project
_Resource:_ [_week 5 project_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/project.md)

* Doing an XHR request to request information from the front end to the back end server, and then using the `request` module to send an API request from the back end server. 
* Our code [here](https://github.com/fac-13/jeth)
* To refresh the page with new data every 3s when the user on the page (e.g. for the most up to date news), you can use `setInterval(function(){ //XHR request }, 3000);`
* Infinite scroll works by capturing the height of the window and the offset from this value, and in scroll event if almost the height of the page (near the bottom) then another XHR request is sent. [See here for the code](https://github.com/fac-13/research/issues/25)

#### heroku
##### Deployment via Heroku CLI
* When within your local repo, in terminal do the following:
    ```
    heroku login
    heroku create
    git push heroku master
    heroku ps:scale web=1
    heroku open
    ```
* Then add a PROCFILES file (no extension) to root and inside add `web: node src/server.js` — needed to let heroku know what commands are used to start the server

##### Deployment via GitHub
* Connect your GitHub to your Heroku account, find the repo, and then enable automatic deploys from master.

## Other snippets of code
*   Terminal commands
    *   You can use `&&` between two commands in terminal -- if the left is true, then use the right code.
    *   `node [filename]` runs the file in terminal
    * `node` to start using terminal like a browser console.
    * `-D == —save-dev`
* [Overview of Blocking vs Non-Blocking - Node.js](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)
* To test your front end to back end API calls, use `curl` in your terminal (like `node`) or in your browser to see the response.
    * Since this can identify bugs in your front end code (such as preventing `e.preventDefault()` on `submit` buttons - since buttons with submit within a form by default will try to refresh the page* on click (OR adding the attribute `type="button"` within `html` - but this would mean you need to handle submission on click and on pressing the enter key))
    * > *Given that the input field is inside of a form with a 'submit button', you don't need to add an event listener to it to call the XHR request. The default behaviour of the form is to call the first 'submit' button available.
* If you want to redirect a page on the front end, you can use `window.location` - but this (and also redirecting on the backend) is bad practice for the user experience when it comes to error handling (when filling out a form and something goes wrong, you don't want to go to a whole new page)
* When using `forEach` - when you initiate a new empty array and then loop over an old array and use `push` to add new items to the new empty array <=> should use `map`.
    * > The only case where it's not equivalent is if there's conditional logic in the body of the `forEach` callback which results in fewer or more elements in the new array than the original (in this case you can use `reduce` instead).
* With the `input` html tag - you can set the type such as `type="data"` to bring up a default calender for the user to select dates.
* `handler.js` file should ideally not have much blocking events. For example using `require` to get `JSON` is done asynchronously by `Node.js` whilst getting is using `filepath` is synchronous (so should stick to using `require`)