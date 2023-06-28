# Functions

# Defining Functions

## Function Declaration

Function declarations consist of the function keyword, followed by these components:
- An identifier that names the function.
- A pair of parentheses around a comma-separated list of zero or more identifiers / parameter names.
- A pair of curly braces with zero or more JavaScript statements inside / body of the function.

```jsx
function distance(x1, y1, x2, y2) {
let dx = x2 - x1;
let dy = y2 - y1;
return Math.sqrt(dx*dx + dy*dy);
}
```

## Function Expressions

Function expressions look a lot like function declarations, but they appear within the context of a larger expression or statement, and the name is optional

```jsx
const square = function(x) { return x*x; };
```

It is a good practice to use `const` with function expressions so you don’t accidentally overwrite your functions by assigning new values.

<aside>
❓ Function definitions are hoisted. Function expressions and arrow functions are not.

</aside>

```jsx
function add(x, y) {
 return x+y
}

const add = function(x, y) {
	return x+y
}

// these two functions are two different function objects!
// even though they look the same, behind the scenes, they are not
```

## Arrow Functions

```jsx
const sum = (x, y) => { return x + y; };
const sum = (x, y) => x + y;
const polynomial = x => x*x + 2*x + 3;
const constantFunc = () => 42;
```

Arrow functions are ideal when you need to pass one function to another (common with array methods like `map()`, `filter()`, and `reduce()`

```jsx
let filtered = [1,null,2,3].filter(x => x !== null); 
let squares = [1,2,3,4].map(x => x*x);
```

### What is different about arrow functions?

- They inherit the value of `this` keyword from the environment in which they are defined.
- They do not have a prototype property, so they can’t be used as constructor functions for classes.

## Nested Functions

In JavaScript, functions may be nested within other functions.

```jsx
function hypotenuse(a, b) {
function square(x) { return x*x; }
return Math.sqrt(square(a) + square(b));
}
```

Scoping rule: nested functions can access the parameters and variables of the function (or functions) they are nested within. 

Nested functions do not inherit the `this` value of the containing function. 

# Invoking Functions

JavaScript functions can be invoked in five ways:
- As functions
- As methods
- As constructors
- Indirectly through their call() and apply() methods
- Implicitly, via JavaScript language features that do not appear like normal function invocations

## Function Invocation

The basic way of invoking a function with two arguments: `f(a, b)`

If the function is the property of an object or an element of an array - that is a METHOD INVOCATION expression.

<aside>
⛔ CONDITIONAL INVOCATION
You can insert ?. after the function expression and before the open parenthesis in a function invocation in order to invoke the function only if it is not `null` or `undefined`. That is, the expression `f?.(x)` is equivalent to: `(f !== null && f !== undefined) ? f(x) : undefined`

</aside>

For function invocation`this` is the global value. In strict mode it is `undefined`. Arrow functions inherit the value of `this` keyword from the environment in which they are defined. 

### Recursive functions and the stack

A recursive function is one that calls itself.  When a function A calls function B, and then function B calls function C, the JavaScript interpreter needs to keep track of the execution contexts for all three functions. When function C completes, the interpreter needs to know where to resume executing function B, and when function B completes, it needs to know where to resume executing function A. You can imagine these execution contexts as a stack. When a function calls another function, a new execution context is pushed onto the stack. When that function returns, its execution context object is popped off the stack. If a function calls itself recursively 100 times, the stack will have 100 objects pushed onto it, and then have those 100 objects popped off. This call stack takes memory. On modern hardware, it is typically fine to write recursive functions that call themselves hundreds of times. But if a function calls itself ten thousand times, it is likely to fail with an error such as “Maximum call-stack size exceeded.”

TLDR: a recursive function can be bad for memory, you can exceed maximum call-stack size.

## Method Invocation

Method - a JS function that is stored in an object. Usually it is invoked this way: `o.m(x,y)`But (although very rarely) can also be invoked this way: `o['m'](x,y)`

### Method Chaining

If a method returns an object, you can use the return value of one method invocation as past of a subsequent invocation.
When you write a method that does not have a return value of its own, consider having the method return `this`. If you do this consistently throughout your API, you will enable a style of programming known as method chaining in which an object can be named once and then multiple methods can be invoked on it:

```jsx
new Square().x(100).y(100).size(50).outline("red").fill("blue").draw();
```

If a nested function is invoked as a method, its this value is the object it was invoked on. 

If a nested function (that is not an arrow function) is invoked as a function, then its this value will be either the global object (non-strict mode) or undefined
(strict mode). 

It is a common mistake to assume that a nested function defined within a method and invoked as a function can use this to obtain the invocation context of the method. Example:

```jsx
let o = { // An object o. 
	m: function() { // Method m of the object.
		let self = this; // Save the "this" value in a variable.
		this === o // => true: "this" is the object o.
		f(); // Now call the helper function f().
		function f() { // A nested function f
			this === o // => false: "this" is global or undefined
			self === o // => true: self is the outer "this" value.
		}
	}
};
o.m(); // Invoke the method m on the object o.

// BUT if you wrote function f() this way:
const f = () => {
this === o // you would get true
// because arrow functions inherit this 
};
```

## Constructor Invocation

If a function or method invocation is preceded by the keyword `new`, then it is a constructor invocation.

```jsx
// in constructors you can omit ()
// so these are equivalent 
o = new Object();
o = new Object;
```

A constructor invocation creates a new, empty object that inherits from
the object specified by the prototype property of the constructor.

Constructor functions do not normally use the return keyword.

## Indirect Invocation

JS Functions are objects, so they have their own methods. Let’s talk about `call()` and `apply()`.

Both methods:

- invoke the function indirectly
- allow you to explicitly specify the `this` value ⇒ this means you can invoke any function as a method of any object
- allow you to specify the arguments for the invocation

`call()` uses its own argument list as arguments to the function 

`apply()` expects an array of values to be used as arguments

## Implicit Function Invocation

There are various features that do not look like function invocations but that cause functions to be invoked. These may result in bugs, side effects and performance issues, so BE CAREFUL when writing functions that may be implicitly invoked.

1. if an object has setters and getters defined, querying or setting the value of its properties may invoke those methods
2. when an object is used in a string context `toString()` is implicitly called
3. when an object is used in a numeric context `valueOf()` is implicitly called
4. when you loop over the elements of an iterable object, there are a number of methods that could be called…
5. tagged template literal is a function invocation in disguise
6. proxy objects have their behavior completely controlled by functions

# Function Arguments and Parameters

## Optional Parameters and Defaults

A function is declared with fewer arguments that parameters ⇒ the additional parameters are set to `undefined`. So you should write functions so that some arguments are optional.

```jsx
// Append the names of the enumerable properties of object o to the
// array a, and return a. If a is omitted, create and return a new array.
function getPropertyNames(o, a) {
	a = a || []; // If a is undefined, use a new array
	for(let property in o) a.push(property);
	return a;
}
// getPropertyNames() can be invoked with one or two arguments:
let o = {x: 1}, p = {y: 2, z: 3}; // Two objects for testing
let a = getPropertyNames(o); // a == ["x"]; get o's properties in a new array
getPropertyNames(p, a); // a == ["x","y","z"]; add p's properties to it
```

When designing functions with optional arguments, you should be sure to put the optional ones at the end of the argument list so that they can be omitted.

### Default Parameters

```jsx
// This function returns an object representing a rectangle's 2 dimensions.
// If only width is supplied, make it twice as high as it is wide.
const rectangle = (width, height=width*2) => ({width, height});
rectangle(1) // => { width: 1, height: 2 }

// Append the names of the enumerable properties of object o to the
// array a, and return a. If a is omitted, create and return a new array.
function getPropertyNames(o, a = []) {
for(let property in o) a.push(property);
return a;
}
```

## Rest Parameters and Variable-Length Argument Lists

Rest parameters allow us to write functions that can be invoked with arbitrarily more arguments than parameters.

```jsx
function max(first=-Infinity, ...rest) {
	let maxValue = first; // Start by assuming the first arg is biggest
// Then loop through the rest of the arguments, looking for bigger
	for(let n of rest) {
		if (n > maxValue) {
			maxValue = n;
		}
	}
// Return the biggest
	return maxValue;
}
max(1, 10, 100, 2, 3, 1000, 4, 5, 6) // => 1000
```

## The arguments object

Before we had rest parameters they used to use this.

```jsx
function max(x) {
	let maxValue = -Infinity;
	// Loop through the arguments, looking for, and remembering, the biggest.
	for(let i = 0; i < arguments.length; i++) {
	if (arguments[i] > maxValue) maxValue = arguments[i];
	}
	// Return the biggest
	return maxValue;
}
max(1, 10, 100, 2, 3, 1000, 4, 5, 6) // => 1000
```

## Spread operator for Function calls

Spread operator unpacks or spread out the elements of an iterable object. 

```jsx
let numbers = [5, 2, 10, -1, 9, 100, 1];
Math.min(...numbers) // => -1
```

Don’t confuse spread and rest operator when using with functions. Spread operator is used in function calls, rest operator is used in function definitions.

## Destructuring Function arguments into Parameters

An example where rest parameters and destructuring are used together

```jsx
// Multiply the vector {x,y} or {x,y,z} by a scalar value, retain other props
function vectorMultiply({x, y, z=0, ...props}, scalar) {
	return { x: x*scalar, y: y*scalar, z: z*scalar, ...props
};
}
vectorMultiply({x: 1, y: 2, w: -1}, 2) // => {x: 2, y: 4, z: 0, w: -1}
```

## Argument Types

An example of a function that performs type-checking

```jsx
// Return the sum of the elements an iterable object a.
// The elements of a must all be numbers.
function sum(a) {
	let total = 0;
	for(let element of a) { // Throws TypeError if a is not iterable
		if (typeof element !== "number") {
			throw new TypeError("sum(): elements must be numbers");
		}
		total += element;
	}
	return total;
}

sum([1,2,3]) // => 6
sum(1, 2, 3); // !TypeError: 1 is not iterable
sum([1,2,"3"]); // !TypeError: element 2 is not a number
```

# Functions as Values

In JS functions are not just syntax. They are values, so they can: 

1. be assigned to variables
2. stored in the properties of objects or the elements of arrays
3. passed as arguments to functions, and so on…

What we can do when functions are used as values

```jsx
// We define some simple functions here
function add(x,y) { return x + y; }
function subtract(x,y) { return x - y; }
function multiply(x,y) { return x * y; }
function divide(x,y) { return x / y; }
// Here's a function that takes one of the preceding functions
// as an argument and invokes it on two operands
function operate(operator, operand1, operand2) {
	return operator(operand1, operand2);
}
// We could invoke this function like this to compute the value (2+3) + (4*5):
let i = operate(add, operate(add, 2, 3), operate(multiply, 4, 5));
// For the sake of the example, we implement the simple functions again,
// this time within an object literal;
const operators = {
	add: (x,y) => x+y,
	subtract: (x,y) => x-y,
	multiply: (x,y) => x*y,
	divide: (x,y) => x/y,
	pow: Math.pow // This works for predefined functions too
};
// This function takes the name of an operator, looks up that operator
// in the object, and then invokes it on the supplied operands.
// Note the syntax used to invoke the operator function.
function operate2(operation, operand1, operand2) {
	if (typeof operators[operation] === "function") {
		return operators[operation](operand1, operand2);
	}
	else throw "unknown operator";
}
operate2("add", "hello", operate2("add", " ", "world")) // => "hello world"
operate2("pow", 10, 2) // => 100
```

## Defining your own Function Properties \\

In JS Functions are a specialized kind of object, so they can have properties. 

# Functions as Namespaces \\

Variables declared inside a function are not visible outside.

### IIFE

Immediately Invoked Function Expression is a JavaScript function that runs as soon as it is defined.

```jsx
(function () {...})() 
```

# Closures \\

Before you understand closures, you need to understand execution context. The environment in which code is executed is very important. It’s either:

- global code - default environment
- function code - when it’s inside a function body

Execution context: the environment / scope the current code is being evaluated in. In other words: we start a program, we start in global execution context. We call the variables created in this context - global variables. 

When we call a function a few steps happen:

1. JavaScript creates a local execution context
2. That context will have its own set of variables.
3. The new execution context is thrown onto the *execution stack*. Think of the execution stack as a mechanism to keep track of where the program is in its execution.

The function ends when it meets `return`or a closing bracket `}`. If the function has no `return` statement, `undefined` is returned. At the end the local execution context is destroyed. All the variables that were declared within the local execution context are no longer available.

A closure is a function defined inside another function and has access to its lexical scope even when it is executing outside its lexical scope. The closure has access to variables in three scopes:

- Variables declared in its own scope
- Variables declared in the scope of the parent function
- Variables declared in the global scope

In JavaScript, all functions are closures because they have access to the outer scope, but most functions don't utilise the usefulness of closures: the persistence of state. Closures are also sometimes called stateful functions because of this.

In addition, closures are the only way to store private data that can't be accessed from the outside in JavaScript. They are the key to the UMD (Universal Module Definition) pattern, which is frequently used in libraries that only expose a public API but keep the implementation details private, preventing name collisions with other libraries or the user's own code.

- Closures are useful because they let you associate data with a function that operates on that data.
- A closure can substitute an object with only a single method.
- Closures can be used to emulate private properties and methods.

---

A closure is the combination of a function and the lexical environment within which that function was declared. i.e, It is an inner function that has access to the outer or enclosing function’s variables. The closure has three scope chains

1. Own scope where variables defined between its curly brackets
2. Outer function’s variables
3. Global variables

Let's take an example of closure concept,

```jsx
function Welcome(name) {
  var greetingInfo = function (message) {
    console.log(message + " " + name);
  };
  return greetingInfo;
}
var myFunction = Welcome("John");
myFunction("Welcome "); //Output: Welcome John
myFunction("Hello Mr."); //output: Hello Mr.John
```

As per the above code, the inner function(i.e, greetingInfo) has access to the variables in the outer function scope(i.e, Welcome) even after the outer function has returned.

# Function properties, Methods and Constructor

## length

`length` property of a function shows the number of parameters it declares.

If a function has a rest parameter, that parameter is not counted for `length`

## name

This shows the name of the function.

It is useful for debugging and making errors. 

## prototype

Every function has a different prototype object. 

When a function is used as a constructor, the newly created object inherits properties from the prototype object.

## call() and apply() \\ reread

Allow you to indirectly call a function. 

## bind()

Purpose: bind function to an object. 

```jsx
function f(y) { return this.x + y; } // This function needs to be bound
let o = { x: 1 }; // An object we'll bind to
let g = f.bind(o); // Calling g(x) invokes f() on o
g(2) // => 3
let p = { x: 10, g }; // Invoke g() as a method of this object
p.g(2) // => 3: g is still bound to o, not p.
```

**Call:** The call() method invokes a function with a given `this` value and arguments provided one by one

```jsx
var employee1 = { firstName: "John", lastName: "Rodson" };
var employee2 = { firstName: "Jimmy", lastName: "Baily" };

function invite(greeting1, greeting2) {
  console.log(
    greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
  );
}

invite.call(employee1, "Hello", "How are you?"); // Hello John Rodson, How are you?
invite.call(employee2, "Hello", "How are you?"); // Hello Jimmy Baily, How are you?
```

**Apply:** Invokes the function with a given `this` value and allows you to pass in arguments as an array

```jsx
var employee1 = { firstName: "John", lastName: "Rodson" };
var employee2 = { firstName: "Jimmy", lastName: "Baily" };

function invite(greeting1, greeting2) {
  console.log(
    greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
  );
}

invite.apply(employee1, ["Hello", "How are you?"]); // Hello John Rodson, How are you?
invite.apply(employee2, ["Hello", "How are you?"]); // Hello Jimmy Baily, How are you?
```

**bind:** returns a new function, allowing you to pass any number of arguments

```jsx
var employee1 = { firstName: "John", lastName: "Rodson" };
var employee2 = { firstName: "Jimmy", lastName: "Baily" };

function invite(greeting1, greeting2) {
  console.log(
    greeting1 + " " + this.firstName + " " + this.lastName + ", " + greeting2
  );
}

var inviteEmployee1 = invite.bind(employee1);
var inviteEmployee2 = invite.bind(employee2);
inviteEmployee1("Hello", "How are you?"); // Hello John Rodson, How are you?
inviteEmployee2("Hello", "How are you?"); // Hello Jimmy Baily, How are you?
```

Call and apply are pretty interchangeable. Both execute the current function immediately. You need to decide whether it’s easier to send in an array or a comma separated list of arguments. You can remember by treating Call is for **comma** (separated list) and Apply is for **Array**.

Whereas Bind creates a new function that will have `this` set to the first parameter passed to bind().

## toString()

## Function() Constructor

# Functional Programming \\

One of the main concepts in functional programming is DECLARATIVE programming. In declarative programming we stay away from `if` statements and use ternary operator instead. We stay away from `for` loops, and use `forEach` method instead, etc.

Avoids concepts of *shared state* and *mutable data* observed in OOP. \\

## General Terms

### Pure and Impure functions

Pure

It can only work on the data that was passed to it. It cannot work on any external data. It has to solely depend upon its input.

```jsx
function add(x,y) {
	return x+ y
}
// this is a pure function 
// no matter how many time we call this function
// it's going to give us the same result
```

Impure

This next example is an impure function, because:
- It relies upon external data (the console is external). 
- It also has a side effect. It writes something to the console. The function is mutating the state of our application by writing something to the console.

```jsx
function add2(x, y) {
console.log(x+y) //external & produces a side effect
return x+y
}
```

What would be considered an observable side effect??? Just about anything useful that we would need to do within an application: HTTP requests, reading or writing to local storage, querying or manipulating the DOM, or even just simply changing state. All of that is considered an observable side effect and it makes our functions impure. When we are writing a program, we want as many pure functions as possible, because they are consistent, BUT in order to do anything meaningful, we will have to write impure functions.

### Immutability

If something is immutable it cannot be mutated - it cannot be changed, it’s consistent and it’s safe.

If you want to make an object immutable, you can use `Object.freeze`.

```jsx
const urls = {posts: ''}
Object.freeze(urls)
urls.posts = 'sssss' // doesn't do anything 
```

## Processing Arrays with Functions

Finding the mean: nonfunctional style

```jsx
let data = [1,1,3,5,5];
let total = 0;
for(let i = 0; i < data.length; i++) total += data[i];
let mean = total/data.length; // mean == 3; The mean of our data is 3
```

Finding the mean: functional style

```jsx
const sum = (x,y) => x+y;
let data = [1,1,3,5,5];
let mean = data.reduce(sum)/data.length; // mean == 3
```

### Higher-order Functions

Functions are values and just like strings or numbers they can be assigned to variables or passed into other function.

```jsx
let triple = function(x) {
 return x*3
}

let waffle = triple
waffle(30)
```

Higher-order functions are functions that accept other function as an argument and/or returns a function. For examples array methods are higher-order functions.

If functions were not first class objects in JS we wouldn’t have been able to pass them as arguments or assign them to variables. 

In functional programming we have to think of functions as objects.

## Partial Application of Functions \\

## Memoization \\

Memoization is a programming technique which attempts to increase a function’s performance by caching its previously computed results. Each time a memoized function is called, its parameters are used to index the cache. If the data is present, then it can be returned, without executing the entire function. Otherwise the function is executed and then the result is added to the cache. Let's take an example of adding function with memoization.

```jsx
const memoizAddition = () => {
  let cache = {};
  return (value) => {
    if (value in cache) {
      console.log("Fetching from cache");
      return cache[value]; // Here, cache.value cannot be used as property name starts with the number which is not a valid JavaScript  identifier. Hence, can only be accessed using the square bracket notation.
    } else {
      console.log("Calculating result");
      let result = value + 20;
      cache[value] = result;
      return result;
    }
  };
};
// returned function from memoizAddition
const addition = memoizAddition();
console.log(addition(20)); //output: 40 calculated
console.log(addition(20)); //output: 40 cached
```

## Summary

You can define functions with the function keyword and with the ES6 => arrow syntax.

You can invoke functions, which can be used as methods and constructors.

Some ES6 features allow you to define default values for optional function parameters, to gather multiple arguments into an array using a rest parameter, and to destructure object and array arguments into function parameters.

You can use the ... spread operator to pass the elements of an array or other iterable object as arguments in a function invocation.

A function defined inside of and returned by an enclosing function retains access to its lexical scope and can therefore read and write the variables defined inside the outer function. Functions used in this way are called closures, and this is a technique that is worth understanding.

Functions are objects that can be manipulated by JavaScript, and this enables a functional style of programming.