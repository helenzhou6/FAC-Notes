
# Week One
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
_Resources:_ [_introduction to testing and TDD methodology_](https://github.com/foundersandcoders/testing-tdd-intro), [_the fizzbuzz challenge using TDD method_](https://github.com/foundersandcoders/fizzbuzz), [_and the romanizer code along using same thing_](https://github.com/foundersandcoders/roman-numeral-tdd-codealong)

* Unit testing - one unit/function is being tested.
* TDD methodology: Write a failing test (to specify what you want the outcome of your function to do), then a passing test (write the function to make the test pass, usually in the simpliest way to make it pass) and then refactor (you should be able to see a pattern in your code)

#### Code coverage
_Resource:_ [_The istanbul NPM_](https://github.com/dwyl/learn-tape#bonus-level)

* Code coverage calls your functions and lets you know exactly which lines of code you have written are "covered" by your tests (i.e. helps you check if there is "dead", "un-used" or just "un-tested" code) We use [istanbul](https://github.com/dwyl/learn-istanbul) for code coverage. 

### Other stuff learnt
* In ```./index.js```: write ```module.exports = {fizzbuzz, display};``` to export the ```fizzbuzz``` and ```display``` functions to other files. In the other files put ```var {fizzbuzz, display} = require('./index.js’);``` to import both functions
* ```switch``` statement: to evaluate expressions need to put ```swithc(true)```. Not need to break if you want to continue with function.