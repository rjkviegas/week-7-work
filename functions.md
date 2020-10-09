## Functions the Ultimate

Notes taken from Douglas Crockford [lecture](https://www.youtube.com/watch?v=DogGMNBZZvg&t=7021s)

### function expression

A function expression creates functions using the function expression/literal, which returns a new function object, which in turn can be invoked.

The naming of a function allows for it to be called **recursively** (by itself so it repeats).

Function objects are **first class**.
This means they may be:
- passed as an argument to a function
- returned from a function
- assigned to a variable
- stored in an object or array

Function objects inherit from ```Function.prototype```.

### ```var``` statement

The ```var``` statement declares and initialises variables within a function.

A variable declared anywhere within a function is visible everywhere within the function. This is commonly referred to as **function scope**.

```var``` statement
- declaration part gets **hoisted** to the top of the function, initalising with ``` undefined```
- initialisation part turns into an ordinary assignment
Example:

```var myVar = 0, myOtherVar;```
expands into
```
var myVar = undefined.
  myOtherVar = undefined;
  ...
myVar = 0;
```

### function statement

A function statement is a short-hand for a ```var``` statement with a function value. A function name is mandatory.
Example:
```function foo() {}```
expands to
```var foo = function() {};```
which further expands to
```
var foo = undefined;
foo = function() {};
```
The assignment of the function is also hoisted (to the top of the function).

### function expression v function statement
If the first token in a statement is ```function``` then it is a function statement.

#### Tips
Declare all variables at the top of the function.
Declare all functions before you call them.

### Closure
Is sometimes referred to as lexical/static scoping
Functions can nest, functions are values.

Example using **Closure**

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

### Functional Inheritance

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
```
function my_little_constructor(spec) {
  let {member} = spec;
  const reuse = other_constructor(spec);
  const method function() {
    // It can use spec, member, reuse, method
  };
  return Object.freeze({
    method,
    goodness: reuse.goodness
  });
}
```
Messing Around:

```
function counter_constructor() {
  let counter = 0;

  function up() {
    counter += 1;
    return counter;
  }

  function down() {
    counter -= 1;
    return counter
  }

  return Object.freeze({
    up,
    down
  });
}
let spec = {name : "Dewey", mana_cost : 50, isLong : true};

function game_counter(spec) {
  let {name, mana_cost, isLong} = spec;
  const reuse = counter_constructor(spec);
  
  const myName = function() {
    return "My name is " + name;
  }
  
  const losesLegs = function() {
    isLong = false;
  }
  
  const isLongLong = function() {
    return isLong;
  }
  
  return Object.freeze({
    myName, 
    losesLegs,
    isLongLong,
    up : reuse.up,
    down : reuse.down
  });
}

const gamedeath = game_counter(spec);

gamedeath // {myName: ƒ, losesLegs: ƒ, isLongLong: ƒ, up: ƒ, down: ƒ}

gamedeath.up() // 1
gamedeath.down() // 0
gamedeath.myName() // "My name is Dewey"
gamedeath.isLongLong() // true
gamedeath.losesLegs() // undefined
gamedeath.isLongLong() // false
```




