# Pre course notes
Links and snippets I'd like to record... (based on the [pre course FAC materials](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/precourse))

## My Pre course materials
* [Recommended materials by FAC to go through](https://github.com/foundersandcoders/recommended-materials) _Mixed_
* [Harvard computer science course](http://cs50.tv/2013/fall/) _Videos_
* [Javascript for beginners - with cats! ](http://jsforcats.com/)_Article_

## FAC Pre-course set up

* [FAC link](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/precourse#installation-list) _Instructions_
* [Information about Node](https://github.com/maxogden/art-of-node) _Article_
* [Bash Cheat Sheet](https://learncodethehardway.org/unix/bash_cheat_sheet.pdf) _Article_
* [Bash Reference](http://www.gnu.org/software/bash/manual/bashref.html) _Article_

## Useful Snippets
### Using Terminal/Command Line to navigate  
* Directory = folder. If starts with '.' - it is hidden in the operating system. **ls -a** lists the hidden directories as well.
* **pw** = print working Directory
* **mkdir** = make a new Directory
* **cd** = change Directory
* **ls** = list Directory
* **rmdir** = remove directory. If you try to do rmdir on Mac OSX and it refuses to remove the directory even though you are positive it's empty, then there is actually a file in there called .DS\_Store. In that case, type **rm -rf** `[dir]` instead (replace `[dir]` with the directory name).
* **pushd** = push directory, pushes the current location and goes to inputted path
* **popd** = pop directory, goes to last pushed location and removes it from list
* **cp** = copy a file/directory. Use the **cp -r** command to copy more directories with files in them.
* **mv** = move a file or directory (changes the path) or rename it
* **less** = opens a file, page by page. **q** to exit.
* **cat** = prints a whole file without pages
* **find** = find
* **diff** = difference between two files. Add **-u** for _unified format_ (easier to read).
* Also learn **sudo**, **chown** and **chmod**
> N.B. Tip: Notice how sometimes I put a **/** (slash) at the end of a directory? That makes sure the file is really a directory, so if the directory doesn't exist I'll get an error.  

### Using git and github
##### Terms
* **Repository** - is a collection of files tracked by git as a cohesive unit
* **Commit** is a snapshot of every file within your repository at the time the commit was made. Useful to make  one **commit** per logical change. When choosing whether to commit, just keep in mind that each commit should have one clear, logical purpose, and you should never do too much work without committing.
* **log** shows the commits in a particular repository prior to the current commit. Commits make up the log output.
* **HEAD** the commit you are currently working on.
* **Staging area** files are added to the staging area (acts as an intermediate), and once ready to commit all files are bundled together under one commit. Staging area has a copy of the most recent commit by default (until changes are added to it)

> N.B. Many projects contain a file named "README" that gives a general description of what the project does and how to use it.

##### Git terminal commands
* **git log** log of git commits (each with unique ID) of the repository you are currently in. **git log --stat** gives more  stats (**q** to exit)
* **git diff** differences
between the commits when passing in two git IDs as arguments. If no arguments are submitted, then it lists the differences between the staging area and working directory. **git diff --staged** compares staging area to most recent commit (useful right before commiting)
* **git reset --hard** discards any changes to staging area and working directory (!!! - can't change it back!)
* **git clone [url]** copies one repository on one computer to another
* **git checkout** revert all files back to how it was in a previous commit (like restoring a prev version). Will have detached HEAD warning
* **git clone [url]** make a copy of an entire Git repository, including the history, onto your own computer
* **git init** initialises new git repository (creates an empty .git repository). Files in the working directory is transferred to the .git repository.
* **git status** what files have changed since the last commit
* **git add [Files]** To add new files to the repository or add changed files to staged area.
* **git commit** (commit them); **git commit -a -m "commit message goes here"** to commit all files (-a) and add a message (-m)
> * Many command line tools, including Git, are less useful if your files contain very long lines. For example, if you use **diff** to compare two files that have all their content on the same line, **diff** will only show you that the two files are different. It will not be able to pinpoint the location of the difference for you. Many developers use a max line length of 80 to 120 characters.  
> * Standard practice is to write commit messages as if they are command lines

##### Git Errors and Warnings Solution
[Udacity Trouble Shooting Guide](https://storage.googleapis.com/supplemental_media/udacityu/2960778953/troubleshooting_guide.pdf)
* _Should not be doing an octopus._
Octopus is a strategy Git uses to combine many different versions of code together. This message can appear if you try to use this strategy in an inappropriate situation.
* _You are in 'detached HEAD' state._
HEAD is what Git calls the commit you are currently on. You can “detach” the HEAD by switching to a previous commit, which we’ll see in the next video. Despite what it sounds like, it’s actually not a bad thing to detach the HEAD. Git just warns you so that you’ll realize you’re doing it.

### Semantic HTML
**Semantics** - meaning behind what you said.
* When you put a url into web browser - sends a request to website. Would like the documents on the location indicated. The website retrieves files and sends to web browser and read.
* HTML5 offers `<section>`, `<article>`, `<aside>` and `<nav>` subsections.
> * Transition to an HTML5 document structure is made smoother by incorporating some ARIA landmark roles, which are both relatively well supported and somewhat analogous to the section-based navigation we should expect later.
> * Landmark roles, such as role=“contentinfo” and role=“banner”, address accessibility only — not data mining — and each may be used only once per document.
* The main purpose of semantic HTML is for the automated extraction of meaning from content.
* A semantic element clearly describes its meaning to both the browser and developer - for example, instead of `<div>` and `<span>`, we can use better alternatives such as `<form>`, `<table>`, and `<img>`. Helps:
  * accessibility - help assistive technologies read and interpret your webpage
  * searchability - help computers make sense of your content
  * internationalization - only 13% of the world are English native speakers
  * interoperability - help other programmers understand the structure of your webpage

###### Tags for structuring a web page
![Structure of a page](https://almosthumor.files.wordpress.com/2011/09/html5demo1.jpg)
* `<aside>` a sidebar - it should not belong to the main outline of the HTML doc and aren’t usually rendered by assistive technologies`
* `<article>`: a “Independent, self-contained, complete composition in a document, page, application, or site, which is intended to be independently distributable or reusable (e.g., in syndication)”. Must be identified by a heading within the article element.
* `<div>` should be used as a tool for “stylistic applications… when extant meaningful elements have exhausted their purpose.”
* Using `<section>` demonstrably enhances HTML structure without breaking accessibility. It is “A section is a thematic grouping of content, typically with a heading”.
* `<figure>`: A marked-up photo, with a caption. `<alt>` is used for accessibility reasons.
* `<canvas>` - [is a 2D drawing API added to HTML5, where you can draw anything directly without Flash or Java.](https://codepen.io/mi-lee/post/an-overview-of-html5-semantics)

### CSS
#### Layout
##### Display values
[Exhaustive list of display values](https://developer.mozilla.org/en-US/docs/Web/CSS/display). Every element has a default display type, but can be overridden (usually area a `block`-level element or `inline` element).
* **Standard block-level elements**: `<div>`, `<p>`, `<form>`, `<header>`, `<section>` (stretches as far as can)
* **Standard inline element**: `<span>` (flow of the page/paragraph is not disrupted)
* **none** (another common display value): `<script>`. Commonly used in JS to hide and show elements without deleting and recreating them. Renders them as if they don't exist (VS `visibility: hidden` - hides the element but takes up space)
* `inline-block` elements  are like `inline` elements but they can have a width and height. (Like floated elements except no need to use `clear` on subsequent elements) NB:
 * are affected by the `vertical-align` property, which you probably want to set it to the top.
 * You need to set the width of each column
 * There will be a gap between the columns if there is any whitespace between them in the HTML

##### Layout techniques
* [Centering in CSS](https://css-tricks.com/centering-css-complete-guide/) <- this is great, states which method to use based on element structure.
* Horizontal vertically: use `max-width` & `margin: 0 auto`
* `box-sizing` is used to negate the effects of _the box model_ (where the element's border and padding stretch beyond the specified width). Set:
>  [universal selector] {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}
* `column` (new) to make [multi-column text](http://learnlayout.com/column.html)

###### `position` property
* `position: static`: elements are _not positioned_ in any special way (elements with set position = _positioned_)
* `position: relative`: behaves same as `static` unless you add some extra properties. Setting the `top`, `right`, `bottom`, and `left` properties of a relatively-positioned element will cause it to be adjusted away from its normal position (thus can overlap with other elements). Other content will not be adjusted to fit into any gap left by the element. Leaves a gap (as not removed from the natural flow of the page).
* `position: fixed`- positioned relative to the viewport (there even if scroll). Doesn't leave a gap - removes from the flow.
* `position: absolute` - behaves likes `fixed` except relative to the nearest _positioned_ ancestor instead of relative to the viewport. If no _positioned_ ancestors, uses document body and moves along with page scrolling.

###### `float`ing
* `float` is intended for wrapping text around images.
* `clear` property can be used to arrange sections to appear after floated elements (e.g. `clear: left` clears elements floated to the left).
* **clearfix hack**: using `.clearfix {
  overflow: auto;
}` on outer containers means it stretches to enclose floated elements as well.

###### `flexbox` layout model_
Can be used for [vertical alignment or to have certain elements fill the rest of the space](http://learnlayout.com/flexbox.html).

#### CSS Specificity
* If two selectors apply to the same element, the one with higher specificity wins.
* There are four distinct categories which define the specificity level of a given selector: inline styles, IDs, classes, attributes, and elements. ID > attribute. A class selector > element selectors.
* Measuring specificity: Start at 0, add 1000 for style attribute, add 100 for each ID, add 10 for each attribute, class or pseudo-class, add 1 for each element name or pseudo-element. OR Count the number of ID attributes in the selector (= a). Count the number of other attributes and pseudo-classes in the selector (= b). Count the number of element names and pseudo-elements in the selector (= c). Concatenating the three numbers a-b-c gives the specificity. [Specificity calculator!](https://specificity.keegan.st/)
 * So for link styling: you’re best off putting your styles in the order “link-visited-hover-active”, or “LVHA” for short.”
* Inherited values and universal selector has specificity of 0, 0, 0, 0.

#### Em vs Rem
* Both `em` and `rem` are flexible, scalable units which are translated by the browser into `px` values.
* `rem`: the pixel size they translate to depends on the font size of the root element of the page, i.e. the `html` element/browser font size settings. That root font size is multiplied by the `rem` number. Use for most things, including font sizes.
* `em` units, the `px` value you end up with is a multiplication of/relative to the font size on the element being styled/on which they are used. This font size is influenced by inheritance from parent elements unless explicitly overridden with a unit not subject to inheritance. Useful for `padding`, `margin`, `width`, `height` and `line-height`. Use it for: Any sizing that should scale depending on the font-size of an element other than the root.
 * The font size of the element they’re used on should be set in `rem` units to preserve scalability but avoid inheritance confusion.
* Every element automatically inherits the font size of its parent. NB: careful of the scaling effects with multiple wrapped containers


### DOM Manipulation
* According to the W3C HTML DOM standard, everything in an HTML document is a node:
 * The entire document is a document node
 * Every HTML element is an element node
 * The text inside HTML elements are text nodes
 * Every HTML attribute is an attribute node (deprecated)
 * All comments are comment nodes

#### Walking the DOM to find any node
* Finding a node on a page. Function: `document.getElementById('elementId')` - takes a string and returns the DOM element with that ID.
* If the node doesn't have an ID (e.g. text nodes). Each node has a `childNodes` property that contains an ordered array of all its children (so can index into this array). e.g. `document.getElementById('stars').childNodes[0]`
 * Can use `firstChild` and `lastChild` to find first and last child nodes.
* Nodes also have `previousSibling` and `nextSibling` properties that allow one to slide sideways on the DOM tree.
* `parentNode` - returns the node that encloses the current node.
* NB: `firstChild`, `lastChild`, `previousSibling`, `nextSibling`, and `parentNode` return `null` if there so no such child/sibling/parent node. `childNodes` array is of length zero if there are no child nodes.

#### Manipulating the DOM
* Simplest way is to change the DOM is to assign some HTML to an element's `innerHTML` property. E.g. `element.innerHTML = 'Hello<br>World'` BUT DO NOT USE
* Every node has a method `removeChild` that takes as an argument the node that is to be destroyed. Note that this method needs to be called on the offending node's parent. E.g. `node1.removeChild(node2)` - where node1 is the parent node and node 2 is a child within node 1.
* Nodes may be added as children of another node with the `node1.appendChild(node2)` method. This method is called on the new parent and takes the child as its argument. If the child you are appending is an existing element in the DOM, it will be automatically removed from its existing location. Any grandchildren will follow the child.
* Creating a new text node is easy, use just call `document.createTextNode('hello')` (then use `appendChild` to add to DOM)
* New elements are created by calling `document.createElement('SPAN')` with the argument being the element's HTML tag name. (Use `appendChild` to add to DOM)
