# Chapter 3. First-Class Functions for the Novice: Definitons & Arguments

Topics Covered:

- Importance of understanding JavaScript functions
- What it means for a function to be a 'first-class citizen'
- Define a function
- Secrets of parameter assignment

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

JavaScript objects are capable of:

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

We can _define_ functions in JavaScript using **function literals**. A function literal
  will create a _function value_ similar to how numeric or string literal values are
  created by numeric or string literals. This is important to understand; once again,
  we are seeing how functions are _first class citizens_ in JavaScript; they can be
  used in JavaScript anywhere that literals are used.

There are a few ways to define JavaScript functions. The author breaks them down into
  four categories:

  - Function declatarions & function expressions: The most common ways of defining
    functions. While they are quite similar, understanding the differences is key
    to understanding when and why a function is available for invocation.

    Ex (declaration): `function myFunc() { }`
    Ex (expression): `let a = function() { }`

  - Arrow/lambda functions: Introduced with the ECMAScript 2015 (ES6), arrow functions
    allow us to define functions with much less syntax, and much more clarity. Arrow
    functions also allow us to handle scope and the `this` situation. I'll leave it at that.

    Ex: `myArg => myArg*2`

  - Function constructors: In object-oriented languages like Java and Python, we can create
    new instances of objects/classes by using the `new` keyword prior to calling
    the object/class name. Given that JavaScript can take great advantage of _functional
    programming principles_, we can create new instances of functions in a similar fashion.

    Ex: `new Function('a', 'b', 'return a + b');`

    This will create a new function which will take two parameters (a and b) and return the
    sum value.

  - Generator functions: Another new feature introduced in ECMAScript 2015 (ES6), generator functions
    differ from ordinary functions in many ways. Generator functions allow the user to exit
    and re-enter the function as necessary while maintaining it's variables & values. Generator
    functions are actually just a _version_ of a normal function; as such, we define
    them in the same way we would define _function declarations, function expressions, and function 
    constructors_. The only difference is an asterisk(*) after the `function` keyword.

    Ex: `function* myGen() { yield 1;}`

This book covers function declarations and expressions, arrow functions, and generators. **FUNCTION   
  CONSTRUCTORS ARE NOT COVERED**; they present an interesting capability to dynamically create functions
  and evaluate code, but is more of a 'corner feature of the JavaScript language' (pg. 78)

### 3.3.1. Function Declarations and Function Expressions

**Function declarations** are the most basic way of defining functions in JavaScript; the `function`
  keyword begins the definition and is followed by a name for the function. After this we create
  opening-and-closing parenthesis `()` which will contain a list of comma-separated parameters, and
  finally we define the body of the function within opening-and-closing braces `{}`. Function
  declarations _must be placed on their own, as a separate JavaScript statement, but can
  live within another function or block of code_.

  Ex: 
  ```javascript
  function nameOfAFunction(firstParamOrArgument, secondParamOrArgument) {
    // Function body goes here

    myStatement1;
    myStatement2;
  }
  ```

**Function expressions** are functions that are _always part of another statement_ (the value
  of an assignment expression, argument passed to another function, etc.). This provides great
  flexibility for us, because they allow us to create functions when and where we need them,
  allowing for our code to be grouped by concern and makes it easier to understand.

Ex:
```javascript
function functionDeclaration() {      // Standalone function declaration
  function innerFuncDeclaration(){};    // Inner function declaration
}


var funcAsVar = function(){};         // Function expression as variable declaration + assignment


funcAsVar(function(){                 // Function expression passed as argument to function call 'funcAsVar'
  return function(){};                // Function expression as a return value
});


(function namedFunctionExpression(){  // Named function expression inside an IIFE
})();


+function(){}();                      
-function(){}();                       // Function expressions that will
~function(){}();                       // be invoked immediately as arguments
!function(){}();                       // to unary operators
                                        
```

The first function is an example of how a function declaration can be contained
  within another function's body, but _is a separate statement_. The `funcAsVar` variable is 
  an example of a function as a part of another statement; in this case, it is a variable
  assignment. The next example builds on top of the previous function; we _invoke_ `funcAsVar` and
  pass an unnamed function as an argument, and we return another, separate unnamed function.

The primary difference between function declarations and function expressions is that
  function declarations **require** a name, where as function expressions have optional
  naming.

The last few examples are examples of _immediately invoked function expressions_.

**Immediate functions**, immediately invoked function expressions, or _IIFEs_, are exactly
  what they sound like; functions that are invoked immediately after they're defined. They
  are generally contained in a way that they do not effect the global namespace 
  (`namedFunctionExpression` example), or are defined without names. Immediate functions
  allow us to mimic modules in JavaScript. This topic will be discussed in futher
  detail in Chapter 11.

Immediate functions are wrapped in parenthesis so that the browser's JavaScript
  parser can differentiate between function declarations and function expressions.
  If the function expression is not contained within parenthesis it appears
  to the JavaScript parser as a function declaration; given that function
  declarations require names and immediate function expressions are generally
  anonymous, the parser will throw an error.

Both of the following methods are valid ways of declaring immediate functions:

- `(function(){})(3)` - Declaring the function expression, wrapping it in parenthesis,
    and following it with another set of parenthesis to call it immediately and pass in
    3 as an argument.

- `(function(){}(3))` - Declaring the function expression, calling it immediately with
  a set of parenthesis and passing in 3 as an argument, and wrapping the entire function
  expression and call in parenthesis.

### 3.3.2. Arrow Functions

**Arrow functions** allow JavaScript developers to create functions in a short
  and succinct manner. In many ways, arrow functions can be viewed as _simplified
  function expressions_, in that _they do not need the `function` keyword, braces, 
  or `return` statements_ explicity defined.

For example, we can take the example code from section 3.1.2 where we used
  the `sort` method and passed in a function expression as a callback, and rewrite
  it to take advantage of arrow function syntax:

```javascript
var values = [0,3,2,5,7,4,8,1];

values.sort((val1, val2) => val1 - val2);
```

So fresh. So clean.

The syntax of an arrow function can be broken down like so:
1. Declare parameter or parameters. If there are multiple parameters (or none), wrap 
    them in parenthesis: `param` or `(param1, param2)`
2. The 'fat arrow' operator : `param => ` or `(param1, param2) =>`
3. Return expression **OR** standard function body:
  ```javascript
  param => expression // Equivalent of 'return expression' in standard function
  (param1, param2) => expression
  (param1, param2) => { 
    aStatment;
    anotherStatement;
    return returnStatement;
  }
  ```

If the purpose of the arrow function is just to return something, there is no need to create
  a function body in brackets; if, however, more advanced calculation needs to be done, or
  the function should not return a value, use brackets to specify a standard function body. **If
  you create a function body, you must specify the value to be returned if there is one.**

----------

## 3.4. Arguments and Function Parameters

**TLDR;** Arguments and parameters aren't the same thing. Stop interchanging the terminology.
  Just. Stop it. No bueno.

**Parameter:** A variable listed as part of a function definition  
**Argument:** A value passed into a function upon invocation

If a function invocation is provided with arguments, they are assigned to the parameters
  _in the order specified by the function definition_. For example, the first argument
  of a function invocation would match up with the first parameter in the function
  definition, the second argument with the second parameter, and so forth and so on.
  If more arguments are provided than there are parameters, JavaScript will avoid
  assigning them to parameter names. If there are less arguments than parameters, JavaScript
  will assign the parameters that weren't given arguments as `undefined`.

### 3.4.1. Rest Parameters

If we are defining a function that will be receiving an unknown amount of arguments, but
  we want to be able to reference all of them, we can take advantage of ES6 **rest parameters**.
  Rest parameters are best understood through example:

```javascript
function addMax(first, ...restOfNumbers) {
  let sorted = restOfNumbers.sort((a, b) => b - a;);

  return first + sorted[0];
}

addMax(3, 7, 6, 13, 4, 9, 21, 5);
```

In the above example we have defined a function `addMax` and given it the parameters `first`
  and `...restOfNumbers`. `...restOfNumbers` is the _rest parameter_ in this function; rest
  parameters must be the last parameter in a function definition and are preceded by 
  ellipsis (...). When we call the `addMax` function and provide it multiple arguments, the first
  argument is assigned to the `first` parameter while the rest of the arguments are assigned
  to `...restOfNumbers` in a new array. After sorting the restOfNumbers array in descending order,
  we add the argument assigned to the `first` parameter (in this case, the number 3) to the largest
  value in our sorted array, which would be at index 0, and return the sum.

### 3.4.2. Default Parameters

Not really much to explain here. ECMAScript 2015 (ES6) brings support for default parameters;
  if you're going to be calling a function multiple times and passing in the same argument,
  might as well just define it in the function as a default and then override it if it needs
  to be something different. More concise, less repetition of logic, more flexible, easier
  to read...all that good stuff.

Example:
```javascript
/** The following function has two parameters, 'firstParam' and 'secondParam'.
  * The latter has been provided with a default value "hello there!"
  * The function will return a string.
  */
function withADefaultParameter( firstParam, secondParam = "hello there!") {
  return firstParam + " says " + secondParam;
}


// No second argument provided, return value is 'Nitish days hello there!'
withADefaultParameter("Nitish")

// Second argument provided, will replace secondParam's default value and return 'Leo says Woof".
withADefaultParameter("Leo", "Woof")
```

**Any value can be a assigned as a default parameter**; primitive data types like numbers
  or strings, complex types like objects, arrays, or functions, etc. The values are
  evaluated from left to right; as such, any parameter that is preceded by any other parameter
  can take advantage of that parameter's assigned value. Weird and wordy I know. But it's true.
  Read it until it starts making sense. I swear it's real. **HOWEVER**, this technique isn't
  particularly recommended. It takes away from code readability and doesn't really provide
  any benefit; maybe three or four words less that you have to type. Not worth.

----------

## 3.5. Summary

- JavaScript should be learned as a _functional language_ if one wishes to be able to write
  sophisticated code.
- In JavaScript, _functions are first-class citizens/objects_. Top dog of JS world.
  All hail our functional overlords.
- Callback functions (functions that other code _calls back_ at a later time) are
  frequently utilized in JavaScript development, particularly for event-handling.
- Since functions are first-class objects, they can contain properties and attributes
  just like any other object. This opens up another layer of functionality:
    - Store child functions within a parent function for future reference/invocation
    - Create a cache (memoization) to avoid repetitive or unnecessary computations.
- The different types of functions are: **function declarations, function expressions,
  arrow functions, and function generators.
- Function declarations _must be named and be placed as separate statements_. Function
  expressions don't require a name, but _must be a part of another code statement_.
- Arrow functions are prettier, cleaner, nicer, and there's no reason not to use them. Use them.
- **Parameters** are variables listed in a _function definition_, while **arguments** are values
  passed to _function invocation_.
- The amount of parameters and arguments provided for a given function do not need to match:
  - If there are more parameters than arguments, the additional parameters are given the value
    `undefined`.
  - If there are more arguments than parameters, the extra arguments aren't bound to the
    parameter names.
- **Rest parameters** and **default parameters** were introduced with ECMAScript 2015 (ES6):
  - Rest parameters give us a way to handle extra arguments; they are placed in a new array
    and can be referenced by the name given to the rest parameter.
  - Default parameters give us a way to specify a value for a parameter in the _function definition_
    which will change very rarely; to override the default value, we simply provide our own in the
    _function invocation_.

----------

## 3.6. Exercises

1. In the following code snippet, which functions are callback functions?
    ```JavaScript
    numbers.sort(function sortAsc(a,b){
      return a - b;
    })

    function ninja(){}
    ninja();

    var myButton = document.getElementById("myButton");
    myButton.addEventListener("click", function handleClick(){
      alert("Clicked");
    })
    ```

    Answer: sortAsc() and handleClick() are callback functions as they are being utilized
      by another piece of code and are called upon as needed. ninja() is not a callback;
      it is directly invoked at one specific instance and we know that it will not be called
      again.

2. In the following snippet, categorize functions according to their type (function declaration,
  function expression, or arrow function).

  ```JavaScript

  // sortAsc() is a function expression
  numbers.sort(function sortAsc(a,b){
    return a - b;
  });

  // Arrow function
  numbers.sort((a,b) => b - a)

  // Function expression (immediately invoked function expression)
  (function(){})();

  // Function declaration
  function outer(){
    // Function declaration
    function inner(){}
    return inner;
  }

  // Function expression
  (function(){}());

  // Arrow function expression (immediately invoked)
  (() =>  "Yoshi")();
  ```

3. After executing the following code snippet, what are the values of variables
  `samurai` and `ninja`?

  ```JavaScript
  // var samurai has a string value "Tomoe"
  var samurai = (() => "Tomoe")();


  // var ninja is undefined; the arrow function is invoked immediately and does not provide
  // a return value, as such, once the program has completed, the value of ninja will be
  // `undefined`.
  var ninja = (() => {"Yoshi"})();
  ```

4. Within the body of the `test` function, what are the values of parameters `a`, `b`, and `c`
  for the two function calls?

  ```JavaScript
  function test (a, b, ....c) { /*a, b, c*/}

  test(1,2,3,4,5); // A = 1, B = 2, C = [3,4,5]
  test();          // A, B = undefined, C = [];
  ```  

5. After executing the following code snippet, what are the values of the `message1` and `message2`
  variables?

  ```JavaScript
  function getNinjaWieldingWeapon(ninja, weapon = "katana"){
    return ninja + " " + weapon;
  }

  var message1 = getNinjaWieldingWeapon("Yoshi");               // "Yoshi katana"
  var message2 = getNinjaWieldingWeapon("Yoshi", "wakizashi");  // "Yoshi wakizashi"
  ```
