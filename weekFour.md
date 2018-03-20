# Week Four
_Resource:_ [_week Four schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-4)  _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-4/resources.md).

It's Node themed week!

## Day One
### Node
_Resource:_ [_Node Intro workshop_](https://github.com/foundersandcoders/Node-Intro-Workshop)

* **Node.js** is a JavaScript runtime (helps you write JS programs) built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking (asynchronous), single-threaded I/O model (it talks to networks, file systems or other I/O (input/output, reading/writing) sources.) It is mainly used for web servers or file system utilities.
* Node handles I/O with: callbacks, events, streams and modules.

#### Server
* A server is a computer program that receives requests from other programs, the client, and sends back a response, e.g. to share data, information or hardware and software resources. E.g. the website address asks server for files to display the website.
![Image of what server is](https://camo.githubusercontent.com/6d63917fa946de22ba4e84fa4adbb6000d73352a/68747470733a2f2f66696c65732e6769747465722e696d2f6865726f6e323031342f4669694b2f7365727665722e706e67)

    * Handle manipulation of data in the database
    * File manipulation
    * Authentication
    * Secret Logic
    * Client side sends requests to a server which sends back data to the front end to be displayed
* Usually your server is faster than your client's machine
* **Front-end** Client-side JavaScript runs in the visitor's browser (in the `public` folder), whereas **back-end** server-side code runs on a website's web server and deals with logic (in the `src` folder)

#### Modules
* Small packaged programs you can integrate with the other programs you are writing. Three types:
    * **core modules** - the Node core library comes installed with Node automatically. Includes `fs`, `http`(helps us process our server requests and responses), `path`, `querystring` and `url`.
    * **third-party modules** or **packages** - thousands of open-source modules -  Node.js' package ecosystem, `npm`, has a large ecosystem of open source libraries 
    * Access by 'requiring' them: `var http = require('http');`

* **Node Package Manager (npm)** is a tool, installed with Node, for managing Node's ecosystem of modules in your projects.
    * Used to install tools, packages as dependencies for our projects, and also publish our own packages.
    * npm comes with its own command-line interface you can use in your terminal while within your relevant project folder (e.g. `npm search`, `npm install x --save-dev` and `npm init`)
    * Steps after running `npm init` -  that installs a Node virtual environment - [here](https://github.com/shannonjensen/node-workshop/blob/master/step01.md) (i.e. dealing with the `package.json` file - that contains meta-information about your project).

### Building a Content Management System (CMS)
_Resource:_ [_Node Girls/Shannon edition workshop_](https://github.com/shannonjensen/node-workshop)

#### Setting up the server
* In file `src/server.js`
* Use `http.createServer()` to build the server
* `server.listen(3000, function(){}` - taking **port** number 3000 and the callback function. A **port** is needed for the server to listen to. Can think of it as creating a mail slot for direct incoming requests (from URL requests).
* `node server.js` on command line to turn on server (can use `nodemon` to refresh server)

#### Handling GET requests
* When a request reaches the server, we need a way of responding to it. The `router` function receives requests and routes them to any appropriate handler functions (which handle the requests) or handles the requests directly (like giving the server a pre-paid addressed envelope)
```
function router (request, response) {
  response.writeHead(200, {"Content-Type": "text/html"});
  response.write(message); //response body
  response.end(); // finish response
}
```
* Can use one of the methods of the `response` object ^ see above block code.
    * E.g. `response.write()`. Every response has a header that contains information (argument: status code and header object (if not listed server will try and guess) - `response.writeHead(200, {"Content-Type": "text/html"});`
* `var server = http.createServer(router);` - pass in the `router` function
* **endpoint** is the part of the URL that comes after `/`. Inside `router` function: `var endpoint = request.url;` (can check what type of HTTP method request was using `request.method`)
* To send any file from the server use core module `fs` - file system - that allows you to read and write to and from your hard drive. First server needs to read it using `fs` method: `fs.readFile('path to the file', callback);`
    * In router function:
    ```
      if (endpoint === "/") {
        response.writeHead(200, {"Content-Type": "text/html"});

        fs.readFile(__dirname + '/public/index.html', function(error, file) {
        if (error) {
            console.log(error);
            return;
        }

        response.end(file);
        });
    }
    ```
    * `__dirname` is a Node global object that gives you a path to current working directory. We can use this instead of writing the whole path.
    * use `..` to go up a directory
    * `.` is look in current folder
    * Can do `__dirname + '/../public/index.html'` or `path.join(__dirname, '..', 'public', 'index.html')`
* A webpage can have several different requests to the server: E.g. One is the original browser request, another is a request sent by `<link>`, and the last one is a request sent by `<img>`
    * Can write a generic route that is able to deal with lots of different assets (in `handler.js` file).
    ```
    const handleHomeRoute=(request,response)=>{
        fs.readFile(filePath, (error,file)=>{
            if (error) {
                response.writeHead(500,'Content-Type: text/html')
                response.end("<h1> sorry, we've had a problem on our end</h1>");
            } else {
                response.writeHead(200,'Content-Type: text/html')
                response.end(file);
            }
        });
    }

    const handlePublic=(request,response,url)=>{
        const extension = url.split('.')[1];
        console.log('split 0:', url.split('.')[0], 'extension:', extension);
        const extensionType = {
            html: 'text/html',
            css: 'text/css',
            js: 'application/javascript',
            ico: 'image/x-icon'
        };
        const filePath = path.join(__dirname,'..',url);
        fs.readFile(filePath,(error,file)=>{
            if (error) {
                response.writeHead(404, 'Content-Type: text/html');
                response.end("<h1>404 file not found</h1>")
            } else {
                response.writeHead(200, `Content-Type: ${extensionType[extension]}`);
                response.end(file);
            }
        })
    }
    ```
    * `handleHomeRoute` - when `endpoint === '/'` (gets the `index.html` file) & `handlePublic` - for other assets (uses the file name to find the file within the `public` folder and the file extension to pick the right `Content-Type`)
* It's good practice to separate routes and handlers into their own files. For core module do `const x = require('x');` and for modules in own code write `const x = require('[path e.g. ./x]');` for dependencies, and to export modules do `module.exports = {x, x};`
    * In `router.js` file
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
    * In `server.js` file:
    ```
    const http = require('http');
    const router = require('./router');
    const port = 4000;
    const server = http.createServer(router);
    server.listen(port);
    console.log(`server up and running on localhost: ${port}`);
    ```

#### Handling POST requests
* Using the `POST` http request method
* `<form action="/create-post" method="POST">`
-  when e.g. submitting a `form`.
    * The `action` attribute (in the HTML) is where the form data will be sent and the `name` attribute is used later to reference the data.
    * When you hit Send, the form will send a `POST` request to the server, using the `/create-post` endpoint.
##### Receiving form data on the server
* Data doesn't come through the server in one go; it flows to the server in a stream. Need to collect a bucket of water in the server
* Listen for the 'data' event and when ot starts to arrive, collect the chunks of data into the `allTheData` variable.
    ```
    request.on('data', function (chunkOfData) {
        allTheData += chunkOfData;
    });
    ```
* Once all the data has come through, listen to the 'end' event:
    ```
    request.on('end', function () {
        var convertedData = querystring.parse(allTheData);
        console.log(allTheData);
        response.end(); // need this to finish
    });
    ```
    * NB Need `querystring.parse()` to convert the `allTheData` **query strings** (how html forms send data over the internet) to an JS object (to use it)
    * To prevent going to the page `/create-post` after submitting, can use the `endpoint` if statement to redirect to an endpoint, and use `response.writeHead(200, {"Location": "/"});` and use the redirect [status code](https://httpstatuses.com/)