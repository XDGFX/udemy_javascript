# Strict mode

Strict mode can be enabled in JavaScript by adding the `"use strict"` directive
to the beginning of a script. This is recommended as it enforces tighter
restrictions on the code, and helps prevent some common coding errors.

# Function definitions

There are three ways of defining a function in JavaScript:

### 1. Function declaration

This is closest to a `def` declaration in something like Python.

```javascript
function myFunction() {
  // ...
}
```

> Can be called before or after the function is defined.

### 2. Function expression

As functions are technically a special type of variable, the function object
can be created and then assigned to a variable name.

```javascript
const myFunction = function () {
  // ...
};
```

> Can only be called after the function is defined.

### 3. Arrow functions

This works the same as function expressions, but is a shorter way of
writing functions.

```javascript
const myFunction = () => {
  // ...
};
```

> Can only be called after the function is defined.

# `let`, `const`, and `var`

A variable can be defined using one of three keywords. Note that after it is
defined, it can be reassigned without using the keyword again.

### 1. `let`

Used to assign a variable that may be reassigned.

```javascript
let myVariable = "Hello";
myVariable = "World";
```

### 2. `const`

Used to assign a variable that cannot be reassigned.

```javascript
const myVariable = "Hello";
myVariable = "Goodbye"; // Error
```

### 3. `var`

Also used to assign a variable that may be reassigned, but originally comes from
ES5. Typically it should not be used in modern JavaScript, the only difference
is that `var` variables are never block scoped, but instead function scoped by
the nearest function.

# Variable scope

There are three different scopes in JavaScript:

- Global scope
- Function scope
- Block scope

They work similarly to Python, and determine which variables are accessible from
where.

Global scope is the top level scope, and is accessible from anywhere.

```javascript
let myVariable = "Hello"; // Global scope
```

Function scope can be accessed from anywhere within the function.

```javascript
function myFunction() {
  let myVariable = "Hello"; // Function scope
}
```

Block scope is where JavaScript differs from Python; where variables are scoped within curly braces such as those used in `if` statements.

```javascript
if (true) {
  let myVariable = "Hello"; // Block scope
}

console.log(myVariable); // Error
```

The difference between `let` and `var` is that `let` is block scoped, while
`var` is function scoped. Therefore, replacing `let` with `var` in the previous
example would no longer produce an error, however it would be better to just
declare the variable beforehand in the function scope.

## Reassigning variables

Variable reassignment works differently to Python; reassigning any variable will
change the original variable value and not create a new variable. This means if
the original variable was global scoped and was accessible anywhere, the new
value will also be accessible from anywhere.

```javascript
let myVariable = "Hello";

const myFunction = function () {
  myVariable = "Goodbye";
};

console.log(myVariable); // Hello
myFunction();
console.log(myVariable); // Goodbye
```

# Hoisting

Hoisting is a JavaScript feature that allows variables to be accessible/usable
before they are actually declared.

This can be interpreted as moving the declaration of a variable to the top of
the current scope, and then assigning the variable to a placeholder value, until
the variable is actually declared.

### Hoisting Table

|                                   | Hoisted? |              Initial Value               |         Scope          |
| :-------------------------------: | :------: | :--------------------------------------: | :--------------------: |
|      `function` declarations      |  ✅ Yes  |             Actual function              | Block (in strict mode) |
|          `var` variables          |  ✅ Yes  |               `undefined`                |        Function        |
|    `let` and `const` variables    |  🚫 No   |            `<uninitialised>`             |         Block          |
| `function` expressions and arrows |          | Depends on if using `var` or `let/const` |                        |

In practice, this means that function declarations can happen anywhere within
the block, and you can still use it anywhere. Function expressions or variables declared with
`let/const` can only be used after they have been declared. Avoid using `var` in
all other cases 😉.

# `this`

The `this` keyword is similar to the `self` keyword in Python, but actually has
a much larger scope. The `this` keyword is a special variable which points to
the 'owner' of the function in which the keyword is used.

`this` is not static; it depends on how the function is called, and the value is
only assigned when the function is called.

There are four different ways to call a function:

- **Method call**: `myObject.myFunction()`, `this` points to `myObject`
- **Function call**: `myFunction()`, `this` is undefined (in strict mode)
- **Arrow function call**: `myFunction()`, `this` is the surrounding function
  (lexical `this`)
- **Event listener**: `myElement.addEventListener()`, `this` is the DOM element
  that the event listener is attached to

`this` never points to the function itself, or the variable environment of the function.

# `arguments`

The `arguments` keyword is another special keyword used in JavaScript. It can be
used inside a function to get all arguments passed to the function.

```javascript
function myFunction(a, b, c) {
  console.log(arguments);
}

myFunction(1, 2, 3); // [1, 2, 3]
```

It can be used to access more parameters than specified in the function
declaration. However, there is a more modern way of doing this, which is
described later.

```javascript
function myFunction(a, b, c) {
  console.log(arguments);
}

myFunction(1, 2, 3, 4); // [1, 2, 3, 4]
```

Arrow functions do not get this keyword.
