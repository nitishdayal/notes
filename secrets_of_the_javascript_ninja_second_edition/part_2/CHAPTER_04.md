# Chapter 4. Functions for the Journeyman: Understanding Function Invocation

Topics Covered:

- `arguments` and `this` _implicit_ parameters
- Ways of invoking functions
- Dealing with function context-related problems

## Do You Know? (Pre-Chapter Knowledge Check)

1. Why is the `this` parameter known as the function _context_?

    - The `this` parameter is known as the _function context_ because it is used
      to refer to the object on which the function is being invoked upon; for example,
      if we call a method on an array (`array.push()`), the `this` parameter would
      refer to the array on which the `.push()` method was called upon. 

2. What's the difference between a function and a method?

    - A function is a stand-alone object, whereas a method is a function which
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

`ninja1` is variable that has been assigned an _object_ with a _property_, `getMyThis`,
  _which references the_ `whatsMyContext` _function_. To call this function, we invoke
  the function using a _method reference_ (`ninja1.getMyThis()`) which makes the _function
  context_ the `ninja1` object.

`ninja2` is an exact copy of `ninja1` aside from the variable name. This was done to
  illustrate that the `whatsMyContext()` function can be referenced by as many
  objects and variables as we need, but the _function context_ always relates directly
  to the object on which the function is invoked on.

### 4.2.3. Invocation As A Constructor

_Constructor functions_ are declared exactly like any other function; we can construct
  new objects using _function declarations and function expressions_. The difference
  lies in the process for _invoking the function_. To invoke a function as a 
  _constructor_, we simply precede the function invocation with the `new` keyword.

> **NOTE: DO NOT CONFUSE _FUNCTION CONSTRUCTORS_ WITH _CONSTRUCTOR FUNCTIONS_.** _Function
> constructors_ allow us to create functions from _dynamically created strings_.
> _Constructor functions_, the topic of this section, are functions used to create
> and initialize object instances.

A _constructor function_ creates an _empty object instance_ and passes it to the function
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

Since _constructor functions_ have these special conditions to take into consideration,
  they are coded in a different manner than other functions.

**Coding considerations for constructors**

Constructors _initialize a new object_ that is created by the _function invocation_
  with a set of specified _initial conditions_. Constructor functions follow a
  naming convention to differentiate them from other functions or methods. While
  standard functions/methods are named along the lines of the action they perform,
  _constructor functions_ are generally named as a _noun describing the object
  which the function will be creating_ and is capitalized. They allow us to write
  more elegant code by providing the ability to create multiple objects that
  conform to a similar pattern without having to repeat code.

### 4.2.4. Invocation With `Call` and `Apply` Methods



----------

## 4.3. Fixing The Problems of Function Contexts


### 4.3.1. Using Arrow Functions To Get Around Function Contexts

### 4.3.2. Using The `bind` Method



----------

## 4.4. Summary


