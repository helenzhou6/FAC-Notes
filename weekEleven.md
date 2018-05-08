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

* `return [true/false] && [true/false]` - if `true && false` then would equate to `return false` and if the first is `false` then overall would be `return false` too (you can use this outside of `if` statements) -> called **bitwise operators**
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

## Day Two
_Resources:_ [_FAC learning outcomes_](https://github.com/foundersandcoders/london-programme/blob/master/weeks-10-16/learning-outcomes-project-1.md) _and_ [_notes about project work_](https://hackmd.io/f9yAZlxDTtyru_u1Mb9fPg?view), 

### Preparing for build sprint

1. Break down your user journey into user stories
2. Re-draw your wireframes (depending on whether the results of your user testing has changed your ideas from the prototype)
3. Talk through your tech stack choices - look [here](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/weeks-10-12/build-sprint/tech-choices.md) for more advice
4. Software architecture
5. With your Product Owner and Scrum Master, write tasks in issues (an issue per user journey > broken into user stories > then broken down into smaller tasks each as an issue).
    * Each issue should contain:
        - pictures of your wireframes
        - acceptance criteria (define these with your PO)
        - a priority label
        - a time estimate
6. Order issues in milestone and assign them

### Starting build
1. Write your initial `README.md` ([good example here](https://github.com/fac-12/Little-Window) and [another template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2))
that contains:
    - description of app
    - problem statement
    - user stories
    - team `git flow` section that can be referred to by anyone so everyone is clear on the work flow the team will follow.
    - instructions for how to run/set up local environment - helpful for other devs and code reviewers
    - images of prototype :camera: or a link :link: to figma/invision (or both)
    - tech stack 
    - section on learning - this can be updated with useful links as you create your project. This helps everyone in your project have the resources needed.

2. Set up your environment
    - Deployment (e.g. Heroku)
    - Continuous Integration (e.g. Travis)
    - Testing suite (travis or other CI?)
    - linter/formatting settings
    - File structure
    - import [dwyl labels](https://label-sync.herokuapp.com/) also take a look at the descriptions for labels [here](https://github.com/dwyl/labels#labels)
    - pre-commit - this checks certain criteria and advises you not to commit if these are not met
        - passing all tests?
        - no linter errors
        - test coverage over a certain %
3. Decide on the git flow e.g.:
    - assign yourself to an issue and add `in-progress` label :traffic_light: 
    - starts work on issue
    - commit work referencing issue add maybe adds an [emoji](https://gitmoji.carloscuesta.me/) :smile: 
    - create a pull request
        - reference issue add 'in-review' label
        - add description of work completed
        - `assign` all of your team
        - if you are still working on the PR add [WIP] to the title (work in progress)
    - __everybody__ in the team reviews the PR (all need to state 'approved') - **IMPORTANT!!!** :warning: 
    - the creator of the PR should respond to all the questions and/or make changes requested (or say why don't want to change has to be a good reason)
    - the final team member to review should `assign` the QA and add `awaiting-merge` label :clock1: 
    - QA to merge into master branch and delete the other branch

### The Scrum Framework
_Resources:_ [_youtube video on scrum framework_](https://www.youtube.com/watch?v=_BWbaZs1M_8)

* Agile is umbrella of methods - scrum framework part of it, where based on shared commitment of all members to complete tasks.
    * **Product owner** from business
        * 1) Creates **product backlog** to ensure vision is met - that acts as the wall between outsiders and the developers
    * **Scrum master/agile coach**
        * 2) **Sprint planning** with PO (never longer than 4 weeks - usually 2 weeks). Highest business value (and possibly others) from product backlog taken and commit to doing it within the sprint. This can't be changed (they only need to way <4 weeks).
    * **Team member**
        * 3) **Stand ups / daily scrum** - commitment meeting, to ensure team members - done yesterday, will do today and what in the way to knock it down (esp if outside the team). No longer than 15 mins.

* 4) At the end of every sprint should have a MVP (minimal viable product) - i.e. product backlog completely done and could move to user if wanted to.
* 5) **Sprint review** - show and tell to the outsiders - can see actual progress towards the whole product, can interact with it in a meaningful way and give feedback to the team whilst still relevant. Inspect the product (what). Good ideas can go into the backlog.
* 6) **Retrospective** - scrum master facilitates this meeting: lessons learned but with a purpose - what will change about how they're working together. Inspect the process (how) that will go into next sprint (for continuous development).

### The Scrum Master
_Resources:_ [_dwyl document_](https://raw.githubusercontent.com/dwyl/process-handbook/master/README.md)

#### Responsibilities:
> - Total understanding of the problem (that the product is trying to solve)
>    - liaising with the client over wider project issues so that the team can focus on coding
>    - Full knowledge of the product backlog to enable them to efficiently do backlog grooming, sprint planning and time estimating
>
> - **Lifting blockers** aka making sure the team has everything they need to progress with their work. For example, ensuring there are:
>   - no outstanding external dependencies
>   - all the designs, copy, login/card details needed
>   - decisions made on issues that are in discussion
>
> - **Ceremonies** are the events that the team take part in every sprint. E.g. **stand ups** - quick update for the whole team to ensure everyone is aware of what the team is working on and ensure priorities are aligned
>        - They should quickly cover what each team member has worked on since the last stand up, what they plan to be working on until the next one and crucially, whether there are any blockers, know of any _risks_ with the issue they are working on or have any questions for the team. Should be done with the scrum backlog open and referring to issue numbers
>        - Held at the beginning or end of the day, lasting 2-4 mins per team member
>
> **Sprint Demo** (or review) - the client is shown the completed user stories for the milestone. The demo will usually be introduced by the scrum master or the product owner. The demo will then be handed over to the developers who have been working on it as they begin to go through the user flow. In your walkthrough try and tie all of the work you've done to the proposed scope (milestone).
>
> **Retrospective** 10-30mins - each team member should have the opportunity to share things that they felt **went well**, **not very well** or things that could be **improved** based on the last sprint.
>
> **Spring Planning** 1-1.5hrs. PO to prepare backlog with user stories and acceptance criteria for each issue. Review the backlog and client's priorities to outline what will be put into the next sprint milestone. / Aim is to recognise the highest priority issues and how long it would take to complete them