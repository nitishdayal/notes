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

    - A function is a stand-alone object, where as a method is a function which
      is _a property of an object_.

3. What would happen if a constructor function explicitly returned an object?

    - 

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

[**NOTE**: While there is little need to utilize the `arguments` parameter due to the
  introduction of `rest` parameters in the latest JavaScript standard, it's important
  to understand the functionality and uses of `arguments` as we're very likely to
  come across it in legacy code.]

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
  rather silently picking them up and remove some 'unsafe' language features. One such feature 
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


