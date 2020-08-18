## Functions the Ultimate

Notes taken from Douglas Crockford [lecture](https://www.youtube.com/watch?v=DogGMNBZZvg&t=7021s)

function expression

Create functions using the function expression/literal which returns a new function object which can be invoked.

Naming a functions allows for it to be called recursively.

Function objects are **first class**.
This means they may be:
- passed as an arugment to a function
- returned from a function
- assigned to a variable
- stored in an object or array

Function objects inherit from ```Function.prototype```.

The ```var``` statement declares and initialises variables within a function.

A variable declared anywhere within a function is visible everywhere within the function. This is commonly referred to as 'Function scope'.

```var``` statement
- declaration part gets **hoisted** to the top of the function, initalising with ``` undefined```
- initialisation part turns into an ordinary assignment

```var myVar = 0, myOtherVar;```
expands into
```
var myVar = undefined.
  myOtherVar = undefined;
  ...
myVar = 0;
```

function statement

Function name is mandatory.
Function statement is a short-hand for a var statement with a function value.
```function foo() {}```
expands to
```var foo = function() {};```
which further expands to
```
var foo = undefined;
foo = function() {};
```
The assignment of the function is also hoisted (to the top of the function).

function expression v function statement
If the first token in a statement is ```function``` then it is a function statement.

Declare all variables at the top of the function.
Declare all functions before you call them.

Closure, lexical/static scoping
Functions can nest, functions are values.

Using **Closure**

```
var digit_name = (function() {
  var names = ['zero', 'one', 'two', 
    'three', 'four', 'five', 'six',
    'seven', 'eight', 'nine']
  
  return function(n) {
    return names[n];
  };
}());

alert(digit_name(3));
```

Function as module (NO GLOBAL VARIABLES CREATED)
```
var...
function...
function...
```
compared to
```
(function() {
  var...
  function...
  function...
}());
```

A module pattern
```
var singleton = (function() {
  var privateVariable;
  function privateFunction(x) {
    ... privateVariable...
  }
  return {
    firstMethod : function(a, b) {
      ... privateVariable...
    },
    secondMethod : function(c) {
      ... privateFunction() ...
    }
  };
}());
```

Functional Inheritance

```
function gizmo(id) {
  return {
    toString : function() {
      return "gizmo " + id;
    }
  };
}

function hoozit(id) {
  var that = gizmo(id)
  that.test = function(testid) {
    return testid === id;
  };
  return that;
}
```




