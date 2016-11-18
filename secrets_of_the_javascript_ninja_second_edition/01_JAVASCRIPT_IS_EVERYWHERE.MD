# Chapter 1.JavaScript is Everywhere

Topics Covered:

- JS core features
- Core items of JS engine
- 3 best practices of JS development

## Do You Know? (Pre-Chapter Knowledge Check)

1. What are Babel and Traceur, and why are they important to today's JavaScript developers?
     
     - Babel/Traceur are JavaScript _transpilers_. They allow JS developers to take advantage of upcoming
     JS standards such as ES6/ES7 in their applications right away, and then **transform** and **compile** 
     the future-proof code into a modern standard (ES5) that browsers can understand.

2. What are the core parts of any web browser's JavaScript API used by web applications?
    
    - DOM (Document Object Model): Web applications generally define their content within HTML 
    **documents**, which are sent to a browser containing instructions for **modeling objects** to 
    be displayed to the user.
    - Events: Events. Basically anything. Something happening or not happening, either intentionally 
    or unintentionally. As long as you can 'listen' for the event, you can use it in your program.    
    - Browser API: API provided by the browser which provides device information, local storage 
    capabilities, remote server communication, etc.

----------
## 1.1. Understanding the JavaScript Language
----------

JS developers tend to stagnate in terms of growth after gaining a firm grasp on the 
  fundamentals. The authors guess  that these developers might come from another language
  more along the lines of C which provided 'the impression of familiarity.'

> “People often feel that if they know C# or Java, they already have a pretty solid 
> understanding of how JavaScript works. But it’s a trap! When compared to other 
> mainstream languages, JavaScript is much more functionally oriented.”  

Excerpt From: John Resig, Bear Bibeault, Josip Maras.  
“Secrets of the JavaScript Ninja, Second Edition.”

Differences between JS & C-like languages:

- First-class functions: In JS, functions are treated as first-class objects. 
    Benefits discussed in Chapter 3.
- Closures: Closures allow for functions to refer to a variable or set of variables 
    and update the value as needed every time it's called upon.
- Scopes: Block-level scopes are a recent addition to JS. Previously, JS development
    relied solely on global variables and function-level variables.
- Prototypical OOP: **THIS IS THE BIG ONE FOLKS**. JavaScript is not Java. It does 
    not rely on class-based inheritance. JavaScript uses prototypes. Deep dive later.
    You'll see (or I suppose...I'll see).

To have a strong foundation in any form of JS development, regardless of platform, 
  you must have a deep understanding of the relationship between objects and prototypes,
  and functions and closures.

Additional features for writing efficient code:

- Generators: Functions capable of generating multiple values per request, and 
    suspending in between requests.
- Promises: Provide more control over asynchronous code.
- Proxies: Limit/control access to certain objects.
- Advanced array methods: Hooray! We can do #justprogrammingthings with JavaScript!
- Maps & Sets: Dictionary collections and unique collections, respectively.
- Regular expressions: Regex support is always a good thing. Mo support mo betta.
- Modules: **THIS IS THE BIG ONE**. Break up those huuuuuuuuuge packages and 
    applications into small, self-contained, maintainable pieces of code and use them as needed. 

### 1.1.1. How will JavaScript evolve?

- ES2015/ES6: We're finally getting to a point where modern browsers support ES6
    syntax! But there are still many people on machines with older or outdated browsers.
    _Hence transpilers_. Big jump from ES5.
- ES2016/ES7: The beginning of yearly incremental changes to the JS spec. 
    More like fine-tuning the language as opposed to a complete rewrite.

To take advantage of the new features introduced in ES6, ES7, and any future 
  updates to JavaScript, developers rely on _JavaScript engines_ to update and 
  support the latest standard. Given that ES6 was finalized and	released in 2015 
  and we're only now getting to a point where it's becoming *nearly* fully supported 
  in *most* major browsers, this usually means a JS developer has one of two options:

- Wait for the latest spec to gain more widespread support, and continue to program using the currently
    supported standard.
- Make applications that only support the latest and greatest browsers (effectively alienating a huge chunk
    of the market).
- Learn about those new features and find ways to implement them in daily programming by _transpiling_.

### 1.1.2. Transpilers Give Us Access to Tomorrow's JavaScript Today

While the update between ES5 to ES6 was a tedious process for most browsers, upcoming changes to the JS
	spec should be far more incremental in nature, allowing for browsers and JS engines to be updated at
	much more frequent speeds. However, most users of JS applications won't be using the latest-and-greatest
	in web browser technology, and in order to support those users we need a way to _tranform and compile_ 
	our code from	modern JS syntax to a version supported by older browsers.

----------
## 1.2 Understanding the Browser
----------

The book's primary focus is on JavaScript development for the browser, but it does note that JS can now
	be used to code in a multitude of environments. A browser consists of:

- Infrastructure:

	* CSS - Cascading Style Sheets (Appearance)
	* JS - JavaScript (Interactivity)
	* HTML - HTML (Content)

- API (Application Programming Interface):

  * DOM (Document Object Model) - Structural representation of the client-side UI for a
      web application. Initially built based upon instructions provided by the application's
      HTML page. DOM construction is covered in Chapter 2, effective DOM manipulation in 
      Chapter 3
  * Events: **Damn near anything.** Mouse cick? Event. Internet die? Event. Change focus?       Event. Etc.
  * Timers: Apparently an underused part of JavaScript. Can be utilized for all sorts
      of things. Like. Events. And DOM manipulation. And setting off other timers. Etc.


> Develop a comprehensive strategy for tackling (browser-specific quicks and) issues,
> and become intimately familiar with their differences and quirks... [This] can be
> almost as important as proficiency in JavaScript itself.

Excerpt From: John Resig, Bear Bibeault, Josip Maras.  
  “Secrets of the JavaScript Ninja, Second Edition.”

----------
## 1.3. Using Current Best Practices
----------

Understanding the language and the browser make up the fundamentals of JS development.
  Moving past the fundamentals requires a knowledge of:

- Debugging
- Testing
- Performance Analysis

### 1.3.1. Debugging

**What debugging JS apps used to look like:**  "I need to check that value...so 
  I guess I'll log that out....and this one...so I guess...an alert there....and
  ....a log in this one too..."

Enter JS debugging & dev tools (thank you Firebug team <3). Pick one! They kind of all do 
  the same thing! Damn near look the same too! (I like Safari. Yeah. You read that correctly.)Browser-provided JS debugging tools provided the ability to explore the DOM, and make live changes
  to our code while providing immediate feedback, and are valuable tools for anyone involved in
  browser-based development. Some (such as chrome dev tools) even provide Node.js debugging.

### 1.3.2. Testing

The simplest way to hop into JS testing and TDD (Test Driven Development) is to use the `assert`
  function. `assert` functions return a `true` or `false` boolean value corresponding to the
  validity of a provided premise or argument. Loaded sentence breakdown: You give it a thing you're
  expecting from your program, it tells you if it's true or false, however it can figure that out. 
  If it can't figure it out, you're probably gonna get a false.

  An `assert` function is laid out like so:  
  `assert(condition, message);`

  The first parameter is the condition the assert function is expecting to be true, and the
  second parameter is a string message to be displayed if the condition returns false.
  
  Many web browser's built-in debugging tools provide a `console.assert()` method, and
  an `assert()` function exists in the Node runtime environment, but vanilla JS does not
  have any such built-in. If we want to take advantage of assert functions for testing, we
  write them ourselves.

### 1.3.3. Performance Analysis

JS engines have gotten smarter and faster. That's not a reason to write sloppy, poor code.
  A program's performance is generally measured in _time_. JavaScript provides the built-in
  methods `console.time(...)` and `console.timeEnd(...)` which start and stop a timer, 
  respectively. The methods take one argument, a string, which is then used as the name of
  the timer. When `console.timeEnd()` is called, it logs the name of the timer as well as the
  amount of time taken for the process to complete.

----------
## 1.4. Boosting Skill Transferability
----------

Websites aren't websites any more, they're web _apps_. Web apps are becoming progressive apps. 
  The browser is no longer just for the web. As the world of software development and web development
  start to draw influence from one another, the tools and methodologies start carrying over as well. 
  This, however, is not a two-way road. A deep understanding of JavaScript and its core APIs will
  open up the ability to develop "almost any type of application imaginable". Cross-platform desktop
  apps can be built using Electron, NW.js, and various other technologies which allow the devleoper to
  write applications in pure JavaScript but have it run on any desktop operating system. Mobile 
  applications can be created with a variety of frameworks and tehcnologies, such as Cordova or
  Ionic. And of course, server-side and embedded devices support Node.js application development. And
  not only does the language carry over, the methodology embraced by standard client-side web apps
  carries over too.

----------
## 1.5. Summary
----------

_ Take time to understand the layout of client-side web apps. The ideas and tools carry over to 
    an extensive collection of platforms.
- Deep-dive into the core mechanics of JS (browser APIs, events, timers, DOM) as well as 
    browser-provided Infrastructure.
- The focus of the book is on core JS mechanisms (functions, closures, prototypes) and 
    new features (generators, promises, proxies, maps, sets, and modules).
- JS applications run in a variety of environments. It's home, however, is the browser.
- Client-side applications are _event driven_, so we'll learn about events. All. The. Events.
- **JavaScript Best Practices:** debugging, testing, perfomance analysis.


