
# Week One
_Resource:_ [_week one schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-1).

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
_Resource:_ [_the challenge solution_](https://github.com/foundersandcoders/accessibility-challenge/blob/solution/index.html)

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
* Also look at the ```form``` and ```quote`` sections of the solution.

* Look into [aria labels](https://stackoverflow.com/questions/22039910/what-is-aria-label-and-how-should-i-use-it)
* Look into [Preloading content with rel="preload"](https://developer.mozilla.org/en-US/docs/Web/HTML/Preloading_content)

### **Practicing Git**
_Resource:_ [_Git challenge_](https://github.com/foundersandcoders/git-workflow-workshop-for-two)

Introduction into the idea of forking a repository. Never work on the master! Always work on a branch, then once accepted by a second programmer it is merged into the master.
1. Found a problem: create an issue on repo
2. ```git pull origin master``` to pull remote repo (origin refers to remote)
3. ```git checkout -b <new-branch-name>.``` to create new branch and change HEAD to branch.
4. Update the files/do code. ```git add``` and then ```git commit```- with a commit messages linking to the issue (e.g. ```Relate #2```)
5. In case remote master has changed since, do ```git checkout master``` (switch head to master first), ```git pull origin master```, then checkout to branch. Finally do ```git merge master``` and work out any merge conflicts.
6. Then ```git push origin <branch-name>``` the branch.
7. Generate a pull request and link in the comments  with the issue #<number> (```Relate #2```)
8. Second programmer reviews changes and accepts the merge (branch into master)
9. Close issue and branch

#### Other useful stuff
* [HTML reference](https://htmlreference.io/) - that features all elements and attributes.
* [CSS reference](https://cssreference.io)
* Before closing the commit message with a quote symbol you can press enter on your keyboard to continue typing in the new terminal line. The text in the second line can be used as an additional message.
* git commands
    * ```touch``` - new file
    * ```Ctrl C``` - Esc
    * ```git checkout``` - like ```cd``` for git


### **Research Afternoon**
_Resources:_ [_Topics Covered_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/research-afternoon.md) and [_good presentation tips_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/presentation-guidance.md)

#### Accessibility Team
_Resources:_ [_the team notes_](https://hackmd.io/s/SJ2RdkQOz)
* Use [skip links](https://www.joedolson.com/2013/07/designing-accessible-navigation/) to other sections of your site first
* Use the ```accesskey``` attribute
* Elements should be assigned a ``role`` and/or an ```aria-label``` attribute
* Give focus to modals by using the ```tabindex``` attribute or ```.focus()```

#### Regex
_Resources:_ [_the team notes_](https://hackmd.io/CYUwHA7ADGDMUFoCMEkE4EBYQDNECMBjAQ1gTSTADYpCoJRD8g==?view)

* Made up of ```/pattern/modifiers;```. Modifiers = flags
* NB Mulitpliers: ```a{n}``` : exactly n of a

#### Command Line
_Resources:_ [_the team notes_](https://hackmd.io/IYTgrALBDs3AtMAHAMwAzwmgpheAjJEANnhAgBN8LoQUoBmCIA==)
* Keyboard shortcut ```Ctrl + K``` deletes line from position of cursor onwards
* Can create shortcuts/funcs e.g. ```alias chrome=’open –a “Google Chrome”’``` and ```alias fac=”cd desktop/coding/fac”

## Day Three
### **CSS Gallery challenge**
_Resources:_ [_CSS gallery challenge_](https://github.com/foundersandcoders/css-gallery-challenge)
* Instead of using floats, can use ```display:flex``` and ```flex-grow: 1;``` to complete tasks 1-3
* Duplicating the photo three times (task 4) - use ```background-image:repeat-y```

### **Projects**
_Resources:_ [_The project_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-1/project.md)
* (Practiced coding within a group and [flying down the git rabbit hole](#practicing-git))