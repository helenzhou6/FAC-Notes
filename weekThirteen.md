# Week Thirteen

_Resource:_ [_week thirteen schedule and learning outcomes_](https://github.com/foundersandcoders/react-week) _and_ [_list of resources_](https://github.com/foundersandcoders/react-week/blob/master/resources.md)

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
_Resource:_ [_the workshop_](https://github.com/oliverjam/intro-react-workshop) _and_ [_stopwatch workshop_](https://github.com/oliverjam/intro-react-workshop/tree/master/workshop-top-notch-stopwatch) _and_ [_react docs: clock (v good)_](https://reactjs.org/docs/state-and-lifecycle.html)

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
  * `title` is not a DOM element, it's an object. Therefore need to `render` the object onto the page using `ReactDOM.render(title, root);` (only do this once - So usually at the top level have `<App />` that has multiple react components and composes it all together)
  

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
1. stateless - functional components (can still be dynamic if parent state changed and passed down using props) / don’t have states, just used to present data passed down e.g. `const Repo = props => <div>{props.name}</div>` - is a function that returns JSX defined outside the class
2. class /container component or stateful component- interact-able to have a state. Props: passed in (like html), state that is local to that component that can be set (props can be passed in and changed). If state changes, then React will re-render with new state (and any children that has referred to this state via props). Every class based component has a `render` function
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
`setState` method: pass it a function that takes the previous state and returns a new state object - if changes, React knows and will update the DOM accordingly
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
      // merging is shallow: won't replace anything else in state object
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
            // so component has passed its state down as props to child components
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
* NB: `this.props` is set up by React itself, `this.state` has special meaning, but can add additional fields to the class manually if need to store something that doesn't participate in the data flow (like `this.timer` - the timer ID / `setInterval90` = returns an id that references that interval, that is saved globally within the class and targeted later to clearInterval static values)
* NB **'unidirectional' data flow** - any data from that component can only affect components below them in the tree/ a waterfall of props
* React elements are immutable - once created can't change children or attributes

#### Destructuring
_Resouce:_ [_destructing workshop_](https://github.com/oliverjam/learn-destructuring)

* An ES6 feature that pulls values out of arrays or objects and assigns them to variables. E.g. `const { name, surname } = { name: 'Zooey', surname: 'Miller' };` 
* Can grab nested values
```js
const {
  data: { name },
} = { data: { name: 'Zooey' } };
console.log(name); // "Zooey"
```
* Can set defaults when nothing has been passed in (so it is `default` (see below).
  * NB: `null` is a legit value so in these cases the default will not be set (in which case need a ternary operator or `if(null)` within function)
  * Similarly, React can turn '0' into a string and render that rather than seeing it as a falsy value.
```js
const {
  data: { name = [default value e.g. {}] },
} = { data: {} };
console.log(name); // "{}"
```
It also works in function parameters:

```js
function formatName({ name, surname }) {
  return `${name} ${surname}`;
}
const user = { name: 'Zooey', surname: 'Miller' };
console.log(formatName(user)); // "Zooey Miller"
```
This enabled a cool pattern—named function parameters:

```js
function calculateTotal({ subtotal, tax, tip }) {
  return subtotal * (1 + tax) + tip;
}
const total = calculateTotal({ tax: 0.2, subtotal: 100, tip: 10 });
console.log(total); // 130
```
And using it with React
```jsx
class Stopwatch extends React.Component {
  state = {
    time: 0,
    running: false
  }
  render() {
    const { time, running } = this.state;
    return (...)
  }
}
```
Another nice trick is to combine destructuring with the rest operator (`...`) to pull off just the parameters you need:
  * With the rest operator: if given an object, will return an object (and same with array)
  * With regular JS you can’t spread on object onto a line (only into another object), but JSX allow you to do `{…restProps}` to pass props down to children on one line (React magic)
```jsx
const TextInput = ({ id, label, ...restProps }) => (
  <label htmlFor={id}>
    {label}
    <input id={id} {...restProps} />
  </label>
);
```
You can also rename the properties during destructuring, in case there may be conflicts with ones you already have.

```js
class Counter extends React.Component {
  state = {
    count: 0,
  };
  render() {
    // this line is only needed for the 'Count:...' can access it
    const { count } = this.state;
    return (
      <button
        onClick={() =>
        // first argument (prevState containing count is deconstructed and renamed to prevCount), and second argument props is deconstructed to get step, where <Count step=2 />
          this.setState(({ count: prevCount }, { step }) => ({
            count: prevCount + step,
          }))
        }
      >
        Count: {count}
      </button>
    );
  }
}

```
### Bundlers
_Resource:_ [_hackMD notes_](https://hackmd.io/B_RJWosBT9Ksq4E9hptLHQ#/)
* Bunders made ES Modules work, and can transform code (minification/ transpilation / compilation from another language /include non-JS asset like `style.css`)
* Entry point - usually `index.js`, use `import` and `exports` etc to transpile
`commons.js` - what all pages has // vs one page specific javascript bundles (can async load what need)
* **Compile** - like changing to different language VS **transpile** - editing what you write

### Dynamic data workshop
_Resource:_ [_workshop using GitHub API_](https://github.com/sofiapoh/react-dynamic-data-workshop)
* User card brokedn down into some top level components. These components will be `Class` components so that we'll have access to state and lifecycle methods. These components will act as "containers" that will do our data fetching and then pass that data down to purely presentational _functional components_. 
* Lifecycle methods are a place to run functions related to the components lifecycle. Run a data fetching function (API call) as the component mounts the DOM, in `componentDidMount()` - do `getUserData(username).then(data => this.setState({userData: data}));`
* Usually data over network gets to us slower than DOM renders content so to safeguard, in `render()` have:
```js
if (!this.state.userData) {
  return <h3>...Loading</h3>;
}
```
  * > Note that unless the payload of the made request is not paritcularily heavy you might want to skip loading state and just return `null` to defer rendering until the content is ready. This way you won't get a janky looking flash of loading state before the component finishes loading.

* If passing down something to children via props and relying on async daya (e.g. GitHub API call includes repos_url) - need use a ternary statement to render the component only when the data is fully loaded: `{repos_url ? <RepoList url={repos_url}` within `render()`
* `{data.map(repo => <Repo key={repo.id} {...repo} />)}`. With dynamically generated content, and any looping over the data need to make a key, so when the content updates (e.g. node has been deleted etc), then the `key` is how React can keep track of all the nodes
  * This `map` function must be written inside `render()` - can't wrap it in a function elsewhere since the context needs to be bound (so it won't run within the React file). Usually can avoid by using arrow functions (it automatically binds to context by default - `() => this.renderList()`), otherwise need to bind ‘this’ - `this.renderList.bind(this)` within `render()`

### Integration tests
_Resource_ [_Testing workshop_](https://github.com/oliverjam/learn-react-testing)
* Test UI (checks functionality) not tests on implementation. So to get elements use things like `getLabelByText` since priority is find labels by text since that is what the user finds. If change code then tests should fail.


#### Jest
* Jest will automatically look for any filenames ending in either `.test.js` or `.spec.js` and run them. It'll also run anything in a folder called `__tests__`. `package.json` needs `"test": "jest --watch"` for watch mode.
* Common component folder setup:
  ```
  button
      ├── button.css
      ├── button.js
      └── button.test.js
  ```
* > It's worth noting that `toEqual` performs a recursive check of all properties on an object (sometimes called deep equal). `toBe` on the other hand checks object identity.

#### React Testing Library
* [React Testing Library](https://github.com/kentcdodds/react-testing-library) is designed to help you write good React integration tests. 

* Use `render` and `Simulate` from this library.
  * > Simulate does _not_ simulate actual browser events. This means there are certain caveats, like simulating a click on a submit button will not trigger the form's submit event. Read [this section of the React Testing Library](https://github.com/kentcdodds/react-testing-library#fireeventnode-htmlelement-event-event) docs for a workaround.
    * React events are different to DOM events - react are root elements (not bound to component - bubbles up) -> means normally clicking a button will bubble up and submits the form but doesnt. 
    * Not in DOM - just lives in memory (render method renders it to the root element and not on document yet). `renderIntoDocument` -> would add it to the actual document so can fire actual DOM event like submit the form. Need to re-render the DOM if needed to `renderIntoDocument` or else weird side effects. (`renderIntoDocument` is now `render`)
* Helper methods to find DOM notes: `getByText`, `getByLabelText` and `getByTestId`. The first will find nodes by text content, the second by label content (for inputs), and the third by `data-testId` attributes (for nodes that are hard to find by text). If you can't find a node using one of them you can still use regular DOM methods on the `container` that is returned.
* A useful convenience method when debugging is `prettyDOM`, which you can use to log nicely formatted HTML nodes (`console.log(prettyDOM(node)))`)

* Examples of use:
```js
// EXAMPLE ONE
import React from 'react';
import ReactDOM from 'react-dom';
import Toggle from './toggle.js';
import TestUtils from 'react-dom/test-utils';
import { prettyDOM } from 'react-testing-library';

test('The toggle toggles when clicked', () => {
  const root = document.createElement('div');
  ReactDOM.render(<Toggle>just clicked</Toggle>, root);
  const buttonNode = root.querySelector('button');
  TestUtils.Simulate.click(buttonNode);
  const childrenNode = root.lastChild;
  expect(childrenNode.textContent).toEqual('just clicked');
});
// where toggle is:
      <React.Fragment>
        <button
          onClick={() =>
            this.setState(prevState => ({ toggled: !prevState.toggled }))
          }
        >
          {this.state.toggled ? 'Hide' : 'Show'}
        </button>
        {this.state.toggled && <div>{this.props.children}</div>}
      </React.Fragment>

// EXAMPLE TWO

import React from 'react';
import { renderIntoDocument, cleanup, fireEvent } from 'react-testing-library';
import Jadenizer from './jadenizer.js';

// ensures our document gets cleared out after each test
// so we don't have lots of copies of our component in there
// otherwise our tests might affect each other
afterEach(cleanup);

test('Jadenizer component', () => {
  const { getByText, getByLabelText, getByTestId } = renderIntoDocument(
    <Jadenizer />
  ); // use renderIntoDocument so we have a real document with browser events
  const button = getByText('Jadenize');
  const input = getByLabelText('Enter text for Jadenization');
  input.value = `how can mirrors be real if our eyes aren't real`;
  fireEvent.change(input); // ensure our onChange gets called
  fireEvent.click(button); // fire a real browser event on the submit button
  const output = getByTestId('output');
  expect(output.textContent).toBe(
    `How Can Mirrors Be Real If Our Eyes Aren't Real`
  );
// where Jadinizer is:
        <form onSubmit={this.jadenize}>
          <h2>Jadenizer</h2>
          <label htmlFor="jadenizer-input">
            Enter text for Jadenization
            <input
              id="jadenizer-input"
              className="form__input"
              value={input}
              onChange={e => this.setState({ input: e.target.value })}
            />
          </label>
          <button type="submit" className="form__button form__button--jaden">
            Jadenize
          </button>
        </form>
       {output && <div data-testid="output">{output}</div>}

// EXAMPLE THREE
import React from 'react';
import {
  renderIntoDocument,
  fireEvent,
  cleanup,
  waitForElement,
} from 'react-testing-library';
import fetchMock from 'fetch-mock';
import Markdownifier from './markdownifier.js';

afterEach(cleanup);

test('Markdownifier component', () => {
  const mockResponse = `<h1 id="a-heading">a heading</h1>`;
  fetchMock.mock('https://micro-marked-nqbbqbtkrq.now.sh/', mockResponse);
  const { getByText, getByLabelText, getByTestId } = renderIntoDocument(
    <Markdownifier />
  ); // use renderIntoDocument so we have a real document with browser events
  const button = getByText('Markdownify');
  const input = getByLabelText('Enter markdown');
  input.value = `# a heading`;
  fireEvent.change(input); // ensure our onChange gets called
  fireEvent.click(button); // fire a real browser event on the submit button
  waitForElement(() => getByTestId('output')).then(output => {
    expect(output.innerHTML).toEqual(mockResponse);
  });
  // wait until our element callback finds a DOM node
  // then we have access to the node
  // so we can assert against it to make sure the innerHTML is correct

  // where Markdownifier is:
          <form onSubmit={this.markdownifier}>
          <h2>Markdownifier</h2>
          <label htmlFor="markdown-input">
            Enter markdown
            <textarea
              id="markdown-input"
              className="form__input form__input--markdown"
              value={input}
              onChange={e => this.setState({ input: e.target.value })}
              rows="6"
            />
          </label>
          <button type="submit" className="form__button form__button--markdown">
            Markdownify
          </button>
        </form>
        {output && (
          <div
            data-testid="output"
            dangerouslySetInnerHTML={{ __html: output }}
          />
        )}
});


```

* ([Workshop](https://raw.githubusercontent.com/oliverjam/learn-react-testing/master/README.md) has more information on why haven't covered Enzyme and Snapshot testing)

### `this`
* The value of `this` can change (dynamic). 

1. By default, using `this` in the global scope = `this` will be the global object --> such as the `window` object of the browser.
  * > The window object is the default local scope, so it is what `this` defaults to if there is not a more specific object for `this` to be bound to.
  * > In strict mode there is no default local scope so `this` is `undefined`
  * So within React classes, using `this.state` is fine if it is not used outside the context of the class
2. `this` value is based on where the function containing `this` is invoked.
  ```js
  function returnThis() {
    return this;
  }
  const person = {
    name: "Zooey",
    returnThis
  };

  person.returnThis(); // returns { name: "Zooey", returnThis: Function }
  ```
  Where `this` is now the object it is called upon.
  * > This is how `this` is bound for [function declarations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function) which are one of the multiple ways to define functions in javascript.
  * The problem: using `this` when writing a method within a React class e.g. `myFunc() = {this.state.time}`. Then `.onClick(this.myFunc)`, since when function is invoked it is within the browswer, (the call context has changed), so `this` will be the `window` object of the browser, rather than `this.state.time` within the React class (context of what `this` should be will be lost). (Also when trying to assign the method to another variable: `const returnThisTwo = person.returnThis;` - `this` is window object)
    * > To call any function or use any variable is essentially shorthand for doing `window.myFunction/Variable`, so we can understand that in this scenario `this` is effectively whatever object the function is being called on, in this case window.
3. Can explicitly tell the function what `this` is using `.bind()`
  ```js
  const person2 = {
    name: "Charli"
  };
  // we assign the method in this way as during the creation of
  // the object `person2` is not defined
  person2.returnThis = returnThis.bind(person2);

  const returnThisThree = person2.returnThis;

  returnThisThree(); // gives us { name: "Charli", returnThis: f } - even if 
  ```
  * Alternatively, can use arrow functions: `this` will always be whatever `this` was in the lexical context of the function's delaration. 
    * > At the time of writing the object literal the value of `this` is the global object, any arrow functions that refer to `this` declared inside will always refer to the global object, whereas the function declaration has the `this` of whatever it is called on.
3. Within object literal — is the global `this` (since it doesn’t exist until finished writing it) - the lexical context of the `this` is global. 
  * `this` isn’t bound to object yet until it’s made (object literal: in the process of writing it) - global until using
lexical content of `this` before the React object is made is global until runs the function
  * Whilst function can refer to itself since function is still being defined (only once it is called and starts running is `this` defined)
  * Example one:
  ```js
  // for `person.returnThis()` to return { name: "Taylor" etc }
  const person = {
    name: "Taylor",
    // myThis: this --> this will be undefined since JS looks through and sees global 
    // myThis: () => this --> same here as object not defined yet
    returnThis: () => this // --> if going to use it on itself, object is not defined yet, so looking at the context it has been defined (as arrow function) - `this` refers to global undefined
    returnThis: function () {
      return this; // --> this is correct - the `this` will refer to the person when calling `person.returnThis()` (after person has been made)
    }
  };
  // and if bind (doing `.bind(this)` at the end of the object (then same as arrow function), then would still be undefined since object not created yet

  // can also do person.returnThis = person.returnThis.bind(person); to bind `this``
  // since person has now been defined and binds 'this' after person is made so can access 'this'

  ```
  * Example Two:
  ```js
  class Cat {
    constructor(name) {
      this.name = name;
    }

    sayHi() {
      return `Hi I'm ${this.name}, miaow`; // refers to window at the moment
    } 

  // SOLN: using arrow function, but no longer declaring a method inside a class using the syntax `sayHi(){}`
  // sayHi = () => {
  //   return `Hi I'm ${this.name}, miaow`;
  // }

  // OR function declaration and the `this.sayHi = this.sayHi.bind(this);` within the constructor (since have access to `this` inside there)
  // lexical `this` inside the constructor is the `this` of the class

  // OR class property that has a function assigned to it
  // sayHi = function() {
  //   return `Hi I'm ${this.name}, miaow`;
  // }.bind(this)
    const getCatName = ({ sayHi }) => sayHi();

    const catGreeting = getCatName(new Cat("Francois"));
  }
  ```
  * Example three:
  ```js
  const Button = ({ onClick, children }) => (
    <button onClick={onClick}>{children}</button>
  );

  class Counter extends React.Component {
    constructor() {
      super();
      this.state = {
        count: 0
      };
      // this.handleUpdateCount = this.handleUpdateCount.bind(this); -- 1. SOLN - to bind `this` to the class 
    }
    //   handleUpdateCount(type) {
    //     return function() {
    //       this.setState(({ count }) => ({ // INCORRECT this is the window
    //         count: type === "inc" ? count + 1 : count - 1
    //       }));
    //     };
    //   }

    // NB THIS IS A HIGHER ORDER FUNCTION -- it is returning a function, so no need to write one for 'inc' and one for 'dec'
    // method on a class- shorthand
    handleUpdateCount(type) {
      console.log(this) // this would be within the class, since it is called immediately upon creation of the class (should be called 'makeUpdateCountFunc')
      return () => { //-- 2. SOLN using arrow functions
        console.log(this) // this would be the window by default since it is called onclick in a different context
        this.setState(({ count }) => ({
          count: type === "inc" ? count + 1 : count - 1
        }));
      };
    }

    // 3. OR this arrow function to generate the function
    // handleUpdateCount = (type) => () => {
    //   this.setState(({ count }) => ({
    //     count: type === "inc" ? count + 1 : count - 1
    //   }));
    // }

    // 4. OR bind the `this` at the end
    // handleUpdateCount(type) {
    //   return function () {
    //     this.setState(({ count }) => ({
    //       count: type === "inc" ? count + 1 : count - 1
    //     }));
    //   }.bind(this);
    // }

    // OR 5.
    // handleUpdateCount(type) {
    //   this.setState(({ count }) => ({
    //     count: type === "inc" ? count + 1 : count - 1
    //   }));
    // }
    //  AND THEN CHANGE the bottom (the JSX) to: {() => this.handleUpdateCount("inc")}

    render() {
      return (
        <React.Fragment>
          <Button onClick={this.handleUpdateCount("inc")}>+</Button>

          <p data-testid="count">{this.state.count}</p>

          <Button onClick={this.handleUpdateCount("dec")}>-</Button>
        </React.Fragment>
      );
    }
  }
  ```

* NB: One can explicitly bind `this` and call a function at the same time using `.call` or `.apply`.
* Can `.bind()` anything (e.g. `.bind(object)` - can bind it to anything since everything in JS is an object). Can only bind once (so arrow function can't use `.bind()`). Invoking a function is shorthand for function.Prototype.call -- when `.bind(this)`, sets the this (where `this` can be an object etc)

### React Project
_Resource:_ [_React Project Goals_](https://github.com/oliverjam/fac-react-project/blob/master/README.md)

Paired programmed to make a [React game arcade](https://github.com/fac-13/HP-game) - Two games, one to guess how long 10 seconds is, and another: whack a mole (where the mole is your github avatar). Play it [here](https://relaxed-wing-98fffd.netlify.com/).

I then forked this repo and worked on it independently: view the GitHub repo [here](https://github.com/helenzhou6/Whack-a-Mole) and play it [here](https://whack-a-moleeee.netlify.com/)

#### Changing parent's state

* For a child to update a parent component's state, can pass a function down to the child via `props` to set the parent's state: (Thanks to [stackoverflow post](https://stackoverflow.com/questions/35537229/how-to-update-parents-state-in-react))

  ```jsx
  class Parent extends React.Component {
    constructor(props) {
      super(props)

      this.handler = this.handler.bind(this)
    }

    handler(e) {
      e.preventDefault()
      this.setState({
        someVar: someValue
      })
    }

    render() {
      return <Child handler = {this.handler} />
    }
  }

  class Child extends React.Component {
    render() {
      return <Button onClick = {this.props.handler}/ >
    }
  }
  ```

### `Parcel` Setup for React
_Resources:_ [_Parcel setup for React instructions_](https://github.com/oliverjam/fac-react-project/blob/master/docs/setup.md)
* Transpiled with `Babel` - source files are processed (so can use ES Modules, ES6+ and JSX - custom React syntax)
* Bundled all together with `parcel` (and puts them in the `dist` folder)

1. `npm i -D parcel-bundler babel-plugin-transform-class-properties babel-preset-env babel-preset-react`
  * `parcel-bundler` does the actual bundling and `babel-plugin-transform-class-properties`: lets us write our state as an object directly on a class ([more info here](https://github.com/oliverjam/intro-react-workshop/blob/master/03-surpass-with-class/README.md#state))
2. `npm i react react-dom`
3. `package.json`, add script: `"start": parcel index.html`
4.  Create `.babelrc` file containing:

```js
{
  "presets": ["env", "react"],
  "plugins": ["transform-class-properties"]
}
```
> Parcel will automatically use Babel to transpile all ES6 and React syntax out of the box, but we want to use a new feature (class properties), so we need to tell Parcel to use an extra plugin.

5.  Create `index.html` file containing:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My App</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="index.js"></script>
  </body>
</html>
```

> Parcel will use this as an 'entrypoint' to your app. It'll find the `index.js` script in there and follow all the `import`s to bundle everything. Then it'll replace the `index.js` with the final JS bundle when it outputs the `dist` folder.

#### Testing

1. `npm i -D jest babel-jest babel-core`
2.  `"test": jest --watch` add test script to `package.json`

> Jest will read the `.babelrc` file to know what presets/plugins it should use. Parcel actually defaults to using `babel-preset-env` and `babel-preset-react`, but Jest doesn't, which is why we had to install and specify them.

#### Eslint
1. `npm i -D eslint eslint-config-recommended eslint-plugin-react babel-eslint`
2. Create `.eslintrc.json` file with:
```json
{
  "parser": "babel-eslint",
  "extends": ["eslint:recommended", "plugin:react/recommended"]
}
```
> This will tell ESlint to use `babel-parser` instead of its own. That way it won't complain about new things that Babel understands/transpiles away.

### Boilerplate
Made a boilerplate with a backend and client side front end - [here](https://github.com/helenzhou6/React-setup)

### Deploying a React App - Netlify
_Resources:_ [_deploying instructions_](https://github.com/oliverjam/fac-react-project/blob/master/docs/deploying.md)

* Add to `package.json` - `"build": parcel build index.html --public-url ./`
> If you get build errors using Parcel with Netlify it may be because they build using an older Node version by default (v6!). You can tell them to use whatever version you're using by setting an environment variable called NODE_ENV in the 'Deploy Settings' (scroll down to 'Build environment variables').
* To look at deploying on Heroku - look at the boilerplate I made [here](https://github.com/helenzhou6/React-setup)

### React routing
* [A React Router Tutorial - Medium article](https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf)
* And a new router library focused on accessibility and simplicity - [Reach Router](https://reach.tech/router/)
  > Might be worth a look if you don't need all the features the full React Router provides
  * Look at the [Craft Track project](https://github.com/fac-13/craft-track) for an example of Reach Router

### Other snippets of code learnt
* Only strings and symbols allowed as JS object keys (so not need to put '"' around them)
* Use **composition** to share code among React components - use JSX to add `<Toggle />` to <Counter /> (not via inheritance - don’t extend the class). Should only every extend `React.component`
* Can only have a single child in react (so need to wrap html in a `<div>` or wrap it in `React.fragment`) / `React.fragment` = not need to wrap in div (can only return one thing from React component), unless use this
* Event bubbling - synthetic event with react — full react does performance stuff with events
* React can keep a record of function calls - time travel 