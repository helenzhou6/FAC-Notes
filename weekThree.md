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
### Flexbox
_Resources:_ [_Flexbox dice workshop_](https://github.com/smarthutza/flexbox-workshop) _-- NB look at the references inside workshop link and_ [_froggy game_](http://flexboxfroggy.com/)
* `align-content` determines the spacing between lines, while `align-items` determines how the items as a whole are aligned within the container. (`align-content` adjusts the space between the row/columns too VS `align-items` is each item or whole group of items). `align-item` is aligning the specific item within a group that has `align-items` applied (note it still takes up the original space - to reshuffle items can then use `order`).
* Adding `reverse` flips/mirrors what you expect: reverses the start and end points,
* To mirror diagonally/flip across x and y axis, since can't have `column-reverse` and `row-reverse` together, do `flex-wrap: wrap-reverse` and `flex-direction: column-reverse`.

![flexbox notes](https://i.imgur.com/wlz2RRc.jpg)


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

## Day Three
_Resource:_ [_solution to the waterfall challenge_](https://github.com/foundersandcoders/mc-waterfall-chaser/tree/master/solution) _and_ [_compose vs pipe_](https://medium.com/@dtipson/creating-an-es6ish-compose-in-javascript-ac580b95104a) _and_ [_composing functions_](http://blakeembrey.com/articles/2014/01/compose-functions-javascript/)

### The pipe and compose functions
* These functions allows you to take some functions and compose them into a single function. (Useful if you have a pattern of modular functions).
* For example a **pipe function**:
```
function addOne(val) {
  return val + 1;
}

function timesTwo(val) {
  return val * 2;
}

function addThree(val) {
  return val + 3;
}

var pipe = function() {
  var fns = Array.from(arguments);
  return function(val) {
    return fns.reduce(function(current, fn){
      return fn(current);
    }, val);
  };
};

console.log(
  pipe(addOne, timesTwo, addThree)(4)
);
```
* (NB: arguments of a function can be accessed using `arguments` and it is array-like (so you can use `Array.from`)
* In this example, `func` returns a function that effectively is: `addThree(timesTwo(addOne(val)))`. It obtains an array of all the arguments, then reduces it into a nested format of functions.
* Thus you can do `var flow = pipe(addOne, timesTwo, addThree)` to have a series of functions perform (`flow(4)`)

* **The compose function** is similar to **pipe** but it's run in reverse.
> You nest functions inside each other so when they are called with a final value, the result explodes outwards through each layer. It works right to left (inner to outer) to create a composition of all those functions (i.e. return a new function).
* E.g.:
    ```
    function compose () {
        var fns = arguments;

        return function (result) {
            for (var i = fns.length - 1; i > -1; i--) {
                result = fns[i].call(this, result);
            }

            return result;
        };
    };
    var number = compose(Math.round, parseFloat);
    ```
    * Where `.call` calls the function with a passed through `this` value (see [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call))

### The Waterfall function
>  * Like the compose function, **the waterfall function** runs an array of functions in series, each passing their result to the next function in the array (used if the passed in functions are asynchronous). Crucially, the waterfall function will not invoke the next function in the array until the current function being run has been returned.
> * The waterfall function takes three arguments:
>    * An initial argument, this is passed into the first async function in the waterfall.
>    * The second argument is an array of functions, each function takes two arguments. The first argument it uses to find a result and the second is a callback function.
>    * A final callback is given as the third argument. This is called after the waterfall has invoked all of the functions in the array.

```
function asyncAddOne(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number'){ return callBack(new Error('need a number!'), undefined) }
    else { return callBack(null, x + 1) }
  }, 200)
}

function asyncDouble(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number') { return callBack(new Error('need a number!')) }
    else { return callBack(null, x * 2) }
  }, 200)
}

function asyncTimesTen(x, callBack) {
  setTimeout(function() {
    if (typeof x !== 'number') { return callBack(new Error('need a number!')) }
    else { return callBack(null, x * 10) }
  }, 200)
}

function waterfall(args, tasks, cb) {
    if(tasks.length === 0){
      return cb(null, args)
    }
    tasks[0](args, function(err, arg){
        if (err){
          return cb(err)
        }
        return waterfall(arg, tasks.slice(1), cb);
    });
}

waterfall(3, [
  asyncAddOne,
  asyncDouble,
  asyncTimesTen
], function(error, result) {
  console.log('Test 1');
  if (error) {
    console.log('test failed, ' + error)
  }
  else if (result !== 80) {
    console.log('test failed, expected 80 but got', result)
  }
  else {
    console.log('Test 1 passed!')
  }
})

```
SO in this mass of confusing code:
* `task[0](args...)` - runs the first task in the array, and then the `tasks.slice(1)` removes the first function in the argument array (so that the next time it runs `task[0]` is the next function and there is one less function in the array)
* Each time `waterfall()` is being called again (so it loops until there are no more tasks) but with the new list of tasks (with one less function in it)
* Nothing is being returned here, only the `cb` section will be run at the very end (when there are no more tasks in the array. `cb` is a test to see if result is 80.
* Math is calculated immediately! So when `3` is passed in as `args` (i.e. it becomes the second argument `x = 3`), `x + 1` is run immediately - now making `args = 4` essentially.
* `cb` - the final call back is simply being passed through each time, and it is only envoked at the end - `cb(null, args)` - where null refers to no errors and `args` is the final number `80`.
* The `null` needs to be passed in as a first argument (e.g. `callBack(null, x * 2)` and `cb(null, args)`) because it refers to there not being an error. At the running of each task, if `x` is not a number being passed in, instead it returns `callBack(new Error('need a number!'), undefined)` (the second argument would be undefined by default), so that when `if (err) {return cb(err)}` runs - it would stop the waterfall, run the `cb` test instead that would return an error message using the first argument (`'need a number!'`) passed in. But if `x` is always a number, and so `err = null` (since `null` is being passed in when `x` is a number), the `if(err)` doesn't run in this example.
* `new Error()` means the error has some other features, but it could've just been the string `'need a number!'`.
* With `callBack(null, x * 10)` essentially the relevant code becomes: 
    ```
    function(null, x * 10){
            if (err != null){
            return cb(er)
            }
            return waterfall(arg, tasks.slice(1), cb);
        }
    ```
* Essentially it performs `asyncAddOne` -> `asyncDouble` -> `asyncTimesTen` (the maths is done in the argument section)/ `(((3 + 1) * 2) * 10 = 80` where at the very end (when `tasks.length===0`), `cb(null, args)` runs -- meaning `function(null, arg) {console.log('Test 1')...}`, where by this point `arg` is `80` (due to immediate maths as mentioned before), and there are no errors (`null` passed in as the first argument).