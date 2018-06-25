
# Week One
_Resource:_ [_week one schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-1), _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/resources.md).

It's Accessibility themed week!

## Day One
### **Paired programming**
_Resource:_ [_paired programming_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/pair-programming.md).
* We ended up trying to solve a simple kata that should of involved using the [```slice()``` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice). Instead we ended up going down a regex rabbit hole - but discovered that regex looks at each character, sees if it matches the argument, then repeats this process for each character until the end of the string.

### **Accessibility**
_Resource:_ [_accessibility exercise_](https://github.com/foundersandcoders/web-accessibility/blob/master/putting-yourself-in-someone-elses-shoes.md) and [_extra resources_](https://github.com/foundersandcoders/web-accessibility/blob/master/tools-that-can-help.md).
* Use this [HTML5 Outliner](https://chrome.google.com/webstore/detail/html5-outliner/afoibpobokebhgfnknfndkgemglggomo?hl=en) tool to check your HTML makes semantic sense. The section titles are obtained by your ```h1``` (if your nav doesn't have one - put an ```heading``` tag but then hide it.)
* See what accessibility issues your site has by putting it through [this validator tool](https://validator.w3.org/nu/). (We thought this gave more in-depth answers than the Chrome Dev Tools Audit). There are other ones listed [here](https://github.com/foundersandcoders/web-accessibility/blob/master/tools-that-can-help.md) -- check them out, but remember human testing too!

### **Github Scavenger Hunt**
_Resource:_ [_hunt link_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/general/github-scavenger-hunt.md)
* Useful links: [FAC Master Ref](https://github.com/foundersandcoders/master-reference), [FAC International](https://github.com/foundersandcoders/international/) and [London Programme](https://github.com/foundersandcoders/london-programme). Remember to look at the Issues tab!

## Day Two
### **Accessibility Workshop**
_Resource:_ [_the actual challenge_](https://github.com/foundersandcoders/accessibility-challenge) _and_ [_the solution_](https://github.com/foundersandcoders/accessibility-challenge/blob/solution/index.html)

Common things missing are listed in the [_solution_](https://github.com/foundersandcoders/accessibility-challenge/blob/solution/index.html). 
* Remember all the stuff to include at the beginning of your document and in the ```head```:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta author="xxx" />
    <meta description="xxx" />
    <meta name="viewport" content="initial-scale=1">
    <title>xxx</title>
    <link type="text/css" href="style.css" rel="stylesheet" />
</head>
```
* Images needs ```title``` to ensure when you hover over the image it appears.
```
<img src="./catonkeyboard.jpeg" title="Can on a keyboard" alt="Picture of a cat typing on a keybaord" />
```
* Also look at the ```form``` and ```quote``` sections of the solution.

* Look into [aria labels](https://stackoverflow.com/questions/22039910/what-is-aria-label-and-how-should-i-use-it)

### **Practicing Git**
_Resource:_ [_Git challenge_](https://github.com/foundersandcoders/git-workflow-workshop-for-two) _and also added in info learnt over Days two to five_

Introduction into the idea of forking a repository. Never work on the master! Always work on a branch, then once accepted by a second programmer it is merged into the master.

![The git flow diagram](https://i.imgur.com/UhfHrMT.jpg)

![Another git flow diagram](https://raw.githubusercontent.com/foundersandcoders/git-workflow-workshop-for-two/master/images/git-flow-summary-table.png)

![The lifecycle of a branch](https://i.imgur.com/dyCSnVN.jpg)

[GitHub Cheat sheet (made for FAC14 as part of mentoring)](https://hackmd.io/FAnreC54RT27io6imUqFAQ)

1. Found a problem: create an issue on GitHub. This will give you an issue number.
2. ```git pull origin master``` to pull remote master files into your local repo (origin refers to remote)
3. ```git checkout -b <new-branch-name>.``` to create new local branch and change HEAD to branch. (Shortcut for ```git branch <new-branch-name>.``` then ```git checkout <new-branch-name>.```).
4. Update the files/do code within this branch. Once ready to commit: ```git add``` (adds to local staging area) and then ```git commit``` (no longer in staging area, commits changes). N.B.: Link the commit message to the issue (e.g. ```Relate #2```). This commits to your local branch (so the commits have been logged - permanent record).
5. In case remote master has changed since, do ```git checkout master``` (switch HEAD to master first), ```git pull origin master``` (pulls the remote master files - updates your local master), then checkout back to branch. Finally do ```git merge master``` (to merge updated local master with local branch) and work out any merge conflicts.
6. If there are any merge conflicts (where HEAD = changes on your branch) that have been resolved, do ```git add``` and ```git commit``` again to save the changed files. 
7. Then ```git push origin <branch-name>``` (pushes the local branch commits into the remote GitHub branch)
8. Generate a pull request (remote branch -> into remote master) and link in the comments  with the issue #<number> (```Relate #2```)
9. All programmers on the team need to reviews changes. The last person to review the code accepts the merge (branch into master to update the remote master branch).
10. Close issue and branch on GitHub if necessary.

* Neat terminal commands
    * ```touch``` - new file
    * ```Ctrl C``` - Esc
    * ```echo "Test" > README.txt``` to add files to a repo
* Neat git commands
    * ```git checkout``` - like ```cd``` for git
    * ```git commit -a -m “..”```shortcut for add and commit at the same time
    * Before closing the commit message with a quote symbol you can press enter on your keyboard to continue typing in the new terminal line. The text in the second line can be used as an additional message (use `#` to link to issues).
    * `git stash` to stash your changes locally without commiting, allowing you to switch your HEAD without complaints. `Git stash` is like a stack. You can stash multiple things and the last bit you stashed is at the top of the stack. When you use `git stash pop` it will apply your most recent stash back into your code and DELETE it from the stack whereas `git stash apply` will apply your most recent stash and KEEP it in the stack.
* [Cool website](https://flaviocopes.com/git-guide/) introducing git (git setting up a new repo, push and pull and branches)
* [Article:A successful git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Article: Why use GitHub](https://medium.freecodecamp.org/a-developers-introduction-to-github-1034fa55c0db)
* [Git comman cheatsheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) and [references to git commands](https://git-scm.com/docs) and [another cheatsheet](http://ndpsoftware.com/git-cheatsheet.html#loc=workspace) - this one is really good
* **AMAZING [website](http://ohshitgit.com/) that tells you what to do when you end up in a pickle (great name too: 'oh shit git')**

#### Other useful stuff
* [HTML reference](https://htmlreference.io/) - that features all elements and attributes.
* [CSS reference](https://cssreference.io)
* Look into [Preloading content with rel="preload"](https://developer.mozilla.org/en-US/docs/Web/HTML/Preloading_content)

### **Research Afternoon**
_Resources:_ [_Topics Covered_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/research-afternoon.md) and [_good presentation tips_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/presentation-guidance.md)

#### Accessibility Team
_Resources:_ [_the team notes_](https://hackmd.io/s/SJ2RdkQOz)
* Use [skip links](https://www.joedolson.com/2013/07/designing-accessible-navigation/) to other sections of your site first
* Use the ```accesskey``` attribute
* Elements should be assigned a ```role``` (that has specific values) and/or an ```aria-labelledby``` or ```aria-label``` attribute (containing any text) for screenreaders.
* Give focus to modals by using the ```tabindex``` attribute or ```.focus()```

#### Regex
_Resources:_ [_the team notes_](https://hackmd.io/CYUwHA7ADGDMUFoCMEkE4EBYQDNECMBjAQ1gTSTADYpCoJRD8g==?view)

* Made up of ```/pattern/modifiers;```. Modifiers = flags
* NB Mulitpliers: ```a{n}``` : exactly n of a

#### Command Line
_Resources:_ [_the team notes_](https://hackmd.io/IYTgrALBDs3AtMAHAMwAzwmgpheAjJEANnhAgBN8LoQUoBmCIA==)
* Keyboard shortcut ```Ctrl + K``` deletes line from position of cursor onwards
* Can create shortcuts/funcs e.g. ```alias chrome=’open –a “Google Chrome”’``` and ```alias fac=”cd desktop/coding/fac”

## Day Three & Day Four & Day Five
### **CSS Gallery challenge**
_Resources:_ [_Day Three CSS gallery challenge_](https://github.com/foundersandcoders/css-gallery-challenge)
* Instead of using floats, can use ```display:flex``` and ```flex-grow: 1;``` to complete tasks 1-3
* Duplicating the photo three times (task 4) - use ```background-image:repeat-y``` - look at the full code solution [here](https://github.com/foundersandcoders/css-gallery-challenge/blob/master/solution/style.css)

### **Projects**
_Resources:_ [_The project (day three and four)_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/project.md) [_and the day five project presentation_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/general/weekly-projects.md#project-presentation)


Practiced coding within a group to make an [accessible responsive one pager](https://fac-13.github.io/plhh/). Also [flew down the git rabbit hole](#practicing-git). Discovered:

#### Forms
* ```input:valid``` CSS styling
* If inputs have appropriate attributes (e.g. ```<input type="email">```, browser has inbuilt validation (input requires '@' symbol)

#### Layout
* CSS grid!

    ```
    .contact__form {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-template-rows: 1fr 1fr 1fr;
        grid-column-gap: 2rem;
    }

    .contact__form .form__message {
        grid-column: 2/3;
        grid-row: 1/3;
    }

    .contact__form .form__submit {
        grid-column: 2/3;
        grid-row: 3/4;
    }
    ```

#### Code review
_Resources:_ [_How to review code_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/codereviewintro.md)
* Look at [this link ^](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/codereviewintro.md) on how to review code.
    * Use the phrase 'have you considered'. Use images and emojis (use ```:``` or ```Ctrl, Cmd, Space```)
    * Try and quote the specific lines you're talking about (use URLs like [here](https://stackoverflow.com/questions/23821235/how-to-link-to-specific-line-number-on-github))
    * Don't close other people's issues! They should be the one to review the changes and close them once happy.

#### Cool Code
Cool code to deal with stuff!
* [Script for modal that is accessible too (focus is correct)](https://github.com/fac-13/ab-fac/blob/master/script.js)
* [Script for smooth scrolling to sections of the page](https://github.com/fac-13/JIKTDEVS/blob/master/script.js)
*  Code to have a [spinning logo]:(https://fac-13.github.io/JIKTDEVS/)
    ```
    .logo{
        transform-origin: 50% 50%;
        animation:spin 14s linear infinite;
    }
    ```
    > Resources:
    > * [A video](https://www.youtube.com/watch?v=IM8eTD01UE8) which suggests drawing on screen and collecting code from svg editor and then cleaning it. 
    > * [A free svg editor](https://github.com/jakearchibald/svgomg) that would produce svg code from your drawings
        SCGOMG - another awesome site to draw svg and then turn into code
    > * There is also a way to view any .svg as code by simply [opening it in a text editor](https://css-tricks.com/using-svg/)

* Floating label code:
    ```
    .form-container__input-container label{
        position: absolute;
        top: 0;
        left: 0;
        color: #000;
        pointer-events: none;
        transition: 0.5s;
    }
    .form-container__input-container input:focus ~ label,
    .form-container__input-container input:valid ~ label{
        top:-29px;
        left:0;
        color: #fff;
    }
    ```
* For those who have JS disabled, can have a hamburger that is interactive using CSS alone: 

    ```
    .hamburger input[type="checkbox"]:checked ~ .header__list {
        display: block;
    }

    input[type="checkbox"] {
        display: none;
    }
    ```

    Where essentially the hamburger is a checkbox and once is checked a child element becomes ```display: block```.

    However, you can not use CSS to untick a checkbox once ticked, so JS is needed to untick it.

### CSS Styling
_Resource:_ a talk by @oliverjam during Week Three (but I thought the content was more appropriate for week 1 so putting it in here 😬). [_The slides_](https://utilitarian-ui.netlify.com/#/) and [_the code along_](https://github.com/oliverjam/react-todo/blob/master/src/doin-it-live.css)

#### Colours!
* Who knew you could search Dribbble using [colors!](https://github.com/oliverjam/react-todo/blob/master/src/doin-it-live.css) (and each post has a colour palette associated)
* `HSL` is an intuitive way to manage colours. Use the same Hue (the first parameter), and adjust the last % for different shades. `hsla()` for transparency too.
* Saturate your greys:
    > * Some `hsl(150, 0%, 85%)` text (5.4 contrast ratio)
    > * Some `hsl(150, 40%, 85%)` text (6.2 contrast ratio)
* Avoid pure black (use `hsl(200, 10%, 25%)`) and pure white background (use `hsl(200, 10%, 98%)`)

#### Typography
* `line-height` should decrease with `font-size` (title: use `line-height: 1.1`, intro paragraph: `line-height: 1.25` and `p` text: `line-height: 1.5`
* `line-length` should be between 60 to 80 characters (use the `ch` unit (the width of a '0'!)
* Use weight and colour to get contrast rather than just font sizes.
* System fonts are nice:
```
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
```

#### Layout
* Use `background-color` rather than internal border
* Have an inner wrapper with:
    ```
    .main {
    max-width: 74rem;
    margin: 0 auto;
    padding: 1rem;
    }
    ```
    (it's like automatic responsiveness)
* Use Vertically offset box shadows
    ```
    .form__btn {
        padding: 0.5rem 1rem;
        box-shadow: 0 1px 4px hsla(200, 10%, 20%, 0.25);
        background-color: hsl(10, 100%, 60%);
        color: #fff;
        border-radius: 6px;
    }

    .form__btn:focus {
        outline: none;
        box-shadow: 0 0 0 4px hsl(10, 100%, 85%);
    }
    ```
> * The basic difference between **CSS Grid Layout** and **CSS Flexbox Layout** is that flexbox was designed for layout in one dimension, layout in a row or a column. Grid was designed for two-dimensional layout, layout in rows and columns at the same time. Info [here](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Relationship_of_Grid_Layout)
* CSS Grid can sometimes be easier (for IE can use `@supports`)
    > *  [Jen Simmon's YouTube channel](https://www.youtube.com/channel/UC7TizprGknbDalbHplROtag). She wrote a lot of the grid spec and does a good job of showing off some of the cool stuff you can do with it
    > * [Wes Bos' CSS Grid course](https://cssgrid.io/): This free video course covers pretty much everything to do with grid and is really fun.
    * Difference between `autofit` and `autofill` is the latter fills the remaining space with empty columns

#### Other stuff
* You can use `:not(X)` (e.g. `:not(:nth-child(2n+1)) `)
* `outline-offset` for outlines are cool!