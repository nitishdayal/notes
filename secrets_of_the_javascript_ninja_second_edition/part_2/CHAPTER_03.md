# Chapter 3. First-Class Functions for the Novice: Definitons & Arguments

Topics Covered:

- Importance of understanding JavaScript functions
- What it means for a function to be a 'first-class citizen'
- Define a function
- Secrets of assigning parameters

## Do You Know? (Pre-Chapter Knowledge Check)

1. In what situations might callback functions be used synchronously? Asynchronously?

    - Callback functions can be used synchronously to increment the value of a given variable. 
      A situation in which callback functions could be used asynchronously would be
      to hold on to a given value until it's called either by the _user_, the _program_,
      or the _browser_.

2. What's the difference between an _arrow function_ and a _function expression_?

    - An arrow function does not use the function keyword and doesn't bind it's own 
      `this, arguments, super`, or `new.target`.

3. Why might you need to use default parameter values in a function?

    - Default parameter values would be needed in a function if the function requires
      a given parameter to perform, but that parameter isn't always provided by the user
      or the program.

----------
## 3.1. What's With the Functional Difference?

Aside from the global JavaScript code executed during the page-building phase, all
  other JavaScript code will be contained within _functions_ and will be run as a 
  result of _function invocation_. Functions as first-class objects/first-class citizens
  can be greatly exploited to the developer's benefit.

JavaScript objects (and, as such, JavaScript functions) are capable of:

  - Being created via literals: `{}`
  - Being assigned to variables, array entries, or as object properties:
      ```JavaScript
      var ninja = {}; // Assigns a new object to variable 'ninja'
      ninjaArray.push({}); // Inserts a new object into array 'ninjaArray'
      ninja.data = {}; // Assigns a new object as another object's property
      ```
  - Being passed as arguments to functions:
      ```JavaScript
      function hide(ninja) {
        ninja.visibility = false;
      }

      hide({}); // A new object is passed as an argument into the 'hide' function
      ```
  - Being returned as values from functions:
      ```JavaScript
      function returnNewNinja() {
        // Returns a new object from a function
        return {};
      }
      ```
  - Possessing dynamically created/assigned properties:
      ```JavaScript
      var ninja = {};
      ninja.name = "Hanzo"; // Creates a new property 'name' on the 'ninja' object
      ```

### 3.1.1. Functions as First-Class Objects

Aything that a JavaScript object can do, a JavaScript function can do as well. In particular,
  functions are _first-class_ objects/citizens, meaning they can also be:

  - Created via literals: `function () {};`
  - Assigned to variables, array entries, and object properties:
      ```JavaScript
      var ninjaFunction = function() {}; // Assigns a new function to variable 'ninjaFunction'
      ninjaArray.push(function() {});    // Inserts a new function into array 'ninjaArray'
      ninja.data = function() {};        // Assigns a new function as an object property
      ```
  - Passed as arguments to other functions:
      ```JavaScript
      function call(ninjaFunction) {
        ninjaFunction;
      }

      call(function() {}); // Passes a new function as an argument to function 'call'
      ```
  - Returned as values from functions:
      ```JavaScript
      function returnNewNinjaFunction() {
        return function() {}; // Returns a new function when 'returnNewNinjaFunction' is called
      }
      ```
  - They can possess dynamically created and assigned properties:
      ```JavaScript
      var ninjaFunction = function () {};
      ninjaFunction.name = "Hanzo"; // Adds property 'name' to function 'ninjaFunction'
      ```

Once again, just driving home the idea that, in JavaScript, **functions _are_ objects** 
  (specifically, first-class objects) that are capable of being _invoked_ in order to 
  perform a specified action. 

### 3.1.2. Callback Functions

A _callback function_ is a function which is set up to be called at a later time, by either
  the browser during event-handling or by other code. We set up some sort of code that we want
  to "call back" when we need it.

JavaScript allows up to write functions anywhere in our code that an expression can appear, which means
  our code will be more compact and easier to understand since function definitions can be placed near
  where they are used. By taking advantage of callbacks, we can **avoid polluting the global namespace**
  with names that will not be referenced from multiple places.

**NOTE**: The author makes it a point to clarify that this definition of a callback is not globally
  accepted; some believe that a function can only be a callback if it is called asynchronously.

**Sorting w/ a Comparator**

Sorting a collection of data is one of the most common tasks a developer will have to tackle. It's not
  particulary challenging; figure out how you want your collection sorted, select the best sorting
  algorithm suited for the task, avoid introducing bugs into the program, and voila! Of those steps the
  only one that will change from application to application is the sorting order. Collections are 
  generally stored in _arrays_, and in JavaScript all arrays have access to a built-in `sort` method.
  The `sort` method takes one argument: a comparison algorithm in the form of a callback. The `sort`
  method will call upon this algorithm when it needs to compare two values to decide the sorting order,
  passing in subsequent values until none are left.

```JavaScript
var values = [0, 3, 2, 5, 7, 4, 8, 1];

  /* Callback function takes two arguments and returns the value from subtracting the first
   * from the second. The sort method will call this function every time it needs to compare
   * two items from the given array.
   */ 

values.sort(function(val1, val2) {
  return val1 - val2;
})
```

We are able to create a function as a standalone entity, pass it as an argument to a method
  or other function, which is capable of accepting a function as a parameter. 

~~#JustFunctionalProgrammingThings?~~

----------
## 3.2. Fun with Functions as Objects

In JavaScript, functions have many similarities with other object types, such as the ability
  to attach properties:

```JavaScript
// Create an object and assign it a new property
var person = {};
person.name = "Jim"

// Create a function and assign it a new property
var car = function() {};
car.Model = "Volvo"
```

### 3.2.1. Storing Functions

In a given situation where unique functions need to be stored in a collection, the primary 
  obstacle is determining whether a given function is new to the collection. If a callback function 
  is duplicated, each event that corresponds with that function will make multiple calls 
  (depending on the amount of duplicates and nested calls). To avoid duplicates in our collection,
  we can give each function a unique property (for example, an ID) or take advantage of
  a new ES6 object, a `set`, which can hold multiple values but only one instance of each.

### 3.2.2. Self-Memoizing Functions

_Memoization_ refers to a function that is capable of remembering its previously computed
  values. This is done by storing the values in some sort of collection (much like storing
  functions). If a function is expected to run complex computations and potentially provide 
  the same results each time it's called upon, _memoization_ can aid in increasing
  performance by avoiding repetition. As long as the function is alive, its stored cache
  is alive.

Benefits:
- Performance benefits for the end user
- No additional initialization required

Drawbacks:
- Caching sacrifices _memory_ in favor of _performance_
- One function is concerned with too many things **(separtion of concern)**
- Difficult to measure load-test or performance; results can vary drastically

_Choose wisely._

----------
## 3.3. Defining Functions

### 3.3.1. Function Declarations and Function Expressions

### 3.3.2. Arrow Functions

----------
## 3.4. Arguments and Function Parameters

### 3.4.1. Rest Parameters

### 3.4.2. Default Parameters

----------
## 3.5. Summary

----------
## 3.6. Exercises
