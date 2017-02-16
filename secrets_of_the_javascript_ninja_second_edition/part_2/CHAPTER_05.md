# Chapter 5. Functions for the Master: Closures and Scopes

Topics Covered:

- Simplifying development with closures
- Tracking JS execution with _execution contexts_
- Tracking variable scopes with _lexical environments_
- Understanding variable types
- Exploring how closures work

## Do You Know? (Pre-Chapter Knowledge Check)

1. How many different scopes can a variable or method have, and what are they?

    -

2. How are identifiers and their values tracked?

    - 

3. What is a mutable variable, and how do you define one in JavaScript?

    -

----------

## 5.1. Understanding Closures

**Closures** allow functions to _access and manipulate variables_ that are external
  to that function, and access _all variables and functions_ that are in scope
  when the function is defined.

Example:
```JavaScript
const outerValue= "samurai";
let later;

function outerFunction() {
  const innerValue = "ninja";

  function innerFunction() {
    assert(outerValue === "samurai", "Outer variable is visible");
    assert(innerValue === "ninja", "Inner variable is visible");
  }

  later = innerFunction;
}

outerFunction();  // Calls the outerFunction which sets 'later' as a reference to innerFunction

later();          // Calls innerFunction through the variable reference
```

Our existing JavaScript knowledge tells us that, when the `innerFunction()` is called
  through the variable `later`, the second `assert` statement should fail because
  the scope within which `innerValue` was defined no longer exists. **BUT IT WORKS.**
  Why? **Welcome to the wonderful world of closures, amigos!**

By declaring `innerFunction` within the body of `outerFunction`, a _closure_ is created
  which encompasses the _function defintiion_ as well as _all variables in scope **at
  the point of function definition**_. Any time `innerFunction` is executed it has access
  to the _original scope in which it was declared_. Closures effectively create a "safety
  bubble" of the function and variables _in scope at the point of function definition_ that
  exists as long as the function is available.

Closures **are NOT** visible structures; you can't log out a "closure object" or anything
  of that sort. However, this does not imply that a function which accesses information 
  via a **closure** doesn't carry overhead; _these functions essentially have a "ball
  and chain" attached in order to carry the closure information_. The information is held
  **in memory** until the JavaScript engine can be sure that the information is no longer
  needed, or until the page unloads.

----------

## 5.2. Putting Closures To Work

### 5.2.1. Mimicking Private Variables

JavaScript doesn't have any sort of native support for **private variables** - 
  _properties of an object that are hidden from external parties_. However,
  by using **closures**, we can achieve a similar effect.

Example:
```JavaScript
// Create a constructor function
function Ninja() {
  let feints = 0;   // Variable's scope is limited to the function body (private variable)

  this.getFeints = () => feints;      // 'get' method provides access to value of private variable
  this.feint = () => { feints++; };   // Incrementor method for value of private variable
};

let ninja1 = new Ninja();
ninja1.feint();
assert(ninja1.feints === undefined,   // Confirms private variable is, in fact, private
    "Private data is inaccessible");
assert(ninja1.getFeints() === 1,      // Confirms value of private variable is accessible via getter
    "Internal feint count is accessible via function method");

let ninja2 = new Ninja();
assert(ninja2.getFeints() === 0,
    "New instance of ninja object has it's own 'feints' variable)";
```

In the example above, we've created a _constructor function_ with a variable `feints` declared
  & defined _within the scope of the function_, effectively making it a **private variable** within
  any instantiation of the `Ninja` constructor. Within the constructor function we've also defined
  an _accessor method_, or _getter_, to provide access to the _value of the variable_, and an
  _implementation method_ to provide _control over the value of the variable_. It's not possible to
  simply target the `feints` property of a Ninja object and set the value to whatever we want; we would
  have to call upon the `feint` _implementation method_ to change the value of `feints`. By using
  a closure, we can maintain the state of any Ninja object without providing _direct access_
  to a user of the method.

~~Insert OOP hype here~~

### 5.2.2. Using Closures With Callbacks

When we're dealing with callbacks, we often need to access external data.
