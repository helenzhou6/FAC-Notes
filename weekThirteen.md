# Week Thirteen

_Resource:_ [_week thirteen schedule and learning outcomes_](https://github.com/foundersandcoders/react-week)

It's React week!

## Day One

### ES6 Classes: object-oriented structures
_Resources:_ [_ES6 workshop_](https://github.com/oliverjam/es6-class-intro)

* NB: JS uses prototype --> will look at itself and see if it's there, then go up the next parent prototype etc and then carry on
* Previously you had to rely on **prototypical inheritance** with constructor functions.
Any new instance of `Hero` has access to whatever we put on its prototype.
```js
function Character() {
  this.health = 100;
}

Character.prototype.getHealth = function() {
  return this.health;
};

Character.prototype.takeDamage = function(damage) {
  this.health -= damage;
};

function Hero(name) {
// calling Character and this is set to Hero - to be able 
  Character.call(this);
  this.name = name;
}

Hero.prototype = Object.create(Character.prototype);

Hero.prototype.getName = function () {
  return this.name;
};
```

* > The `constructor` method (ES6 class syntax) is the equivalent of the defining function in the first example. It takes whatever arguments you call the class with when you instantiate it with the `new` keyword.
>
>  * Classes make it easy to inherit functionality from other classes. We can use the `extends` keyword to access properties on the base class. `super` refers to the base class you're extending. So `super('dog')` is like calling `new Animal('dog')`, only from within our new class.


```js

class Character {
  // can put arguments in constructor since can't in line 13
  constructor() {
    // this needs to be binded - arguments only available in constructor
    this.health = 100;
  }
  // method on the class - can use ES6 to define the function within the class
  getHealth() {
    return this.health;
  }
  takeDamage(damage) {
    this.health -= damage;
  }
}

class Hero extends Character {
  constructor(name) {
    // super calls the constructor of Character - and binds the 'this' to the context of 'Hero' as well as referencing the extending (Character)
    // don't always need a constructor - only call it if constructor
    super();
    this.name = name;
    this.health = 200;
  }
  getName() {
    return this.name;
  }
}
```

#### Exports
* `export default [const name]` usually when only export one thing, then `import a from './a'`
  * Can use any name since effectively creating a new variable and assigning the default-export to it.
* **Named exports** for multiple things: `export { a, b }` or `export const a = 1;`, then `import { a, b } from './a'`. Name needs to match.
* ES Modules are not dynamic so can't import things conditionally & can't use a variable to import path.
```js
const myVar = 'a';

const a = require(`./${myVar}`); // would work
import a from `./${myVar}`; // would not

const a = myVar ? require('./a') : require('something'); // would work
const a = myVar ? import('./a') : import './something'; //would not
```

### Why use React
_Resource:_ [_Why use react workshop_](https://github.com/foundersandcoders/mc-react-solves-what-now) _and_ [_talk on front end dev_](https://hackmd.io/jO3RJSATQOWURPMxDCKpmQ#/1)

* Most front end frameworks renders your view logic based on data changes.
* They store all the application state in JS (a **single source of truth**) Concept: UI as a function of the state -- if pass in state, UI is based on the state. Single source of truth - one single big state object that has all information on state in JS (not relying on getting data from the DOM)
* One of the benefits of react can see the difference between the old DOM and new DOM and will only re-render what is different - **virtual DOM** feature (React works with a big tree of objects known as the "virtual DOM".)

* **Server-side-rendered (SSR) application**
  * Templating with e.g. Handlebars. Server renders HTML and sends to client.
  * Client-side JS just for interactivity (modals etc)
* **Single-page applications (SPAs)**
  * App logic runs on client (server just provides JSON), initial HTML is empty `<div>`
* **SSR SPA**
  * Run your application JS on the (Node) server to render an initial HTML view.
  * Static HTML that “boots up” into an SPA once the JS loads.

### React workshop
_Resource:_ [_the workshop_](https://github.com/oliverjam/intro-react-workshop) _and_ [_stopwatch workshop_](https://github.com/oliverjam/intro-react-workshop/tree/master/workshop-top-notch-stopwatch)

```html
   <!-- These scripts will make React and ReactDOM globally available. It's just a JS library-->
    <script src="./../assets/react@16.0.0/umd/react.development.js"></script>
    <script src="./../assets/react-dom@16.0.0/umd/react-dom.development.js"></script>
    <!-- This will let us write JSX and ES6 in a script tag with a special type of "text/babel" - JS is 'transpiled' -->
    <script src="./../assets/babel-standalone@6.26.0/babel.js"></script>

```

* React has a `createElement` function (first argument: type of element, second argument: attributes, third+ arguments: children):
  ```js
  const title = React.createElement('h1', { className: 'main-title' }, 'Hello world!', 'This will be another text node');
  ```
  * `title` is not a DOM element, it's an object. Therefore need to `render` the object onto the page using `ReactDOM.render(title, root);` (only do this once)

#### JSX
* React apps usually use a non-JavaScript syntax known as JSX (similar to HTML) to create elements.
```jsx
const title = <h1 className="main-title"><span>Hello world!</span></h1>;
```
  * Need a transpiler, and also can't use reserved words like `class` so pretty much all DOM attributes are camelCased (`className`). 

### Element vs component
* **Element** is an object describing what you want in the DOM. A **component** accepts inputs (called **props** that were the attributes passed in) and return React elements (like a function). E.g. `const Title = props => <h1>Hello {props.name}</h1>;` and then to render: `ReactDOM.render(<Title name="Mum" />, root);`
  * > React components should act like pure functions with respect to their props. Don't try to change the props from within the component. Props should only get updated by a component higher up the tree passing new props in (which is equivalent to calling the component function again with new arguments).
  * >  React treats lowercase tag names as HTML and capitalised tags as components, so be sure to always capitalise your component variables.
* Also can embed JS expression in JSX:
  ```jsx
  const Title = props => <h1>Hello {props.name || 'world'}</h1>
  ReactDOM.render(<Title name="Mum" />, root);
  // renders <h1>Hello Mum!</h1>
  ```
* Children is a normal prop with a couple of extra privileges.

  ```jsx
  React.createElement('h1', {}, 'Hello world!')
  // the same as:
  React.createElement('h1', { children: 'Hello world!'})
  // the same as:
  const Title = props => <h1>{props.children}</h1>;

  ReactDOM.render(<Title children="Hello world" />, root);
  // the same as:
  ReactDOM.render(<Title>Hello world</Title>)
  ```

* We can pass React components to each other as children -- **composition** so can encapsulate functionality in a component and then wrap that component around whatever we like.

#### Using classes to create components
Two types:
1. stateless - functional components (can still be dynamic if parent state changed and passed down using props)
2. class component - interact-able to have a state. Props: passed in (like html), state that is local to that component that can be set (props can be passed in and changed). If state changes, then React will re-render with new state (and any children that has referred to this state via props) 
```jsx
// need to extend React.Component for this.state and render()
class Title extends React.Component {
  render() {
    return <h1>{this.props.children}</h1>;
  }
}
```
* Must have `render` that `return` a React element object
* Acess props via `this` - React binds them so can access `this.props`
* React allows components to keep track of their state as an object

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      toggled: false
    };
  }
  render() {...}
}
// OR using ES7 properties proposal - set properties directly on the class instance without needing a constructor::
class Toggle extends React.Component {
  state = {
    toggled: false
  }
  render() {...}
}
```
* With the `state` object: need to be initial **controlled component** i.e. the state needs default values set (not a controlled react component, and then will treat it as an HTML stuff)
`setState` method: pass it a function that takes the previous state and returns a new state object
  * ensure state is always updated correctly, as `setState` calls do not necessarily happen synchronously in order (React may batch them for performance).
  * `setState()` order is important - uses object passed in based on state to create a new object — immutable state: - doesn’t mutate the original state.
  * Only mutate `setState()` —> do not use `this.state` (except in constructor) to change, only within setSate.
* setState - 1st argument: (change state based on previous state), second is callback after state has updated
  * Props is second argument — that are passed in as callback (since async and may have changed in higher up the tree). So not use `this.props` since could’ve changed
  The updater function can also take props if you need to use them to set the new state:

  ```jsx
  class Counter extends React.Component {
    state = {
      count: 0
    }
    inc() {
      this.setState((prevState, props) => {
        return { count: prevState.count + props.increment }
      });
    }
  }
  ```

  If your update does not depend on the previous state then you can just pass a new state object instead of the updater function:

  ```jsx
  class Toggle extends React.Component {
    state = {
      toggled: false
    }
    toggleOn() {
      this.setState({ toggled: true });
    }
    render() {...}
  }
  ```
* You can pass event handlers to React elements as props: they start with 'on' and end with the event name (e.g. `onClick`, `onChange`, `onMouseEnter` etc).
* **expression** - reduces to a value and always returns something vs **statement**- series instructions `if(){//do stuff and not return}`. 
  * JSX needs to be expression
Using JSX but want `if /else` - must use ternary or anonymous function with statement inside
  * `render({//JS})` can do regular javascript inside with return but not good practice as run each time called, should have JSX syntax for JS inside

#### this
* `this` refers to local context on what called on. Default - is the window or module or strict mode, there is no default context (so is undefined)
* Need to ensure `this` is correct so use arrow functions that passes the class's context to your method `<button onClick={() => this.toggle()}>Toggle}</button>` - **syntactic binding**
  * Or we create a new function from the old method, but _bound_ to the class instance.

  ```jsx
  class Toggle extends React.Component {
    constructor {
      this.state = { toggled: false };
      this.toggle = this.toggle.bind(this);
    }
    toggle() {...}
    render() {...}
  }
  ```
  * Or use `toggle = () => {...};` (ES7). Because arrow functions always get their context from where they were defined this `toggle` will have the correct `this`.
  * [arrow function is different to functional expressions. without {} = returns  automatically / const and let isn’t hoisted.]

  #### Lifestyle
  * React goes through a whole cycle of stages when rendering a component. Can hook into a component's "lifecycle" methods  as part of the react classes lifecycle: every time re render and after creation, constructor run, then runs through element 
    * `componentDidMount` - rendered in virtual DOM bit not painted on page yet. Data fetching and DOM manipulation

#### Forms
Look [here](https://github.com/oliverjam/intro-react-workshop/tree/master/04-storm-the-form)
* Unlike most HTML elements, forms inputs have their own internal state stored in the DOM
  * we use a technique called "controlled components" to keep our input state in React instead of the DOM. 


#### Stop watch example
```jsx
  <script type="text/babel">
      const root = document.getElementById('root');

      class Stopwatch extends React.Component {
        state = {
          running: false,
          time: 0
        }
        toggle = () => {
          this.setState(prevState => {
            if (prevState.running) {
              clearInterval(this.timer);
            } else {
              const startTime = Date.now() - prevState.time;
              this.timer = setInterval(() => {
                console.log(Date.now() - prevState.time)
                this.setState({ time: Date.now() - startTime });
              }, 0);
            }
            return { running: !prevState.running };
            }
          )
        }
        clear = () => {
          clearInterval(this.timer);
          this.setState({ running: false, time: 0 });
        }
        render() {
          return (
            <div>
              <div>{this.state.time}</div>
              <button onClick={this.toggle}>
                {this.state.running ? 'Pause' : 'Start'}
              </button>
              <button onClick={this.clear}>Clear</button>
            </div>
          );
        }
      }

      ReactDOM.render(<Stopwatch />, root);
    </script>
</body>

</html>
```