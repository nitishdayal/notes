# Chapter 1.JavaScript is Everywhere

Topics Covered:  
- JS core features
- Core items of JS engine
- 3 best practices of JS development


### Do You Know? (Pre-Chapter Knowledge Check)

1. What are Babel and Traceur, and why are they important to today's JavaScript developers?
     - Babel/Traceur are JavaScript _transpilers_. They allow JS developers to take advantage of upcoming
     JS standards such as ES6/ES7 in their applications right away, and then **translate** and **compile** the
     future-proof code into a modern standard (ES5) that browsers can understand. 
2. What are the core parts of any web browser's JavaScript API used by web applications?
    - DOM (Document Object Model): Web applications generally define their content within HTML **documents**, which
    are sent to a browser containing instructions for **modeling objects** to be displayed to the user.
    - Events: Events. Basically anything. Something happening or not happening, either intentionally or unintentionally. 
    As long as you can 'listen' for the event, you can use it in your program.
    - Browser API: API provided by the browser which provides device information, local storage capabilities, remote
    server communication, etc.


### 1.1. Understanding the JavaScript language

JS developers tend to stagnate in terms of growth after gaining a firm grasp on the fundamentals. The authors guess
 that these developers might come from another language more along the lines of C which provided 'the impression 
 of familiarity.'

> “People often feel that if they know C# or Java, they already have a pretty solid understanding of how JavaScript 
works. But it’s a trap! When compared to other mainstream languages, JavaScript is much more functionally oriented.”

Excerpt From: John Resig  
Bear Bibeault  
Josip Maras. “Secrets of the JavaScript Ninja, Second Edition.” iBooks.

Differences between JS & C-like languages:
- First-class functions: In JS, functions are treated as first-class objects. Benefits discussed in Chapter 3.
- Closures: Closures allow for functions to refer to a variable or set of variables and update the value as
needed every time it's called upon.
- Scopes: Block-level scopes are a recent addition to JS. Previously, JS development relied solely on global
variables and function-level variables.
- Prototypical OOP: **THIS IS THE BIG ONE FOLKS**. JavaScript is not Java. It does not rely on class-based
inheritance. JavaScript uses prototypes. Deep dive later. You'll see(or I suppose...I'll see).

To have a strong foundation in any form of JS development, regardless of platform, you must have a deep
understanding of the relationship between objects and prototypes, and functions and closures.

Additional features for writing efficient code:
- Generators: Functions capable of generating multiple values per request, and suspending in between requests.
- Promises: Provide more control over asynchronous code.
- Proxies: Limit/control access to certain objects.
- Advanced array methods: Hooray! We can do #justprogrammingthings with JavaScript!
- Maps & Sets: Dictionary collections and unique collections, respectively.
- Regular expressions: Regex support is always a good thing. Mo support mo betta.
- Modules: **THIS IS THE BIG ONE**. Break up those huuuuuuuuuge packages and applications into small, self-contained,
maintainable pieces of code and use them as needed. 

### 1.1.1. How will JavaScript evolve?

- ES2015/ES6: We're finally getting to a point where modern browsers support ES6 syntax! But there are still
many people on machines with older or outdated browsers. _Hence transpilers_. Big jump from ES5.
- ES2016/ES7: The beginning of yearly incremental changes to the JS spec. More like fine-tuning the language as 
opposed to a complete rewrite.

Changes to the spec rely on JavaScript engines