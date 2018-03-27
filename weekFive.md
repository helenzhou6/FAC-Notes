# Week Five

_Resource:_ [_week five schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-5) - [our condensed version](https://github.com/foundersandcoders/london-programme/blob/master/current-cohort/week5.md) due to Bank Holiday, _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-5/resources.md).

It's `Node.js` (2) themed week!

## Day One

### TDD - testing server side

_Resource:_ [_TDD workshop_](https://github.com/foundersandcoders/ws-tdd-node-server)

*   > [Tape](https://github.com/substack/tape) is an npm module used for testing server side code written in Node.js.
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

### Error handling

_Resource:_ [_Error Handling Workshop_](https://github.com/foundersandcoders/error-handling-workshop)

> *   JavaScript is a dynamically typed language. You can call a function with any types of arguments passed to it, and the function will try to execute
>     *   E.g. trying to add a number to an object, the JavaScript interpreter coerces them both to strings and concatenates them together to produce `'[object Object]10'`
> *   Two kinds of errors:
>     *   **Programmer errors**: These are **bugs**; they are unintended and/or unanticipated behaviour of the code, and they can only be fixed by changing the code (e.g. calling a function with the wrong number of arguments)
>     *   **Operational errors**: These are runtime errors that are usually caused by some external factor (e.g. any kind of network error, failure to read a file, running out of memory, etc.)
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

## Other snippets of code

*   Terminal commands
    *   You can use `&&` between two commands in terminal
    *   `node [filename]` runs the file in terminal
