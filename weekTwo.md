
# Week Two
_Resource:_ [_week two schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-2)  _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-2/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-2/resources.md).

It's Testing themed week!

## Day One
### NPM
_Resource:_ [_introduction to NPM_](https://github.com/foundersandcoders/npm-introduction)

NPM has libraries of ‘plugins’ to use (e.g. validator of strings for forms).
Are downloaded specific to repo (try to ignore npm -global)

#### The ```package.json``` file
* Once initiated using ```npm init```, the ```package.json``` file is automatically generated specific to that repo.
* Packages are recorded, that are either 'dependencies' if they are needed for the browser to run your project/need to be installed on the server delivering your app (using ```npm install <package-name> --save```) and 'dev-dependencies' that are packages related to the project, but the brower does not need to know about (e.g. testing frameworks, using ```npm install <package-name> --save-dev```).
 * ```package.json``` should be pushed onto GitHub so that anyone who wants to work on your project can download it, and then be able to download all the NPMs they need to work on the project quickly.
 * You can edit the ```package.json``` file, e.g.:
 ```
 "scripts": {
    "test": "nodemon romanizer.test.js | tap-spec"
  },
```
Where ```tap-spec``` NPM makes the output of ```test.js``` prettier, and ```nodemon``` NPM runs the tests every time there is a change in the file.

### Testing
_Resources:_ [_introduction to testing and TDD methodology_](https://github.com/foundersandcoders/testing-tdd-intro), [_the fizzbuzz challenge using TDD method_](https://github.com/foundersandcoders/fizzbuzz), [_and the romanizer code along using same thing_](https://github.com/foundersandcoders/roman-numeral-tdd-codealong). _and_ [_the team presentation_](https://docs.google.com/presentation/d/10OooEvZdnoZFAidz_Hmv7mjy4qL_lCP5aL0H85V2NzU/edit#slide=id.g34b4b49fd0_0_7)

* **Unit testing** - one unit/function is being tested.
* **TDD methodology**: Write a failing test (to specify what you want the outcome of your function to do), then a passing test (write the function to make the test pass, usually in the simpliest way to make it pass) and then refactor (you should be able to see a pattern in your code)
* Code used in testing:
  * **deepequals** testing checking inside an object (as stated [here](https://api.qunitjs.com/assert/deepEqual))
  * Can use `t.pass()` and `t.fail()` within your tests, that can test for other things other than whether actual equals expected.
* Fantastic talk on testing (including testing doubles) by Rabea. Slides [here](http://rabea.co.uk/spies-fakes-and-friends/) and [rock/paper/scissors repo](https://github.com/RabeaGleissner/rock-paper-scissors-js). Also [a blog post on test doubles](http://rabea.co.uk/blog/apprenticeship/post-101) by her


#### Code coverage
_Resource:_ [_The istanbul NPM_](https://github.com/dwyl/learn-tape#bonus-level) and [_team presentation_](https://hackmd.io/qBSjrsOSQJaIfHmXuQgj0A)

* Code coverage is a measure used to describe the degree to which the source code of a program is tested by a particular test suite. It calls your functions and lets you know exactly which lines of code you have written are "covered" by your tests (i.e. helps you check if there is "dead", "un-used" or just "un-tested" code) We use [istanbul](https://github.com/dwyl/learn-istanbul) -- now [nyc](https://github.com/istanbuljs/nyc) for code coverage.
* **Test coverage** team presentation [notes](https://hackmd.io/qBSjrsOSQJaIfHmXuQgj0A).
  > Coverage criteria:
  > * **Function coverage** – Has each function (or subroutine) in the program been called?
  > * **Statement coverage** – Has each statement in the program been executed?
  > * **Branch coverage** – Has each branch of each control structure (such as in if and case statements) been executed? For example, given an if statement, have both the true and false branches been executed? Another way of saying this is, has every edge in the program been executed?
  > * **Condition coverage** (or predicate coverage) – Has each Boolean sub-expression evaluated both to true and false?

  > If you have a bad score, could be:
  > * If used TDD, then indicates that there is redundant code in the program
  > * If not used TDD then your tests are crap: indicates there will be functionality in the program that is not being tested for


  > Use ```"test": "nyc tape <test.js filename> | tap-spec "``` - to make it look pretty on terminal

### Other snippets learnt
* In ```./index.js```: write ```module.exports = {fizzbuzz, display};``` to export the ```fizzbuzz``` and ```display``` functions to other files. In the other files put ```var {fizzbuzz, display} = require('./index.js’);``` to import both functions. NB: the code needs to be wrapped into a closure - see [here](http://www.matteoagosti.com/blog/2013/02/24/writing-javascript-modules-for-both-browser-and-node)
  * Wrapping in a closure makes it an Immediatly invoked function expression (IIFE) - definition [here](https://codeburst.io/javascript-what-the-heck-is-an-immediately-invoked-function-expression-a0ed32b66c18). So with the parenthesis, the function is invoked, whilst without it, the function definition is returned instead.
  * More IIFEs [here](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) and [here](https://toddmotto.com/what-function-window-document-undefined-iife-really-means/). IIFEs are immediately called at runtime and only run once, so good for privacy.
* ```switch``` statement: to evaluate expressions need to put ```swithc(true)```. Not need to break if you want to continue with function.

## Day Two

### DOM Manipulation
_Resource:_ [_DOM Manipulation Challenge_](https://github.com/foundersandcoders/DOM-manipulation-Challenge), _and_ [_the team presentation_](https://hackmd.io/tiUez2TjSrScUSQye8T1hA?both)

* DOM = Document Object Model. Any html elements = DOM elements in the browser. These DOM elements have [properties that can be accessed](https://developer.mozilla.org/en-US/docs/Web/API/Element#Properties) (e.g. ```Element.innerHTML```)
* Accessing DOM elements using ```document.getElementById("myId")``` will return the DOM element, similarly ```document.querySelector(myCssSelector)``` returns the first element matching the CSS selector (where any element you can target for CSS styling e.g. ```input[name = 'firstName']``` & child selectors etc. can be used)

  * Whilst ```document.getElementsByClassName("myClass")``` and ```document.querySelectorAll(myCssSelector)``` returns an [array-like object](http://www.nfriedly.com/techblog/2009/06/advanced-javascript-objects-arrays-and-array-like-objects/) of all the matching elements = a **NodeList** (differences listed [here](https://hackmd.io/tiUez2TjSrScUSQye8T1hA?view)).


* DOM Manipulation team presentation [Notes](https://hackmd.io/tiUez2TjSrScUSQye8T1hA?both)
  * Create (```createElement()```) and add element to DOM, then create content (```createTextNode()```) inside new element and insert it into the new element (```appendChild()``` / ````replaceChild()``` / ```insertBefore()```)

### Pure functions for easy testing
_Resource:_ [_Workshop for writing testable code_](https://github.com/foundersandcoders/ws-pure-functions-easy-testing)

* A **pure function** will always return the same result when run with the same arguments. Usually a single function that is small and predictable (so can easily write a test for it)
  * It has no **side effects**: shouldn't do anything outside of calculating the return value. So it shouldn't use change a global variable. 
  * Changing a global variable by accident is common because they are often a **mutable object** whose state can be modified after it is created like objects and arrays (VS **immutable object** that can't like numbers are strings). So for mutable values, updating the state applies accross all _references_ to that variable. (Example [here](https://benmccormick.org/2016/06/04/what-are-mutable-and-immutable-data-structures-2/))
  * To combat this, you need to essentially copy a global array/object before manipulating it within the function

    _Duplicating an array:_
    ```
    for (var i = 0; i < array.length; i++) {
      arrayCopy[i] = array[i] ;
    }
    ```
    OR
    ```
    arrayCopy = array.map(function(x){
      return x;
    });
    ```
    ...but best documented method is:
    ```arrayCopy = array.slice(0);```

    _Duplicating an object:_
    ```
      var objectKeysArray = Object.keys(object);
      var objectCopy = {};
      objectKeysArray.forEach(function(key){
        objectCopy[key] = object[key];
      });
    ```
    Where it creates an array of the object keys. Then for each key, adds to the new empty array the object[key] and the old object value associated.

    OR
    ```
    var objectKeysArray = Object.keys(object);
    var objectCopy = objectKeysArray.reduce    (function(accumulator, key){
        accumulator[key] = object[key];
        return accumulator;
    }, {});
    ```
    OR
    ```
    var objectCopy = JSON.parse(JSON.stringify((object)));
    ```
    -> That makes a **deep copy** rather than a **shallow copy** of the object (refer to [here](https://we-are.bookmyshow.com/understanding-deep-and-shallow-copy-in-javascript-13438bad941c))

    [The solution to the DOM Manipulation challenge](https://github.com/foundersandcoders/DOM-manipulation-Challenge/blob/solutions/exercise/exercise.js) involves using reduce instead.

### Other snippets learnt
* Remember the [```for...in```](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) statement can be used to loop over properties of an object.
* Every **function** in JS is an object (a collection of ```key:value``` pairs). Within the scope of an object, a function is referred to as a **method** of that object.
* NB: be wary of using **mutating methods** like ```Array.prototype.push```, ```Array.prototye.pop```, or ```Array.prototype.sort``` that alter the array VS methods that generate new arrays like ```Array.prototype.map```, ```Array.prototype.filter```, or ```Array.prototype.concat```
    

### Research topics
_Resource:_ [_the topics_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-2/research-afternoon.md)

* **Technical Spike** [slides](https://docs.google.com/presentation/d/11Hs5fFNEfRyRB96gkG4SoJbL41Qo0_eAWEvlQFb_3XE/edit#slide=id.g346c003bf1_0_55) and [notes](https://hackmd.io/UaJt9A-RSt66Zgy936GiHg)
* (The info from other presentation is integrated into the notes above)

### Event loops
_Resource:_ [_incredible video: What is an event loop anyway_](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=5s)
* JS is single thread. But some things seem to run simultaneously: if you set a timeout, then the **web API** deals with the countdown, and once countdown complete puts the task in the **Callback queue**. Once the **call stack** is empty, then the **event loop** puts the task into the **call stack** and it exceutes.
* Put code in [here](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) to visualise it.
* set ```timeout``` to 0 if want a function to run only when the stack is empty.
* NB the timeout is not the time until exceution, rather it would be at least that amount of time until exceution since the timer will run, and then it will be put into the **call stack** where there may be other things in the **call stack** delaying the exceution even more before it is run.
* Things that need to run and can take up the **call stack** includes rendering - so best is to have breaks in the **call stack** to allow for rendering in between.

## Day Three
### Callbacks
_Resource:_ [_the traffic light challenge_](https://github.com/foundersandcoders/morning-challenge-traffic-lights) _and_ [_JS callback article_](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/) _and_ [_higher order functions video_](https://www.youtube.com/watch?v=BMUiFMZr7vk)

> * **Callback** is any executable code that is passed as an argument to other code, which is expected to call back (execute) the argument at a given time (usually after waiting for the outter function to complete first).
> * **Callback functions** are derived from a programming paradigm known as **functional programming**. At a fundamental level, functional programming specifies the use of functions as arguments.
> * Because functions are **first-class objects**, we can pass a function as an argument in another function and later execute that passed-in function or even return it to be executed later. This is the essence of using callback functions in JavaScript. 

```
function blinkLight(element, colour) {
  return function(callback) {
    element.classList.add(colour);
    setTimeout(function() {
      element.classList.remove(colour);
      if (callback) {
        callback();
      }
    }, time);
  };
}

var red = blinkLight(get('light-top'), 'red');

function light() {
  green(function() {
    yellow(function() {
      time = 3000;
      red(function() {
        time = 1000;
        red();
        yellow(light);        
      });
    });
  });
}
light();

```
In [the challenge](https://github.com/foundersandcoders/morning-challenge-traffic-lights), ```red``` becomes a function that takes the argument ```callback```. It executes some code, and then if a function has been passed in (which the if statement checks for), the function is then called. In this case the green light blinks first before the other function is executed (goes from the outter function to the inner function).
* NB: In order to pass in a function as an argument, it needs to be passed in as an anonymous function, since putting a parentheses after the function name - ```function()``` - means the function would be executed immediately instead of passed in. So then ```red(yellow(green))``` wouldn't work since it would try running ```red()``` and ```yellow()``` simulatenously/or do something weird.

#### Higher order functions
[This video](https://www.youtube.com/watch?v=BMUiFMZr7vk) tells you about **higher order functions** - a function that does at least one of the following: takes one or more functions as arguments, returns a function as its result.
  ```
  var animals = [
    { name: 'Helen', species: 'human'},
    { name: 'Doggy', species: 'dog'}
  ]
  var isDog = function(animal) {
    return animal.species === 'dog';
  }
  var dogs = animals.filter(isDog);
  ```
  * e.g. the `filter` function takes an anonymous function (called a **callback function**). It loops through each item, and each item is passed into the callback function. The callback function returns true or false (to see if should go into new array or not) & then after the filter function would return a new array. Since functions are variables (that can be passed in etc) - this means you can make smaller functions (in this case `isDog()` that can be reused and tested easily.


## Day Three, Four & Five
_Resource:_ [_the project_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-2/project) _and_ [_some slides_](http://slides.com/rachaelcodes/todoproject#/29)

### Snippets learnt
* `li.childNodes.item(0)` == `li.childNodes[0]`
* Display flex: to have one item in a row on the left and the rest on the right, can `display: flex` and then on the left item put `margin-right: auto`
* > **Function declarations** are hoisted to the top of other code. **Function expressions** aren’t hoisted, which allows them to retain a copy of the local variables from the scope where they were defined. See [here](https://www.sitepoint.com/function-expressions-vs-declarations/)
* [Event bubbling](https://www.sitepoint.com/event-bubbling-javascript/).
  > * During propagation, it is possible for a listener to know if an event bubbles by reading the `.bubbles ` Boolean property of the event object. `e.target` = event target, `e.currentTarget` = node references by `this`, and can do `e.eventPhase`.
  * Threephases - **The event capture phase** (only capturer listeners are called) -> **the event target phase** (all listeners on the event target are invoked) -> **the event bubbling phase** (non-capturers)
  > * The event propagation can be stopped in any listener by invoking the `e.stopPropagation()` method of the event object. This means that all the listeners registered on the nodes on the propagation path that follow the current target will not be called. Instead, all the other remaining listeners attached on the current target will still receive the event.
  > * `e.stopImmediatePropagation()` throws the brakes on straight away, preventing even the siblings of the current listener from receiving the event.
  > * Some events are associated with a default action that the browser executes at the end of the propagation. To avoid, use event cancellation: `e.preventDefault`
* [Local storage video](https://www.youtube.com/watch?v=YL1F4dCUlLc&t=0s&list=PLu8EoSxDXHP6CGK4YVJhL_VWetA865GOH&index=15). E.g. in the to do app project to get the item: `var state = JSON.parse(localStorage.getItem('state')) || [];` and to add the item into local storage: `localStorage.setItem('state', JSON.stringify(state));`