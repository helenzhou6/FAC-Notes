#  Week Eleven

_Resources:_ [_the london timetable_](https://github.com/foundersandcoders/london-programme/blob/master/weeks-10-16/week-11.md) 

Project One Week Two - Build part 1 of 2.

## Day One

### Object and arrays Challenge
_Resources:_ [_Array methods, reduce, and object manipulation challenge_](https://github.com/foundersandcoders/mc-objects-and-array-methods-mashup)

> As a note, rewriting these array methods yourself with reduce is not good practice for code you're going to use, this is more just a learning exercise. 
* Can solve some of the problems using `Object.values()` - which iterates over the object and returns an array of the object's values.
* NB: `fn` is a function like `num => num + 1`

#### `.reduce()`
* `Array.prototype.reduce()` takes three arguments: **accumulator**, `currentValue` and `currentIndex` = the current index of the current element being processed in an array.

* The accumulator that accumulates all the previous `callback`'s `return` values - thus always need to `return` out of the `callback`:
    ```js
    function filter(array, fn) {
        return array.reduce((acc, current) => {
            return fn(current) ? acc.concat(current) : acc
        }, [])
        }
    ```

    * For example this iterates through an array, and 
* Can use `.reduce()` in very clever ways:
    ```js
    function some(array, fn) {
        return array.reduce((acc, current) => {
            return acc || fn(current)
        }, false)
    }
    ```
    * Where if `acc` has turned `true` at any point, then `acc` will stay as `true` after all iterations since `fn(current)` will not run if `acc` is `true`

* `return [true/false] && [true/false]` - if `true && false` then would equate to `return false` and if the first is `false` then overall would be `return false` too (you can use this outside of `if` statements)
    ```js
    function every(array, fn) {
        return array.reduce(function(acc, current) {
            return acc && fn(current)
        }, true)
    }
    ```

#### `.map()`
* `Array.prototype.map()` method creates a new array with the results of calling a provided function on every element in the calling array.

    ```js
    var bldngs = {
        14358: {
            address: '1010 Yorkshire Circle, Rocky Mount, North Carolina, 75141',
            rooms: 3,
            value: 450000,
        },
    }
    var minValue = 500000;
    function filterBuildingsByMinValue(bldngs, minValue) {
        return Object.keys(bldngs)
            .filter(function(id) {
            return bldngs[id].value >= minValue
            })
            .reduce(function(acc, id) {
            acc[id] = bldngs[id]
            return acc
            }, {})
    }
    ```
    * Filters the keys of the object first, before using `.reduce()` to loop over. 

    ```js
    function addBuildingIdsToObjects(bldngs) {
        return Object.keys(bldngs).reduce(function(acc, id) {
            return Object.assign(acc, { [id]: Object.assign({ id: +id }, bldngs[id]) })
        }, {})
    ```
    * To return:
    ```js
    {
        14358: {
            id: 14358,
            address: '1010 Yorkshire Circle, Rocky Mount, North Carolina, 75141',
            rooms: 3,
            value: 450000,
        }
    }
    ```
    * The use of `Object.assign()` prevents the original object being mutated.
    * The `+id` turns the string into a number


> Unlike the previous re-writes of array methods the following is actually a useful function to write with reduce as it allows you to filter and map an array without having to iterate over it twice.
>
> For those interested, the type signature for this function is:
> filterMap :: (a -> Bool) -> (a -> b) -> [a] -> [b]
    ```js
    var filterFn(num) => return num % 2 === 0;
    var fn(num) => return num + 1;
    function filterMap(filterFn, fn, array) {
        return array.reduce((acc, curr) => {
            return filterFn(curr) ? acc.concat(fn(curr)) : acc
        }, []);
    }
    ```

### Curriculum planning
_Resource:_ [_the FAC guide_](https://github.com/foundersandcoders/master-reference/blob/master/curriculum-planning/curriculum-update-process.md)

* Following [the FAC guide](https://github.com/foundersandcoders/master-reference/blob/master/curriculum-planning/curriculum-update-process.md) to work on week four, [notes here](https://hackmd.io/k99n2M56Sjy3DNb02DD2oA)
