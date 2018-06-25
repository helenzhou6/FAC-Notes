#  Week Fourteen to Sixteen

_Resources:_ [_the london timetable_](https://github.com/foundersandcoders/london-programme/blob/master/weeks-10-16/week-11.md):
[week-14](./week-14.md) - Design Week Project 2
[week-15](./week-15.md) - Project 2 Week 1
[week-16](./week-16.md) - Project 2 Week 2

Project Two - Three Week project (first week Design week and last two weeks Build Sprint). Followed the same format as the first project (so refer to [Week Eleven to Twelve Notes](https://github.com/helenzhou6/FAC-Notes/blob/master/weekEleventoTwelve.md)).

### Mentoring for FAC14
_Resources:_ [_FAC mentorship advice_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/general/mentorship) _and_ [_Hack MD Notes_](https://hackmd.io/x91Jz-ZwTxyA1pIqZVgseQ)

* Will be mentoring FAC14 for week one.
  * [GitHub Cheat sheet (made for FAC14 as part of mentoring)](https://hackmd.io/FAnreC54RT27io6imUqFAQ)
  * [Powerpoint made for week one](https://docs.google.com/presentation/d/1YTIX3ukOXhInTTiqDpJteDLkcaF3qO7ncmiNTqSbmMc/edit#slide=id.p)
  * [Schedule]: [monday](https://hackmd.io/DkIzhSU2T3K68p0zut9VlA), [tuesday](https://hackmd.io/YLoEaX6hQfGri3sXm6Q-zQ), [wednesday](https://hackmd.io/iCyN_74IR6iUgdTPCQ0f5g), [thursday](https://hackmd.io/UQpwd_NXS-mFn_Uj1n16Nw), [friday](https://hackmd.io/W0mJyt1oSc-m5j8YfbVL6w)

### Recursion workshop
_Resource:_ [_recursion workshop_](https://github.com/foundersandcoders/mc-recursion)

Watch this space...

### Project 
MVP made for an external social enterprise ([Juta Shoes](https://www.jutashoes.com/)). 3 week process (1 week design, 2 week build sprint). Links: [Craft Track - front end](https://github.com/fac-13/craft-track) and [Craft Track - back end](https://github.com/fac-13/crafttrack-server) - a tracker for shoes to be made for Juta Shoes.

* Used Reach Router to handle page changes

### Interview practice
[Interview tips](https://github.com/foundersandcoders/interview-tips)
[Some interview questions](https://hackmd.io/TUVsqHMvReq6UN9pDG7QFw)

### Web performance
_Resource:_ [_Notes for the talk here_](https://slides.com/sofiapoh/webperf101#/)
* Understand what and when to optimise
  * First load above the fold - read more [in this article](https://blog.stackpath.com/glossary/critical-rendering-path). 
  > Tooling like webpack can help you create your above the fold content programmatically.
  * Use svgs > imgs.
  * Use image/svg optimisation software
* Understand how your assets contribute to the page weight
  * Leverage tooling to minfiy & uglify your production code
  * Load only the code you need, use a bundler - lazy load the code
  * Less request to 3rd parties (e.g. use a sprite sheet for icons)
  * Cach assets (use a CDN)
* Understand how browsers render content
  * Look at **critical render path** - where running JS done in between building DOM and building CSSOM - look [at this article for more](https://bitsofco.de/understanding-the-critical-rendering-path/)
  * E.g. **Paint** stage (render pixels to screen) - gradients and box shadows take longer to paint
  * SO do this:
  ```js
  // no extra attributes
  // will block parsing while fetching script and immediately executes 
  <script src="main.js"></script>

  // async attribute: don't block DOM parsing while fetching this script
  // will still block parsing once fetching is done  
  <script async src="main.js"></script>

  // defer attribute: don't block DOM parsing while fetching this script
  // will wait until DOM content is parsed before executing the script
  <script defer src="main.js"></script>
  ```
* Understand what perceived performance is - how quick user thinks your site is
  * Instant feedback (e.g. button that give the illusion pressing down) / use animations / placeholders and spinners (though use with care - sometimes can perceive as longer & need pixel to pixel matching)

### Accessibility talk
_Resources:_ [_Talk on accessibility here_](https://github.com/oliverjam/intro-to-accessibility) _and_ [_the slides here_](https://a11y-intro.netlify.com/#0). _Talk based on these resources:_ [_WAI-ARIA basics - mozilla_](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics), [_inclusive components - blog/pattern library_](https://inclusive-components.design/), [_the accessibility cheatsheet_](https://bitsofco.de/the-accessibility-cheatsheet/) _and_ [_gov.uk article on accessibility_](https://accessibility.blog.gov.uk/)

* Don't rely solely on colour 
* Scale UI based on font-size (use `rem`and don't hard code boxes - minimum height of button should be -44px?)
* Show things in context (e.g. error messages near to the relevant input box) and logical content flow (when tab through - shouldn't jump around a loy)
* Autocompletes: don't require exact spelling
* The Web Content Accessibility Guidelines are a set of best practices. Look at the [cheat sheet](bitsofco.de/the-accessibility-cheatsheet)
  > * **Perceivable**: Can all users perceive your UI? - e.g. text with sufficient size/contrast/line-height, images with alt-text, alerting screenreaders to dynamic content.
  > * **Operable:** Can all users operate the UI components and navigation? e.g. ensure keyboard functionality, no unnecessary time-limits, descriptive link text, hierarchical headings, unique & descriptive page titles.
  > * **Operable:** Can all your users understand the content and how to use the interface? e.g. use plain English, set the lang attribute, label inputs, provide error messages in context.
  > * **Robust** Can your content be understood by different user-agents/assistive tech? e.g. semantic HTML (and ARIA when this fails you), automated and manual testing.
* **WAI-ARIA** = Web Accessibility Initiative – Accessible Rich Internet Applications - A spec for assigning meaning to otherwise meaningless chunks of markup.
  * ARIA gives meaning to screenreaders when beyond semantic html.
    * Visually obvious content might need a text label. e.g. `<button aria-label="menu button">☰</button>`
    * Roles: Signifies your custom component as a type assistive tech can recognise. e.g. a custom `<select>` using divs could use  `role="listbox"`.
    * Live regions: Ensures dynamically updated content is read out. `aria-live="polite"` will be read once the current info finished,  `aria-live="assertive"` will interrupt.

### Snippets of code
* `this.setState(({ shoeDetails: prevDetails }) => { return { shoeDetails: { ...prevDetails, [e.target.id]: e.target.value }` -> the latter overrides the values set in `prevDetails`
* `this.setState(props, callback)` => second argument takes a callback! (for after state change)
* Can chain ternary statements (like `else if` then `else if`): `type === "shoe" ? shoeDetails : type === "workshop" ? workshopDetails : {};`
* `Array.from({ length: 11 }, (v,i) => 35 + i)` —> to make an array from 35 to 46. Since the array is initialized with `undefined` on each position, the value of `v` below will be `undefined` - look at the example in [Mozilla dev docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
* `[condition] && [condition 2]` is the same as `[condition] ? [condition 2] : ‘’` - called **short circuit evaluations** (though both can return `false`)