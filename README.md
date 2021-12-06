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

As functions are technically a special type of variable, the function object can
be created and then assigned to a variable name.

```javascript
const myFunction = function () {
  // ...
};
```

> Can only be called after the function is defined.

### 3. Arrow functions

This works the same as function expressions, but is a shorter way of writing
functions.

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

Block scope is where JavaScript differs from Python; where variables are scoped
within curly braces such as those used in `if` statements.

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
the block, and you can still use it anywhere. Function expressions or variables
declared with `let/const` can only be used after they have been declared. Avoid
using `var` in all other cases 😉.

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

`this` never points to the function itself, or the variable environment of the
function.

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

# Mutable vs immutable variables

Similarly to Python, JavaScript has mutable (objects) and immutable (primitives)
variables.

This is to do with the way that the variable is stored in memory, and whether or
not it can be changed.

In practice, this can cause confusion when copying one variable to another, as
immutable variables will function as two separate variables, whereas mutable
variables will act as one variable with two different labels.

```javascript
// Immutable variables
let age = 30;
let oldAge = age;
age = 31;

console.log(age); // 31
console.log(oldAge); // 30

// Mutable variables
const me = {
  name: "John",
  age: 30,
};
const friend = me;
friend.age = 27;

console.log(me.age); // 27
console.log(friend.age); // 27
```

> 🚧 Behind the scenes, this is because primitives (numbers, strings, booleans)
> are stored entirely in the call stack, whereas objects (arrays, objects) are
> stored in the heap, with only a reference to the object stored in the call
> stack.

> ❗ This is also why we can change values within an object, even though it is
> assigned with the `const` keyword. `const` != immutable.

### Shallow copy

If we really want to copy an object, we could use the `Object.assign()` method.
This merges two objects, and returns a new object with the properties of both
objects.

```javascript
const me = {
  name: "John",
  age: 30,
};

// Option 1
const friend = Object.assign({}, me);
friend.age = 27;

// Option 2 (all in one)
const friend = Object.assign(me, {
  age: 27,
});
```

> ⚠️ This is not a deep copy, meaning that the properties of the object will be
> copied, but not the properties of the properties. This means that if the
> properties of the object have objects, they will be copied as references, and
> we run into the same issue as before!

### Deep copy

There are several ways to achieve this, but they are mostly experimental, only
supported in some browsers, slow, or potentially dangerous!

The most promising option at the moment is the `structuredClone` global
function, which at the time of writing is only supported in Node.js >=17 and
Firefox 94, however it will hopefully be supported on other browsers in the
future.

# REST operator

For clarity, the REST operator is the opposite of the spread operator. The
spread operator (`...`) is used to spread out an array into individual values,
as shown below:

```javascript
const myArray = [4, 5, 6];
const mySecondArray = [1, 2, 3, ...myArray];

console.log(mySecondArray); // [1, 2, 3, 4, 5, 6]
```

The REST operator does the opposite, by combining multiple values into an array.
It uses the same `...` syntax as the spread operator, but can be differentiated
as it's always on the left side of an `=`, whereas the spread operator is always
on the right side.

```javascript
function myFunction(arg1, arg2, ...rest) {
  console.log(arg1); // 1
  console.log(arg2); // 2
  console.log(rest); // [3, 4, 5, 6]
}

myFunction(1, 2, 3, 4, 5, 6);
```

It can work in a similar way to `*args` in Python.

# Function `kwargs`

I felt as though this was worth mentioning due to it's close connection to
multiple args as discussed in the previous section. In Python, you can pass
keyword arguments into a function in any order, as shown:

```python
def my_function(a, b, c):
  print(a, b, c)

my_function(b=2, a=1, c=3)  # 1 2 3
```

This is possible in ES6, but it's a little more complicated. The function must
take an array as the input, and when calling the function, you must pass in an
array as the input.

```javascript
function myFunction({ a, b, c }) {
  console.log(a, b, c);
}

myFunction({ b: 2, a: 1, c: 3 }); // 1 2 3
```

As in Python, these can be substituted with default values if desired.

```javascript
function myFunction({ a = null, b = null, c = null }) {
  console.log(a, b, c);
}

myFunction({ b: 2 }); // null 2 null
```

# Nullish coalescing operator

There are operators which can be used to 'short circuit' a value, similar to
Python.

```javascript
// Old way
if (a === undefined) {
  a = 1;
}

// New way
a = a || 1;
```

This is similar to `or` in Python.

```python
a = a or 1
```

However, there is an issue with this, as it will not work with 'falsey' values.

```javascript
a = 0;
a = a || 1; // 1
```

This can be fixed using the 'nullish coalescing' operator.

```javascript
a = 0;
a = a ?? 1; // 0

a = undefined;
a = a ?? 1; // 1
```

This can also be used when accessing properties of an object. Sometimes a
sub-property will not be available, and will return `undefined`.

```javascript
const obj = {
  a: 1,
  b: 2,
};

console.log(obj.a); // 1
console.log(obj.b); // 2
console.log(obj.c); // undefined
```

This is fine, but accessing sub-properties of that object can require additional
code to check for undefined.

```javascript
const obj = {
  a: {
    x: 1,
  },
  b: {
    y: 2,
  },
};

console.log(obj.a.x); // 1
console.log(obj.b.y); // 2
console.log(obj.c.z); // error

// Old solution
if (obj.a) {
  console.log(obj.a.x); // 1
}
// etc...

// New solution
console.log(obj.a?.x); // 1
console.log(obj.b?.y); // 2
console.log(obj.c?.z); // undefined
```

The operator 'short circuits' the execution, and as soon as `obj.c` does not
exist, it returns `undefined` without executing the rest of the code.
