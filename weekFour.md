# Week Four

_Resource:_ [_week Four schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-4) _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/resources.md).

It's Node themed week!

## Day One

### Node

_Resource:_ [_Node Intro workshop_](https://github.com/foundersandcoders/Node-Intro-Workshop)

*   **Node.js** is a JavaScript runtime (helps you write JS programs) built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking (asynchronous), single-threaded I/O model (it talks to networks, file systems or other I/O (input/output, reading/writing) sources.) It is mainly used for web servers or file system utilities.
*   Node handles I/O with: callbacks, events, streams and modules.

#### Server

*   A server is a computer program that receives requests from other programs, the client, and sends back a response, e.g. to share data, information or hardware and software resources. E.g. the website address asks server for files to display the website.
    ![Image of what server is](https://camo.githubusercontent.com/6d63917fa946de22ba4e84fa4adbb6000d73352a/68747470733a2f2f66696c65732e6769747465722e696d2f6865726f6e323031342f4669694b2f7365727665722e706e67)

        * Handle manipulation of data in the database
        * File manipulation
        * Authentication
        * Secret Logic
        * Client side sends requests to a server which sends back data to the front end to be displayed

*   Usually your server is faster than your client's machine
*   **Front-end** Client-side JavaScript runs in the visitor's browser (in the `public` folder), whereas **back-end** server-side code runs on a website's web server and deals with logic (in the `src` folder)

#### Modules

*   Small packaged programs you can integrate with the other programs you are writing. Three types:
    *   **core modules** - the Node core library comes installed with Node automatically. Includes `fs`, `http`(helps us process our server requests and responses), `path`, `querystring` and `url`.
    *   **third-party modules** or **packages** - thousands of open-source modules - Node.js' package ecosystem, `npm`, has a large ecosystem of open source libraries
    *   Access by 'requiring' them: `var http = require('http');`.

#### NPM

_Resource:_ [_introduction to NPM_](https://github.com/foundersandcoders/npm-introduction)

*   **Node Package Manager (npm)** is a tool, installed with Node, for managing Node's ecosystem of modules in your projects.

    *   Used to install tools, packages as dependencies for our projects, and also publish our own packages.
    *   Are downloaded specific to repo (try to ignore npm -global)
        *   NPM website/library has libraries of ‘plugins’ to use (e.g. validator of strings for forms).
    *   npm comes with its own command-line interface you can use in your terminal while within your relevant project folder (e.g. `npm search`, `npm install x --save-dev` and `npm init`). These are built in scripts (such as `npm start` = `node server.js` and `npm test`) that can be accessed by defauly. Otherwise you can add your own 'shortcuts' to the `scripts` section in `package.json`, which can be accessed using `npm run [own script name]`.
    *   Use `npm help [command]` in terminal to get information on the commance.
    *   Steps after running `npm init` - that installs a Node virtual environment - [here](https://github.com/shannonjensen/node-workshop/blob/master/step01.md) (i.e. dealing with the `package.json` file - that contains meta-information about your project).

#### The `package.json` file

*   Once initiated using `npm init`, the `package.json` file is automatically generated specific to that repo.
*   Packages are recorded, that are either 'dependencies' if they are needed for the browser to run your project/need to be installed on the server delivering your app (using `npm install <package-name> --save`) and 'dev-dependencies' that are packages related to the project, but the brower does not need to know about (e.g. testing frameworks, using `npm install <package-name> --save-dev`).
*   `package.json` should be pushed onto GitHub so that anyone who wants to work on your project can download it, and then be able to download all the NPMs they need to work on the project quickly.
*   You can edit the `package.json` file, e.g.:


```
"scripts": {
   "test": "nyc tape src/test.js | tap-spec",
   "start": "node src/server.js",
   "dev": "nodemon src/server.js"
 },
```

Where `tap-spec` NPM makes the output of `test.js` prettier, and `nodemon` NPM runs the tests every time there is a change in the file.

#### Modularising your code

*   For the frontend, wrap everything in an IIFE for your `DOM.js`, and then use `<script>` to load it in the HTML.

    *   In a `logic.js` file that is loaded using `<script>` in the html (above the others).


    ```
    var module = {
        publicMethod: function(text){
            console.log(text);
        }
    };

    if (typeof module !== 'undefined') {
        module.exports = module;
    }
    ```

    BUT better for privacy to use IIFEs:

    ```
    var module = (function(){
        var publicMethod = function(text){
            console.log(text);
        }
        return {
            publicMethod // which is the same as publicMethod: publicMethod
        }
    })();
    ```

    *   In `DOM.js`


    ```
    (function() {
        publicMethod.console('hi');
    })();
    ```

    *   Wrapping in a closure makes it an Immediately invoked function expression (IIFE) - definition [here](https://codeburst.io/javascript-what-the-heck-is-an-immediately-invoked-function-expression-a0ed32b66c18). So with the parenthesis, the function is invoked, whilst without it, the function definition is returned instead.
    *   More IIFEs [here](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) and [here](https://toddmotto.com/what-function-window-document-undefined-iife-really-means/). IIFEs are immediately called at runtime and only run once, so good for privacy.

*   For backend modularisation, you can use `require` at the top. So have the `module.js` - first version in a file. And then in the other file put `const module = require('./logic');` and then to access use `logic.console()`.
*   To export multiple functions do `module.exports = {console, other};` (you still need to call it in the same manner in the other files, the same as if you exported the whole `module` function, except in this manner you can select which functions to export and which ones not to. This is called **deconstruction**). Also when using require if it is a `.js` file, you don't need to put that extension. Then put `var {console, other} = require('./logic.js’);` (or `var {console}; = require('./logic.js’);` for one) in other files (as long there is deconstruction on both files).
    *   Also the conventional way to export multiple functions is not to wrap it all into a larger object, rather just separated functions in one file, then on import do: `const {console, x } = require (‘./module’)`. This means you only import the ones you need, and no longer need to do `module.console()` (just do `console()`).
    *   Relative paths are needed for local modules `(‘./module’)` but not for core modules.
    * Another example: `const { Pool } = require('pg');`
        * `{ Pool }` is syntactic sugar (shorten/simplify syntax with abstraction) ([destructuring assignment](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)) that is equivalent to:
        ```js
        const pg = require('pg');
        const Pool = pg.Pool;
        ```
*   More examples [here](http://www.matteoagosti.com/blog/2013/02/24/writing-javascript-modules-for-both-browser-and-node).

-   You can only use `require` in the backend! In the required file, wrap them in an object, with each function linking to a key, and `module.exports` the bottom (either the whole object or just selecting a few functions to export)

## Day One & Two

### Building a Content Management System (CMS)

_Resource:_ [_Node Girls/Shannon edition workshop_](https://github.com/shannonjensen/node-workshop), [_another solution by finn_](https://github.com/finnhodgkin/sturdy-giggle/tree/master/src), [_modulisation morning challenge SIMPLE ALT (PROB THE BETTER ONE) solution_](https://github.com/foundersandcoders/modules-challenge/tree/alt-solution) and [_modulisation morning challenge COMPLEX solution_](https://github.com/foundersandcoders/modules-challenge/tree/solutions/src)

Some general points:

*   In the `src` backend folder:
    *   `server.js` is the hub linking things together/restaurant layout - It should include references to only two packages: `http` and your `router` package.
    *   `router.js` is the mail room with mail employees/waiters receiving orders - This is the package that acts as the traffic cop in your application, mapping incoming requests to appropriate application responses. The router should reference only a single package: `handlers.js`.
    *   `handler.js` is the kitchen for orders. The handlers are where the behaviour of your application is defined and where most of the work in your application is likely to be done. In a simple application like this one, all the handlers can go in a single file; in complex applications, the main handler may reference several different files. Unlike the main server file and the router, the handlers are going to need access to your data model.
        > *   `model.js` (not yet covered). The data model is not just the file where you put all your database connexions, but also where you define how you can access that data and what the data should look like. If any assumptions about your database leak into your handlers, something has gone wrong. You should be able to completely change the database you are using and it have no efffect on the rest of your application. Concerns should be kept separate between data and presentation.
*   Can think of **endpoints** in the backend as creating a mail slot for directing incoming mail requests from the front end (from URL requests or others like form actions). Some endpoints never appear in the URL (depending on the type of `HTTP` method) and do not redirect (such as the `href` to assets and AJAX/XHR requests).
    *   Here the HTML for the form is:


    ```
    <form class="" action="/create/post" method="post" id="the-form">
        <h2>Write a Blog Post</h2>
        <textarea name="post" rows="8" cols="40"></textarea>
        <button type="submit">Send</button>
    </form>
    ```
    Where it uses the endpoint `/create/post` to post the data. But even if there is redirection back to `/` written in the backend (since default for `type="submit"` is to redirect to the `/create/post` URL), it still jumps the page to the top. To prevent this, it can be better to write a custom XHR request in the frontend (that sends after pressing the submit button and using `event.preventDefault()`), then leave off the `action` and `method` parts in the HTML and change it to `button type=“button”`
    *   API requests using a specific URL is their API server side posting information to that specific endpoint location.
*   When linking your backend with your frontend:
    *   The latter part of the url is like asking for a specific employee working in the back end. So this part of the URL in the frontend (or `action=/x` with forms in the HTML) must match the backend endpoint.
    *   Also best practice to put frontend code in a `public` folder, and then the frontend HTML requests need to include it (e.g. `href=“public/main.css”`) - so that in the backend, if there is 'public' included in the URL (using `indexOf`, it will know an asset is being requested, and it will match the file path (`/public/[assetname]`)

#### Setting up the server

*   In file `src/server.js`
*   Use `http.createServer()` to build the server
*   `server.listen(3000, function(){}` - taking **port** number 3000 and the callback function. A **port** is needed for the server to listen to.
*   `node server.js` on command line to turn on server (can use `nodemon` to refresh server)
    *   For 'shortcuts', can write mini scripts in the `package.json` file (convention to do the following, NB `start` is needed for [heroku](https://www.heroku.com/):


    ```
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
    ```

#### Handling GET requests

*   When a request reaches the server, we need a way of responding to it. The `router` function receives requests and routes them to any appropriate handler functions (which handle the requests) or handles the requests directly (like giving the server a pre-paid addressed envelope)

```
function router (request, response) {
  response.writeHead(200, {"Content-Type": "text/html"});
  response.write(message); //response body
  response.end(); // finish response
}
```

*   Can use one of the methods of the `response` object ^ see above block code.
    *   E.g. `response.write()`. Every response has a header that contains information (argument: status code and header object (if not listed server will try and guess) - `response.writeHead(200, {"Content-Type": "text/html"});`
*   `var server = http.createServer(router);` - pass in the `router` function
*   **endpoint** is the part of the URL that comes after `/`. Inside `router` function: `var endpoint = request.url;` (can check what type of HTTP method request was using `request.method`)
*   To send any file from the server use core module `fs` - file system - that allows you to read and write to and from your hard drive. First server needs to read it using `fs` method: `fs.readFile('path to the file', callback);`
    *   In router function:


    ```
      if (endpoint === "/") {
        fs.readFile(__dirname + '/public/index.html', function(error, file) {
            if (error) {
                res.writeHead(500, {'content-type': 'text/plain'});
                res.end('server error');
            } else {
                response.writeHead(200, {"Content-Type": "text/html"});
                response.end(file);
            }
        });
    }
    ```
    *   `__dirname` is a Node global object that gives you a path to current working directory. We can use this instead of writing the whole path.
    *   use `..` to go up a directory
    *   `.` is look in current folder
    *   Can do `__dirname + '/../public/index.html'` or `path.join(__dirname, '..', 'public', 'index.html')`
*   A webpage can have several different requests to the server: E.g. One is the original browser request, another is a request sent by `<link>`, and the last one is a request sent by `<img>`
    *   Can write a generic route that is able to deal with lots of different assets (in `handler.js` file):
        *   `handleHomeRoute` - when `endpoint === '/'` (gets the `index.html` file)


        ```
        function homeHandler(request, response) {
            response.writeHead(200, {
                'Content-Type': 'text/html'
            });
            fs.readFile(__dirname+"/../public/index.html", function(error, file) {
                if (error) {
                    response.writeHead(500,'Content-Type: text/html')
                    response.end("<h1> sorry, we've had a problem on our end</h1>");
                } else {
                    response.writeHead(200,'Content-Type: text/html')
                    response.end(file);
                }
            });
        };
        ```
        *   `handlePublic` - the **static file handler** - for assets (when the URL path literally is the backend path to the specific file wanted - since in other cases the endpoints may not be asking for a specific file).
            *   It uses the file name to find the file within the `public` folder and the file extension to pick the right `Content-Type`


        ```
        function staticFileHandler(request, response) {
            var extensionType = {
                html: 'text/html',
                css: 'text/css',
                js: 'application/javascript',
                ico: 'image/x-icon',
                jpg: 'image/jpg',
                png: 'image/png'
            };
            var extension = request.url.split('.')[1];
            fs.readFile(__dirname+"/../public" + request.url, function(error, file) {
                if (error) {
                    response.writeHead(404, 'Content-Type: text/html');
                    response.end("<h1>404 file not found</h1>")
                } else {
                    response.writeHead(200, `Content-Type: ${extensionType[extension]}`);
                    response.end(file);
                }
            });
        };
        ```
*   Can use `respondWith(res, 200, getContentType(endpoint), file);` or `respondWith(res, 404, 'text/plain', 'Error loading content');` instead, where:
    ```
    const getContentType = endpoint => {
    // Get the content type based on the file extension
    return {
        css: 'text/css',
        html: 'text/html',
        js: 'application/javascript',
        png: 'image/png',
        ico: 'image/x-icon',
    }[endpoint.split('.')[1]];
    };
    const respondWith = (res, statusCode, contentType, content, headers) => {
        res.writeHead(statusCode, headers || { 'Content-Type': contentType });
        res.end(content);
    };
    ```
*   NB: can also use `path.extname()`
*   Can also do:
    ```
    const url = /;
    const staticRoute = {
        ‘/‘: ‘fac.html’
    }[url]
    ```
    So `staticRoute = fac.html` Which is the same as having the object in `const staticRoutes` and then doing `const staticRoute= staticRoutes[url]`
*   It's good practice to separate routes and handlers into their own files. For core module do `const x = require('x');` and for modules in own code write `const x = require('[path e.g. ./x]');` for dependencies, and to export modules do `module.exports = {x, x};`
    *   In `router.js` file:
        *   NB It is best practice to put the static file handler in the penultimate position, and the final `else` as a `404` to catch certain errors when the user puts in something stupid in the `URL`.
        *   In order to have it penultimate, in the front end asset requests should include 'public/[assetname]


    ```
    const router = (request, response) => {
        const url = request.url;
        if(url === '/') {
            handler.handleHomeRoute(request, response);
        }
        else if (url.indexOf('public')!==-1){
            handler.handlePublic(request, response, url);
        } else {
            response.writeHead(404);
            response.end('404 resource not found');
        }
    }
    ```
    *   In `server.js` file:


    ```
    const http = require('http');
    const port = process.env.PORT || '5000';
    const router = require('./router.js');
    const server = http.createServer(router);
    server.listen(port, (err) => {
        if (err) throw err
    console.log('server running on: http://localhost:' + port);
    });
    ```

#### Handling POST requests

*   Using the `POST` http request method
*   `<form action="/create-post" method="POST">`

-   when e.g. submitting a `form`.
    *   The `action` attribute (in the HTML) is where the form data will be sent (essentially this is the URL that it will be redirected to) and the `name` attribute is used later to reference the data.
    *   When you hit Send, the form will send a `POST` request to the server, using the `/create-post` endpoint.

##### Receiving form data on the server

*   Data doesn't come through the server in one go; it flows to the server in a stream. Need to collect a bucket of water in the server
*   Listen for the 'data' event and when ot starts to arrive, collect the chunks of data into the `allTheData` variable.
    ```
    request.on('data', function (chunkOfData) {
        allTheData += chunkOfData;
    });
    ```
*   Once all the data has come through, listen to the 'end' event (so all POST requests need `response.on` and `response.end`:
    ```
    request.on('end', function () {
        var convertedData = querystring.parse(allTheData);
        console.log(allTheData);
        response.end(); // need this to finish
    });
    ```
* `querystring` module (using `const querystring = require("querystring");`) to handle grabbing what the user has inputted from the endpoint. And endpoint of `/search/?q=topic`, then `let searchedInput = querystring.parse(query)['/search/?q'].toLowerCase().trim();`
*   NB Need `querystring.parse()` to convert the `allTheData` **query strings** (how html forms send data over the internet) to an JS object (to use it) - to deal with spaces, punctuation, casing and random user inputted spaces
* Querystring turns the endpoint into an object with key value pairs - so need to use `?date=` etc. so it know which part of the endpoint is the key, and which is the value.

#### Final solution

To creating an blog post website. General points:

*   NB `server.js` is in the root for some reason, should really be in the `src` folder (but if you do change it remember `..` to go up a directory when writing file paths)
*   We encountered problems as soon as we added the `script.js` file on the front end [in step 10](https://github.com/shannonjensen/node-workshop/blob/master/step10.md). This is because the purpose of [this front end javascript](https://github.com/shannonjensen/node-workshop/blob/master/public/script.js) is to reload the page (upon the document being ready), and obtain blog post data from the backend using an **XHR/AJAX request** using the URL `/posts`. So the whole thing was crashing since it was trying to find the endpoint `/posts` that had not been written yet.
    *   Using an XHR request this way means the frontend is reaching an endpoint without any page redirection (to the `/posts` URL, hence why there is no need to write a redirection involving `"Location": "/"`.
*   Only through XHR requests can the front end request and receive for information in the backend.
*   The blog information is a [`posts.json` file](https://github.com/shannonjensen/node-workshop/blob/solution/src/posts.json) that is JSON (the key is the timestamp and value is the blog post text).

In `router.js` file (adapted so may not have correct func names):

```
const router = (request, response) => {
    const url = request.url;
    if(url === '/') {
        handler.homeHandler(request, response);
    } else if (url === "/create/post" || url.indexOf('create')!==-1)) { // this could it `indexOf` 'api'
        handlers.createPostHandler(request, response);
    } else if (url === "/posts") {
        handlers.getPostsHandler(request, response);
    } else if (url.indexOf('public')!==-1){
        handler.staticFileHandler(request, response, url);
    } else {
        response.writeHead(404);
        response.end('404 resource not found');
    }
}
```

And in the `handler.js` file:

##### Getting posts - receiving data

So the `script.js` file sends a request for all the blog posts using the `/posts` endpoint. You can either use the `fs.readFile` method, but an easier way (since JSON is a JS file), is to require it at the top and then send over the file \* NB by using `require`, it parses the data by default so will need `JSON.stringify` to turn the data back and be able to send it.

*   With `fs.readFile`, it's arguments are `(filePath, (err, data))`, - where the second argument is a callback that takes in two arguments, error and the data (see [here](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback)).

```
var oldPosts = require('./posts.json');
function getPostsHandler(request, response) {
    response.writeHead(200, {'Content-Type': 'application/json'});
    response.end(JSON.stringify(oldPosts));
};
```

##### Creating posts - sending data

*   To prevent the whole page redirecting to the page `/create-post` after submitting, can use the `endpoint` if statement to redirect to an endpoint, and use `response.writeHead(200, {"Location": "/"});` and use the redirect [status code](https://httpstatuses.com/). -- which is what is happening in this case, but for form submission you could do the method described in the General Points section above.
*   With `fs.writeFile`, it's arguments are `(filePath, data, err callback)`, and the third argument is the error callback function.
*   N.B. where the data must be parsed or stringified.
*   There needs to be a redirection to `/` since even if there is an error, the response needs to be ended in order for the page to move on (and not just be a loading bar that keeps waiting for data to come back)

```
function createPostHandler(request, response) {
    const blogPost = '';
    request.on('data', function(dataChunk) {
        blogPost += dataChunk;
    });
    request.on('end', function() {
        var parsedPost = queryString.parse(blogPost);
        var timePosted = Date.now();
        var blogEntries = oldPosts;
        blogEntries[timePosted] = parsedPost.post;
        fs.writeFile('./src/posts.json', JSON.stringify(blogEntries), function (error) {
            if (error) {
                res.writeHead(500, {'content-type': 'text/plain'});
                res.end('server error');
            }
        });
        response.writeHead(307, {
            'Location': '/'
        });
        response.end();
    });
};
```

## Day Two

### ES6

_Resource:_ [_the ES6 morning challenge_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/morning-challenge-day-2.md)

*   You can use ES6 on the back end, since you determine what your server does, whilst ES5 on the front end since the client may not support ES6 yet.
*   ES6 features (some listed [here](https://github.com/foundersandcoders/mc-es6-challenge))
*   [This article](https://hackernoon.com/es6-features-you-need-to-know-now-b525e2b0755e) (particularly the first half) also quite relevant:

`(trace ? trace: ‘’)` in argument, is equivalent to putting within the function:

```
if (!trace) {
    trace = '';
}
```

[Set data type](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```
// SPREAD - used to combine

  var arrOne = [0];
  var arrTwo = [1, 2, 3, 4];

  // ES5
  var newArr = arrOne.concat(arrTwo);

  console.log('Spread - ES5 :', newArr);

  // ES6
  // Incorrect
  var newArrayTwo = [arrOne, arrTwo];

  console.log('Spread - ES5 incorrect:', newArrayTwo);

  // Correct
  var newArray = [...arrOne, ...arrTwo];

  console.log('Spread - ES6 correct:', newArray);

// -----------------

// SET - used to remove duplicates
// A set object takes in iterable values (e.g. array or string)
// It contains unique values (no duplicates)

  var array = [1, 2, 3, 4, 3];

  // ES5
  var setTwo = array.filter(function (value, index, self) {
    return self.indexOf(value) === index;
  });

  // ES6
  var set = new Set(array);

  console.log('Set - ES5 :', setTwo);
  console.log('Set - ES6 :', set);


// USING IN COMBINATION
  var cat = {
    legs: 4,
    name: 'Helen'
  }
  var bird = {
    legs: 2,
    name: 'Phat',
    color: 'blue'
  }

  // ES6
  var result = new Set([...Object.keys(cat), ...Object.keys(bird)])
  console.log('Combo - ES6 :', result);

  // ES5
  var arrKeys = Object.keys(cat).concat(Object.keys(bird)).filter(function (value, index, self) { return self.indexOf(value) === index });
  console.log('Combo - ES5 :', arrKeys);
```

* **Deconstruction**, Example:
    ```js
    params.auth = helen:ac123;
    const [username, password] = params.auth.split(':');
    ```
    * `[username, password]` is a ES6 destructuring assignment that is syntactic sugar for:
        ```js
        const username = params.auth.split(':')[0];
        const password = params.auth.split(':')[1];
        ```
    * Where username is index 0 of `params.auth.split(':')` and password is index 1, and so on.

### Research afternoon

_Resource:_ [_the research afternoon instructions_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/research-afternoon.md)

#### Architecting

#### Engineering

[_hackMD notes_](https://hackmd.io/ysExIh0bSX2hfBiGjp9bbA)

#### Packaging

[_hackMD notes_](https://hackmd.io/z_Zq1QcVSwWlBSKBysNcZA)

#### Deploying

## Day Three, Four and Five

### Project

_Resource:_ [_FAC instructions_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/project.md)

Producing an autocomplete website/widget. [Our code](https://github.com/fac-13/vithAutocomplete) and [our website](https://vith-autocomplete.herokuapp.com/)

*   [Heroku Cheatsheet](https://hackmd.io/X3nL7fDwRRS3Yn8TIz5NVA)
*   Can use `startsWith()`
*   `querystring` (core Node module) is your friend when it comes to parsing/stringifying URL (particularly when spaces etc are converted to `%20`).
    *   use `const inputTwo = querystring.parse(input)['/search/?q'].toLowerCase().trim();`
*   You can use `<datalist>` (though not fully supported in all broswers), but gives you default dropdown functionality like arrow down etc:

    ```
    <input list="browsers">

    <datalist id="browsers">
        <option value="Internet Explorer">
        <option value="Firefox">
        <option value="Chrome">
        <option value="Opera">
        <option value="Safari">
    </datalist>
    ```

    *   You can also use `aria-activedescendant` and `Combobox` to create a dropdown

*   Use the `<form action="/team_name_url/" method="post">` (example [here](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/forms)) for input rather than sending a request on `enter` button `keyup` event (a bit of a hacky way that we did it - where there were bugs e.g. having the `preventDefault()` on keydown)

### Other snippets of code

*   I love this description of objects: contain properties that are **"key:value" pairs**.
*   DOM Manipulation:
    *   `textContent` removes all of the children nodes and replaces them with a single text node with the given value
    *   Should use `className` rather than adding and removing classes with `classList` (`className` replaces all existing classes with one or more new classes)
*   The [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) can take a third argument - where can e.g. do it once before removing the event listener.
    *   Should never add an event listener within a function (since can accidentally add too many)
