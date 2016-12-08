# Chapter 4. Functions for the Journeyman: Understanding Function Invocation

Topics Covered:

- `arguments` and `this` _implicit_ parameters
- Ways of invoking functions
- Dealing with function context-related problems

## Do You Know? (Pre-Chapter Knowledge Check)

1. Why is the `this` parameter known as the _function context_?

    - The `this` parameter is known as the _function context_ because it is used
      to refer to the object on which the function is being invoked upon; for example,
      if we call a method on an array (`array.push()`), the `this` parameter would
      refer to the array on which the `.push()` method was called upon. 

2. What's the difference between a _function_ and a _method_?

    - A _function_ is a stand-alone object, whereas a _method_ is a function which
      is _a property of an object_.

3. What would happen if a constructor function explicitly returned an object?

    - The constructor's newly created object is discarded and the explicitly returned
      object is the value of the `new` expression.

----------

## 4.1. Using Implicit Function Parameters

Function invocation generally passes _two implicit parameters_: `arguments` and `this`.
  These parameters are included in addition to any _explicitly_ declared parameters. We
  can utilize these parameters just like any explicity named parameter to add functionality
  to our code.
  
### 4.1.1. The Arguments Parameter

In any function, the `arguments` parameter refers to _all arguments passed to the function_
  regardless of whether the function has an explicity defined/matching parameter. We can
  utilize the `arguments` parameter to implement _function overloading_ and _variadic 
  functions_.

> **NOTE**: While there is little need to utilize the `arguments` parameter due to the
>   introduction of `rest` parameters in the latest JavaScript standard, it's important
>   to understand the functionality and uses of `arguments` as we're very likely to
>   come across it in legacy code.

`arguments` is an _object_ with a **length** property which indicates _the exact
  number of arguments_. To see the value of any given item in the `arguments` object,
  we use _array indexing notation_. Array indexes begin at '0', so if five arguments
  were passed into a function and we want to see the value of the fifth item,
  we would call `arguments[4]`. **HOWEVER, IT IS NOT AN ARRAY**. `arguments` cannot
  take advantage of any JavaScript built-in array methods. We can think of it
  as an _array-like construct_ in that we can access the values from `arguments`
  in the same way that we would access values from an array, but the similarities
  end there.

The `arguments` object _aliases function parameters_. If a change is made to the
  `arguments` object on an explicitly defined parameter, the parameter's
  value is updated to reflect that change, and vice versa.

**Example**:
```JavaScript
function changeParam(param){
  console.log(param, arguments[0]);       // Hello Hello
  console.log(param === arguments[0]);    // true

  arguments[0] = "Changed value";

  console.log(param, arguments[0]);       // Changed value Changed value
  console.log(param === arguments[0]);    // true

  param = "Meow. Changed again.";

  console.log(param, arguments[0]);       // Meow. Changed again. Meow. Changed again.
  console.log(param === arguments[0]);    // true
}

changeParam("Hello");
```

In the example above, we call the `changeParam` function and pass in a string literal "Hello".
  `changeParam` has one parameter defined, `param`. The first log prints out the value of
  `param` and `arguments[0]`, which should be the same. The second log verifies that they
  are exactly alike. Then we _update the value of the arguments object at index 0_,
  which corresponds with the `param` property (one and only argument passed into the function
  and declared _explicitly_ as a parameter in function declaration) and updates the value
  of `param` to match. We log out the parameter and the argument at index 0 and check
  their truthiness again; they should both reflect the new string "Change value". Finally,
  we _update the value of the `param`_, which consequently updates the value of `arguments[0]`.
  Once again, we log out their values and truthiness to confirm.

We can limit this functionality by including the string `"use strict;"` at the very beginning of
  our JavaScript code. By doing so, we change the JavaScript engine's behavior to _throw errors_
  rather silently picking them, up and remove some 'unsafe' language features. One such feature 
  is `arguments` aliasing.

If we were to run the example again but include `"use strict";` at the beginning of our
  script, the first set of logs would match and be true but the rest would not. Any changes
  to the `arguments` parameter would not update the `param`, nor would any changes to `param`
  have an effect on `arguments` value.

### 4.1.2. The `this` Parameter: Introducing Function Context

Along with `arguments`, a function invocation is also implicitly passed the parameter
  `this`, _a reference to the object that the function was invoked upon_. As such, `this`
  is often referred to as the _function context_.

JavaScript _function context_ varies from **object-oriented** langauges such as Java or C#. 
  In OO languages, `this` generally refers to a class instance in which the method is
  defined. **All JavaScript functions, methods or otherwise, have access to the `this`
  parameter**. The item that `this` points to is defined by the way in which the function
  is _invoked_.

----------

## 4.2. Invoking Functions

This section goes into detail about **what happens when a function is called**. There
  are multiple ways to invoke a function, and the way a function is invoked impacts
  how the code _within the function_ operates, and how the `this` parameter (function
  context) is established.

There are four ways to invoke functions:

- _As a function_ - `skulk()`; invoked in a straightforward manner
- _As a method_ - `ninja.skulk()`; invocation tied to object, enabling OOP
- _As a constructor_ - `new Ninja()`; a **new object** is instantiated
- _Via the function's_ `apply` _or_ `call` _methods_ - `skulk.call(ninja)`

Excluding the `call` and `apply` variations, the _function invocation **operator**_
  is a set of parenthesis following **any expression** that evaluates to a _function 
  reference_.

### 4.2.1. Invocation As A Function

While it might seem trivial to specify that a function is invoked as a...uh...function,
  we clarify because it _distinguishes the invocation from other invocation
  mechanisms: methods, constructors, and `apply` / `call`. If a function isn't
  invoked as a method, constructor, or via `apply` or `call`, it's _invoked
  as a function_.

**Function invocation** occurs when a function is called using the `()` operator
  and the expression on which the `()` operator is applied _does not reference
  the function as a property of an object_.

The _context_ (`this` parameter) of a function _invoked as a function_ can be one
  of two things: in strict mode it will be `undefined`, whereas in nonstrict
  mode the context of the function would be considerd the _global context (window
  object)_.

### 4.2.2. Invocations As A Method

A function is considered to be _invoked as a method_ if the function is assigned
  to an **object property**, and is invoked by referencing the function **using that
  property**. The function is being _invoked as a method_ of that object.

For the rest of this section we will reference the following code example:
```JavaScript
/***
 * Returns the function context
 * that will allow us to examine
 * the context from outside 
 ***/

function whatsMyContext() {
  return this;
}

var getMyThis = whatsMyContext; // Variable references whatsMyContext function

var ninja1 = {                  // Object with a property which references whatsMyContext function
  getMyThis: whatsMyContext;    
};

var ninja2 = {                  // Object with a property which references whatsMyContext function
  getMyThis: whatsMyContext;    
};
```

The `whatsMyContext()` function simply returns its _function context_. If it is 
  _invoked as a function_, the _function context_ would be global (`window` object)
  unless we are in strict mode, in which case it would be `undefined`. The variable
  `getMyThis` is a _reference_ to the function, **not** a second instance of
  the function. If we follow the `getMyThis` variable with _function invocation
  operators_ `()`, we are once again invoking the function _as a function_ and
  will get the same _function context_ as earlier; the function is not being
  invoked from an _object property reference_, it is not _constructing_ a 
  new function, and we did not utilize `apply` or `call` to invoke the function.

`ninja1` is a variable that has been assigned an _object_ with a _property_, `getMyThis`,
  _which references the_ `whatsMyContext` _function_. To call this function, we invoke
  the function using a _method reference_ (`ninja1.getMyThis()`) which makes the _function
  context_ the `ninja1` object.

`ninja2` is an exact copy of `ninja1` aside from the variable name. This was done to
  illustrate that the `whatsMyContext()` function can be referenced by as many
  objects and variables as we need, but the _function context_ always relates directly
  to the object on which the function is invoked on.

### 4.2.3. Invocation As A Constructor

**Constructor functions** are declared exactly like any other function; we can construct
  new objects using _function declarations and function expressions_. The difference
  lies in the process for _invoking the function_. To invoke a function as a 
  _constructor_, we simply precede the function invocation with the `new` keyword.

> **NOTE: DO NOT CONFUSE _FUNCTION CONSTRUCTORS_ WITH _CONSTRUCTOR FUNCTIONS_.** _Function
> constructors_ allow us to create functions from _dynamically created strings_.
> _Constructor functions_, the topic of this section, are functions used to create
> and initialize object instances.

A **constructor function** creates an _empty object instance_ and passes it to the function
  as its _function context_. The constructor then adds properties to the new object
  as defined in the _function body_. Finally, the _newly constructed object_ is the
  return value of the function invocation. **This is the primary purpose of a
  constructor function**: create a new object, set it up, and return it as the
  constructor value. Any interference with that process does not belong in a constructor.

Technically, we can _new_ up an instance of any function regardless of the function
  body. If the constructor function has a defined return value that is **not an object**,
  the returned value is ignored and a newly created object is returned. If, however,
  the constructor function **explicitly** returns an object, that object will be
  the return value of the `new` expression. The newly created object passed as the
  _function context_ is discarded.

Since **constructor functions** have these special conditions to take into consideration,
  they are coded in a different manner than other functions.

**Coding considerations for constructors**

Constructors _initialize a new object_ that is created by the _function invocation_
  with a set of specified _initial conditions_. Constructor functions follow a
  naming convention to differentiate them from other functions or methods. While
  standard functions/methods are named along the lines of the action they perform,
  **constructor functions** are generally named as a _noun describing the object
  which the function will be creating_ and is capitalized. They allow us to write
  more elegant code by providing the ability to create multiple objects that
  conform to a similar pattern without having to repeat code.

### 4.2.4. Invocation With `Call` and `Apply` Methods

A key difference between the various types of _function invocation_ is how the object
  of the _function context_ is defined. Invoking as a method targets the method's 
  _owning object_; top-level functions target either the `window` object or
  `undefined`, depending on the strictness of the function call; constructors
  target their newly created _object instance_. If we want to _explicitly set
  the function context_, we can utilize the built-in **function methods**
  `apply` and `call`.

As mentioned many times, functions are _first-class objects_ in JavaScript, and just
  like any other object they can have properties and attributes. All JavaScript
  functions have access to the `apply` and `call` methods, and they are invoked
  in similar fashion. The `apply` method accepts two parameters: the object which
  will be the target of the _function context_, and an **array of values** to be used
  as the _invocation arguments_. The `call` method only differs in that, instead of
  accepting the arguments in the form of an array, it simply treats all arguments
  after the first as the _invocation arguments_.

The `call` and `apply` methods can come in handy if we need to _override the function
  context_ with a different object, a practice that is useful when invoking
  _callback functions_.

**Example:**
```JavaScript
/** Function accepts an array and a callback function 
  * as arguments, iterates through the array, and invokes
  * the callback function using the 'call' method, providing
  * the current array item as the function context and the current
  * index as an argument.
  **/

function callExample(list, callback) {
  for (var n = 0; n < list.length; n++) {
    callback.call(list[n], n);
  }
}
```

In the example above, the `call` method was used because we only expect to pass in
  one argument to the callback function. `call` can also be utilized if we are passing
  multiple _unrelated values_ as arguments; if the arguments are related (or are already
  wrapped up in a nice little array) it would be more logical to implement the `apply`
  method. Utilize whichever method seems more appropriate for any given situation. Logical
  applications of the correct method will increase code clarity, which is always a 
  good thing.

----------

## 4.3. Fixing The Problems of Function Contexts

To put it quite frankly, keeping track of the _function context_ can be a pain in the ass.
  We've explored some ways of managing or manipulating the _function context_ to our liking
  by invoking functions via the `call` and/or `apply` **built-in methods** that all functions
  have access to. In the following section we will cover two additional ways of handling
  the _function context_ which, depending on the situation, can be cleaner and more
  elegant.

### 4.3.1. Using Arrow Functions To Get Around Function Contexts

Arrow functions have a unique feature which makes them quite attractive when dealing
  with callbacks; arrow functions don't have their own `this` value. An **arrow function**
  _inherits the 'this' parameter from the object in which they're defined_. For example,
  if we were to write an arrow function _inside_ of a constructor function, the `this`
  parameter would be bound to new objects created by calling the constructor function.
  If we wrote one within the body of a standard function, the `this` property of the
  arrow function would be the function within which it was defined.

**CAVEAT: Arrow functions and object literals**

If an object literal has a property which is an _arrow function_, and that object is
  defined within the global namespace, the arrow function's `this` property will be
  the `this` value of the global code (in a browser, it would be the `window` object).

### 4.3.2. Using The `bind` Method

Yes, another **built-in method** available to all functions that exists to help us
  manage _function context_! The `bind` method accepts one argument, the object
  that we want to set as the _function context_, and returns a **new** function
  that is _always_ bound to the provided object.

Example:
```HTML
<button id="test">Click me!</button>
<script>
  var button = {
    clicked: false,
    click: function(){
      this.clicked = true;
    }
  };
  var elem = document.getElementById("test");
  elem.addEventListener("click", button.click.bind(button));

  var boundFunction = button.click.bind(button);
  assert(boundFunction != button.click,
          "Calling bind creates a completely new function!");
</script>
```

In the example above, we create an _object literal_, `var button = {...}`, to manage
  the state of the button. When the button is clicked, we want to set the _object
  literal's_ `clicked` property to `true`. To accomplish this we _bind_ the `click`
  method to the `button` object, **permanently** associating the `click` method's 
  `this` property with the `button` object. If we simply placed `button.click`
  as the event-handler, the _function context_ of the `click` method would be the
  element to which the event handler is attached, meaning that it would equal
  the HTML button element as opposed to our `button` object literal.

----------

## 4.4. Summary

- When a function is invoked, it is passed two _implicit parameters_, `arguments`
  and `this`:

  - The `arguments` parameter maintains all of the arguments passed into the function.
    It has a `length` property that returns the number or arguments provided upon
    _invocation_, and gives us the ability to access arguments and argument values
    that don't have matching parameters. The `arguments` parameter _aliases_ the
    function parameters; changing the value of one will change the value of the other.
    To avoid this we simply implement "strict mode".
  - The `this` parameter represents the _function context_, an object which should be
    associated with the _function invocation_. We can determine the value of `this`
    based on the way a function is defined **as well as** how it's invoked.
  
- A function can be invoked in one of four ways: As a function, as a method,
  as a constructor, or via the function's `apply` and `call` methods.

- The way in which a function is invoked influences the value of the `this` parameter:

  - Functions _invoked as functions_ set the value of `this` to either the global
    `window` object, or if "strict mode" is enabled, `undefined`.
  - Functions _invoked as methods_ set the `this` parameter as the _object on which the
    function was invoked_.
  - Functions _invoked as constructors_ set the value of `this` to the newly
    constructed object.
  - Functioned _invoked through `call` or `apply`_ set the value of `this` to be
    the first argument provided in the _function invocation_.

- **Arrow functions** don't have a `this` value of their own; they inherit the
  value of `this` from the object in which they are defined.

- The `bind` method is a **built-in method** available to all functions in JavaScript,
  similar to `call` or `apply`, but the similarities end there. Where as `call`/`apply`
  simply provide an argument to be used as the _function context_ during one particular
  _function invocation_, the `bind` method _will create and return a new function with
  the exact same body as the original function_, but with the `this` property
  **permanently** set to an object provided as an argument.

----------

## 4.5. Exercises

1. Write a function which calculates the sum of all passed-in arguments, regardless
   of the number of arguments provided.

  ```JavaScript
  function sumRest(...values){
    var sum = 0;
    for (var i = 0; i < values.length; i++){
      sum += values[i];
    }
    return sum;
  }
  ```

2. After running the following code, what are the values of variables `ninja` and
    `samurai`?

  ```JavaScript
  function getSamurai(samurai){
    "use strict"

    arguments[0] = "Ishida";
    return samurai;
  }

  function getNinja(ninja){
    arguments[0] = "Fuma";
    return ninja;
  }

  var samurai = getSamurai("Toyotomi");   // Toyotomi
  var ninja = getNinja("Yoshi");          // Fuma
  ```

3. When running the following code, which of the assertions wll pass?

  ```JavaScript
  function whoAmI1(){
    "use strict"
    return this;
  }

  function whoAmI2(){
    return this;
  }

  assert(whoAmI1() === window, "Window?"); // fail: "use strict" makes this undefined
  assert(whoAmI2() === window, "Window?"); // pass: nonstrict function targets global window object
  ```

4. When running the following code, which of the assertions will pass?

  ```JavaScript
  var ninja1 = {
    whoAmI: function(){
      return this;
    }
  };

  var ninja2 = {
    whoAmI: ninja1.whoAmI
  };

  var identify = ninja2.whoAmI;

  assert(ninja1.whoAmI() === ninja1, "ninja1?");
  assert(ninja2.whoAmI() === ninja1, " ninja1 again?");

  assert(identify() === ninja1, "ninja1 again?");

  assert(ninja1.whoAmI.call(ninja2) === ninja2, "ninja2 here?");
  ```

  - The first and last `assert` statements would pass; the second and third
  `assert` statements would fail. The second statement fails because the `whoAmI`
  method is being _invoked as a method_, which makes the _function context_
  ninja2. The third statement fails because `identify()` is _invoked
  as a function_, which makes the `this` property the global `window` object.

5. When running the following code, which of the assertions will pass?

  ```JavaScript
  function Ninja(){
    this.whoAmI = () => this;
  }

  var ninja1 = new Ninja();
  var ninja2 = {
    whoAmI: ninja1.whoAmI
  };

  // pass: whoAmI() is an arrow function defined within a constructor function,
  // meaning that the function context will be the new object instantiated from
  // the constructor function.
  assert(ninja1.whoAmI() === ninja1, "ninja1 here?");
  // fail: whoAmI() has a function context equivalent to the object within which
  // it is defined, and in this case that would be 'ninja1'.
  assert(ninja2.whoAmI() === ninja1, "ninja2 here?");
  ```

6. Which of the following assertions will pass?

  ```JavaScript
  function Ninja(){
    this.whoAmI = function(){
      return this;
    }.bind(this);
  }

  var ninja1 = new Ninja();
  var ninja2 = {
    whoAmI: ninja1.whoAmI
  };

  // pass: ninja1.whoAmI() is permanently bound to ninja1
  assert(ninja1.whoAmI() === ninja1, "ninja1 here?");
  // fail: ninja1.whoAmI() is permanently bound to ninja1, and ninja2.whoAmI()
  // references ninja1.whoAmI(), meaning that it will still come back as ninja1.
  assert(ninja2.whoAmI() === ninja2, "ninja2 here?");
  ```
