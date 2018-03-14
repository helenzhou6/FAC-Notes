# Week Three
_Resource:_ [_week three schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-3)  _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-3/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-3/resources.md).

It's API themed week!

## Day One
### API
_Resources:_
* _Introduction to Request and response pattern, HTTP, XMLHttpRequests, Asynchronous code and fixing a broken API request excercise [here]_(https://github.com/foundersandcoders/api-workshop)
* [_XHR workshop_](https://github.com/foundersandcoders/xhr-workshop) - HTTP GET request to the Giphy API. 
* [_API workshop using GitHub API_](https://github.com/emilyb7/workshop-APIs)

### HTTP
* > The Hypertext Transfer Protocol (HTTP) is the mechanism through which data is requested and provided on the World Wide Web.
* URL:

    ![URL](https://github.com/foundersandcoders/api-workshop/raw/master/url.png)

    #### HTTP Methods
    * **GET** : 'gets' resources, doesn't modify them. Sent as part of URL.
    * **POST** : sends data to the server. Not displayed in URL
        * > The data values are sent in the body of the request, in the format that the Content-Type header specifies. This way the server knows how to decode the information.
        * > A Content-Length header is also required so that the server knows when it has received all of the information (a large amount of data might be split across multiple requests).
    * **PUT** Places data at exact path specified in request (usually to overwrite something that already exists)
        * > with POST the path specified in the request will handle the data and the server can put the data where it likes. A PUT request is basically a request to update the resource at the specified path with the data provided.
    * **DELETE** the client requests the server to delete a resource at the path specified.
* > When sending an API request, we use a URL to identify the specific server where we want to send the request (kind of like the address on an envelope) and the rest of the url specifies what it is we want the server to send back. These are called parameters or queries.
* > DNS uses the resource name (e.g. www.google.com) to look up the server to which the request is addressed. Anything beyond the resource name is part of the query or filepath the specifies exactly what data or resources are being requested (including an access token for increased rate limit)

#### XMLHttpRequest
* `var xhr = new XMLHttpRequest();` - using the object constructor, `xhr` is a new instance of the object `XMLHttpRequest()` that has all associated methods.
* There can change properties of the object, including `xhr.open` and `xhr.onreadystatechange`
* `xhr.onreadystatechange` - is an event listener for every time the ready state changes (keeps track of the request from **readyState** 0 to 4, and the response back (i.e. the **HTTP status code** received)). So `(xhr.readyState == 4 && xhr.status == 200)` = complete this function if request completed and a [status code](https://github.com/foundersandcoders/api-workshop/blob/master/02-http.md#http-status-codes) has been received and is 'OK' (not an error like the status code 404.
* Finally, use `xhr.send()` to then send the HTTP request.
* For example: 
```
function fetch (url, callback) {
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4 && xhr.status == 200) {
            var response = JSON.parse(xhr.responseText);
            return callback(response);
        }
    };
    xhr.open("GET", url, true);
    xhr.send();
});
```
* `xhr.responseText` = the response, but it needs to be parsed using: `JSON.parse(xhr.responseText)` so that the response can be accessed like a JS object.

    * Then to have an event listener function:
    ```
    function addListener (selector, eventName, callback) {
        document.querySelector(selector).addEventListener(eventName, callback);
    }
    addListener('#bar', 'click', function (event) {
        var element = document.querySelector('#pam');
        var url = 'https://lulz.org/search?query=' + element.value;

        fetch(url, function (response) {
            // ... do something with the response
        });
    });
    ```

## Day Two

### Software Architecture
_Resource_: [_workshop: software architecture design_](https://github.com/foundersandcoders/Workshop-Software-Architecture-Design) 

* **Architecture** is the general term for the overall structure of a software system /  a conceptual representation of how an app is structured and how all the parts of it interact
* **Software Architecture** is the structure or structures of a system comprising of software elements and the relationships among them. Usually a box (elements/parts of the system) and line (the interactions between the parts) drawing of the system - intended to solve the problems articulated by the spec.
    * [This guide](For a comprehensive overview of different Architectural Styles and Patterns that people have developed) has a comprehensive overview of different Architectural Styles and Patterns that people have developed.
* Key principles (detail version [here](https://msdn.microsoft.com/en-us/library/ee658124.aspx#KeyDesignPrinciples))
    * How can you separate your concerns? i.e. keep unrelated things in separate sections
    * Which aspects of your app are generic, and which are specific to your requirements?
    * How can you make it easy to test?
    * How can you make it easy to change?
    * Can you anticipate changes that are likely to occur?


## Software Design

### Fundamental concepts of JS
_Resource_: [_workshop: software design_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-1)

#### Functions as First Class Objects
_Resource_: [_FAC article_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-1/first-class-functions)
* (See [callback section of weektwo](https://github.com/helenzhou6/FAC-Notes/blob/master/weekTwo.md#callbacks))

#### Closures and Scope
_Resource_: [_FAC article_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-1/closures-and-scope)
* **Scope** = refers to what the execution context of a particular piece of code is.
    * The **global scope** is the default scope for all JS in the browser. Each function has access to its own local scope, and also the scope of any functions that encloses it (relationship is applied recursively).
    * **Block scope** - the block is the closest enclosing `{}`. Variables declared with `var` are available within the function in which they're defined (and all sub-functions) - **function-scoped** VS variables defined with `let` and `const` are only available within the block they're defined in - **block-scoped**. (Also `const` cannot be reassigned). E.g.:
    ```
    if (true) {
        if (true) {
            var t = 5;
        }
    }
    console.log(t);
    ```
   * The `if` statement will always run. This works but not if `var` was `let` or `const` - since `var` does not 'care' about blocks (and thus 'escapes' from it), rather if it is inside a function, then it cannot 'escape'/be accessed outside of it.
   * That's why IIFEs are useful: wrapping a 'module' inside a fuction means that variables inside cannot be accessed globally.

* A **closure**  is a technique for creating scopes that persist even after the function they are defined by has returned. 
```

function createIncrementer () {
  // local scope (scope B)
  var a = 1;

  return function addOneTo (n) {
    // local scope (scope C)
    return a + n;
  }
}

var addOneTo = createIncrementer();

```
> Now the value of `addOneTo` is the function returned from `createIncrementer`.
This value exists in the global scope. The body of `addOneTo` references variable `a` defined in `scope B`, therefore this variable remains available to `addOneTo` even after `createIncrementer` returns.
* I.e. There is a function within another function that returns something - but it still has access to/can use the variable declared within the outter function.

#### Abstraction with Functions
_Resource_: [_FAC article_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-1/abstraction-with-functions)
* **Abstraction** is the process by which we allow the developer to work at a higher conceptual level by limiting the amount of low-level complexity we expose them to. E.g. using `map` or `reduce`. (Thus hiding unnecessary complexity)

### Intermediate level design patterns
_Resource_: [_workshop: software design stage 2_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-2)

#### The Module Pattern
_Resource_: [_FAC article_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-2/module-pattern) _and_ [_mastering the module pattern_](https://toddmotto.com/mastering-the-module-pattern/) _and_ [_the module pattern in depth_](https://github.com/foundersandcoders/ws-software-design-js/tree/master/stage-2/module-pattern)

* A **module** - a self-contained bundle of code.
* Any variable or function defined in the global scope of a script file is also in the global scope of any script loaded after it. To avoid: use the **module pattern**.
    * > It exploits the technique of creating a closure to create variables which can by used by the module, but not directly by any of the code outside it.
    * So can wrap multiple modules in a function (e.g. `function createCalculator ()`) or use IIFEs:
    ```
    var Calculator = (function createCalculator () {
        // ... module code
    })();
    Calculator.add(3);
    Calculator.read();
    ```
* The module pattern may be appropriate if you wish to create an abstraction that has the ability to do the following:
    * hide complex implementations
    * present a simpler interface to the developer than the underlying code
    * maintain an internal private state
    * keep variables which are irrelevant to the rest of the code within the private scope of the module
(VS If the abstraction you wish to create has no internal state)