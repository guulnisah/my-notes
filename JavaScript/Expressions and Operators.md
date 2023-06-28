# Expressions and Operators

An expression is a single unit of JavaScript code that the JavaScript engine can evaluate, and return a value.

Primary Expressions: The simplest expressions are those that stand alone—they do not include any simpler expressions. Primary expressions in JavaScript are constant or literal values, certain language keywords, and variable references.

**Object and array initializers** are expressions whose value is a newly created object or array: [] and {}. These initializer expressions are sometimes called object literals and array literals.

A **function definition expression** defines a JavaScript function, and the value of such an expression is the newly defined function. In a sense, a function definition expression is a “function literal” in the same way that an object initializer is an “object literal.”

# Primary Expressions

Primary expressions - constant or literal values. 

- number literals
- string literals
- some reserved words: true, false, null, this
- a reference to a variable, constant, or property of the global object

# Object and Array Initializers

Object and Array literals. Unlike true literals, however, they are not primary expressions, because they include a number of subexpressions that specify property and element values.

`[] {}`

# Function Definition Expressions

keyword function followed by a comma-separated list of zero or more identifiers (the parameter names) in parentheses and a block of JavaScript code (the function body) in curly braces

# Property Access Expression

A **property access expression** evaluates to the value of an object property or an array element. JavaScript defines two syntaxes for property access:

```jsx
expression . identifier
expression [ expression ]
```

## Conditional Property Access

```jsx
expression ?. identifier
expression ?.[ expression ]
```

In JavaScript, the values `null` and `undefined` are the **only** two values that do not have properties. In a regular property access expression using . or [], you get a TypeError if the expression on the left evaluates to null or undefined. You can use `?.` and `?.[]` syntax to guard against errors of this type.

```jsx
let a = { b: null };
a.b?.c.d // => undefined
```

`a` is an object, `a.b` is a valid property access expression the value of `a.b` is `null`, so `a.b.c` would throw a TypeError. 

By using `?.` instead of `.` we avoid the TypeError, and `a.b?.c` evaluates to `undefined`

`(a.b?.c).d` will throw a TypeError, because it tries to access undefined. But `a.b?.c.d` (without the parentheses) simply evaluates to `undefined` and does not throw an error. 

This is because property access with `?.` is “short-circuiting”: if the subexpression to the left of `?.` evaluates to `null` or `undefined`, then the entire expression immediately evaluates to `undefined` without any further property access attempts.

```jsx
let a = { b: {} };
a.b?.c?.d // => undefined
```

Conditional property access is also possible using ?.[] instead of []. In the expression a?.[b][c], if the value of a is null or undefined, then the entire expression immediately evaluates to undefined, and subexpressions b and c are never even evaluated.

```jsx
let a; // Oops, we forgot to initialize this variable!
let index = 0;
try {
a[index++]; // Throws TypeError
} catch(e) {
index // => 1: increment occurs before TypeError is thrown
}
a?.[index++] // => undefined: because a is undefined
index // => 1: not incremented because ?.[] short circuits
a[index++] // !TypeError: can't index undefined.
```

# Invocation Expression

Syntax for calling (or executing) a function or method. 

```jsx
functionName(argument1, argument2)
```

## Conditional Invocation

With the new `?.()` invocation syntax, if the expression to the left of the `?.` evaluates to null or undefined, then the entire invocation expression evaluates to undefined and no exception is thrown. If the value to the left of `?.` is null or undefined, then none of the argument expressions within the parentheses are evaluated.

```jsx
// before es2020

function square(x, log) { // The second argument is an
optional function
if (log) { // If the optional function is
passed
log(x); // Invoke it
}
return x * x; // Return the square of the
argument
}

// now
function square(x, log) { // The second argument is an optional function
log?.(x); // Call the function if there is one
return x * x; // Return the square of the argument
}
```

Understand the difference 

```jsx
o.m() // Regular property access, regular invocation
o?.m() // Conditional property access, regular invocation
o.m?.() // Regular property access, conditional invocation
```

# Object Creation Expression

An object creation expression creates a new object and invokes a function (called a constructor) to initialize the properties of that object. Object creation expressions are like invocation expressions except that they are prefixed with the keyword `new`:

```jsx
new Object()
new Point(2,3)
```

If no arguments are passed to the constructor function in an object creation expression, the empty pair of parentheses can be omitted:

```jsx
new Object
new Date
```

# Operator Overview

In this table operators are listed from high precedence to low precedence. 

![F5126F26-91A3-4C17-9B84-E265FA50AC4F.jpeg](Expressions%20and%20Operators/F5126F26-91A3-4C17-9B84-E265FA50AC4F.jpeg)

## Number of Operands

Operators can be categorized based on the number of operands they expect (their arity). Most operators are *binary:* they expect two operands. **

There are also *unary* operators, *ternary* operator.

## Operand and Result Type

Operators usually do type conversion. `'3' * '5'` is a legal, `*` will convert the strings to numbers. 

Some operators react differently to different operand types. Examples: 

- `+` will add numeric values but also concatenate strings
- `<` perform comparison in numerical or alphabetical order depending on the type of the operands

lvalue is a historical term that means “an expression that can legally appear on the left side of an assignment expression.” In JavaScript, variables, properties of objects, and elements of arrays are lvalues.

## Operator Side Effects

Examples:

- The assignment operators: if you assign a value to a variable or property, that changes the value of any expression that uses that variable or property.
- The `++` and `--` increment and decrement operators are similar, since they perform an implicit assignment.
- The `delete` operator: deleting a property is like (but not the same as) assigning undefined to the property.

## Operator Precedence

Operators with higher precedence (nearer the top of the table) are performed before those with lower precedence (nearer to the bottom).

```jsx
w = x + y*z;
// * is performed first
// + is second
// = has the lowest precedence, so it's the last here
```

Can be overridden with parentheses 

```jsx
w = (x + y)*z;
// here the + will be performed first 
```

Property access and invocation expressions have higher precedence than any of the operators listed above 

```jsx
// my is an object with a property named functions whose value is an
// array of functions. We invoke function number x, passing it argument
// y, and then we ask for the type of the value returned.
typeof my.functions[x](y)

// typeof is one of the highest-priority operators
// but it's performed the last here 
```

<aside>
☝🏻 JUST USE PARENTHESES IF YOU ARE UNSURE

</aside>

## Operator Associativity

In the table above we have a column A for *associativity*. A value of L specifies left-to-right associativity, and a value of R specifies right-to-left associativity. Left-to-right associativity means that operations are performed from left to right.

The following expressions have right-to-left associativity.

```jsx
y = a ** b ** c;
x = ~-y;
w = x = y = z;
q = a?b:c?d:e?f:g;

// what actually happens

y = (a ** (b ** c));
x = ~(-y);
w = (x = (y = z));
q = a?b:(c?d:(e?f:g));
```

## Order of Evaluation

Adding parentheses to the expressions can change the relative order of the multiplication, addition, and assignment, but not the left-to-right order of evaluation.

```jsx
w = x + y * z

// the subexpression w is evaluated first, followed by x, y, and z
// the values of y and z are multiplied, 
// added to the value of x, 
// and assigned to the variable or property specified by expression w
```

Order of evaluation only makes a difference if any of the expressions being evaluated has side effects that affect the value of another expression. If expression x increments a variable that is used by expression z, then the fact that x is evaluated before z is important.

# Arithmetic Expressions

Arithmetic operators are used to perform **arithmetic calculations.**

| Operator | Name | Example |
| --- | --- | --- |
| + | Addition | x + y |
| - | Subtraction | x - y |
| * | Multiplication | x * y |
| / | Division | x / y |
| % | Remainder | x % y |
| ++ | Increment (increments by 1) | ++x or x++ |
| -- | Decrement (decrements by 1) | --x or x-- |
| ** | Exponentiation (Power) | x ** y |

General facts:

1. The ** operator has higher precedence than *, /, and % (which in turn
have higher precedence than + and -).
2. ** works right-to-left, so 2****2****3 is the same as 2****8, not 4****3.
3. Depending on the relative precedence of unary minus and exponentiation, that expression could mean (-3)****2 or -(3****2).  JavaScript simply makes it a syntax error to omit parentheses in this case, forcing you to write an unambiguous expression.
4. In JavaScript all numbers are floating-point, so all division operations have floating-point results: 5/2 evaluates to 2.5, not 2. 
5. Division by zero yields positive or negative infinity, while 0/0 evaluates to NaN: neither of these cases raises an error.
6. `%` operator computes the first operand modulo the second operand: it returns the remainder after whole-number division of the first operand by the second operand; `%` also works with floating-point values

### Precedence rules

Every complex statement with multiple operators in the same line will introduce precedence problems. Operations on the same level (like `+`
 and `-`) are executed in the order they are found, from left to right.

| Operator | Description |
| --- | --- |
| * / % | multiplication/division |
| + - | addition/subtraction |
| = | assignment |

## The + operator

adds numeric operands or concatenates string operands

The conversion rules for + give priority to string concatenation: if either of the operands is a string or an object that converts to a string, the other operand is converted to a string and concatenation is performed. Addition is performed only if neither operand is string-like.

Order is also important 

```jsx
1 + 2 + " blind mice" // => "3 blind mice"
1 + (2 + " blind mice") // => "12 blind mice"
```

## Unary operators: Arithmetic and Bitwise

The arithmetic unary operators (+, -, ++, and --) all convert their single operand to a number, if necessary.

| Operator | Explanation |
| --- | --- |
| Unary plus (+) | Tries to convert the operand into a number. When used with a number - does nothing. |
| Unary negation (-) | Tries to convert the operand into a number and negates after |
| Increment (++) | Adds one to its operand; converts the operand to a number.  |
| Decrement (--) | Decrements by one, converts the operand to number.  |
| Logical NOT (!) | Converts to boolean value then negates it |
| Bitwise NOT (~) | Inverts all the bits in the operand and returns a number |
| typeof | Returns a string which is the type of the operand |
| delete | Deletes specific index of an array or specific property of an object |
| void | Discards a return value of an expression. |

Bitwise operators do not perform traditional arithmetic operations, they are categorized as arithmetic operators here because they operate on numeric operands and return a numeric value.

- `++x` (pre-increment) means "increment the variable; the value of the expression is the final value"
- `x++` (post-increment) means "remember the original value, then increment the variable; the value of the expression is the original value"

```jsx
let i = 1, j = ++i; // i and j are both 2
// increments the operand 
// evaluates to the incremented value of that operand
let n = 1, m = n++; // n is 2, m is 1
// increments its operand
// evaluates to the unincremented value of that operand
```

Another example showing the difference:

```jsx
x = 0;
y = array[x++]; // This will get array[0]

x = 0;
y = array[++x]; // This will get array[1]
```

This rule also applies to `--`.

## Bitwise Operators \\

# Relational Expressions

These operators test for a relationship (such as “equals,” “less than,” or “property of”) between two values and return true or false depending on whether that relationship exists.

## Equality and Inequality Operators

| Operator | Description | Example |
| --- | --- | --- |
| == | Equal to: returns true if the operands are equal | x == y |
| != | Not equal to: returns true if the operands are not equal | x != y |
| === | Strict equal to: true if the operands are equal and of the same type | x === y |
| !== | Strict not equal to: true if the operands are equal but of different type or not equal at all | x !== y |
| > | Greater than: true if left operand is greater than the right operand | x > y |
| >= | Greater than or equal to: true if left operand is greater than or equal to the right operand | x >= y |
| < | Less than: true if the left operand is less than the right operand | x < y |
| <= | Less than or equal to: true if the left operand is less than or equal to the right operand | x <= y |

<aside>
⚠️ THE =, ==, AND === OPERATORS
JavaScript supports =, ==, and === operators. Be sure you understand the differences between these assignment, equality, and strict equality operators, and be careful to use the correct one when coding! Although it is tempting to read all three operators as “equals,” it may help to reduce confusion if you read “gets” or “is assigned” for =, “is equal to” for ==, and “is strictly equal to” for ===. The == operator is a legacy feature of JavaScript and is widely considered to be a source of bugs. You should almost always use === instead of ==, and !== instead of !=.

</aside>

RULES OF STRICT EQUALITY

1. If the two values have different types, they are not equal.
2. If both values are null or both values are undefined, they are equal. 
3. If both values are the boolean value true or both are the boolean value false, they are equal.
4. If one or both values is NaN, they are not equal. (This is surprising, but the NaN value is never equal to any other value, including itself! To check whether a value x is NaN, use x !== x, or the global isNaN() function.)
5. If both values are numbers and have the same value, they are equal. If one value is 0 and the other is -0, they are also equal.
6. If both values are strings and contain exactly the same 16-bit values (see the sidebar in §3.3) in the same positions, they are equal. If the strings differ in length or content, they are not equal. Two strings may have the same meaning and the same visual appearance, but still be encoded using different sequences of 16-bit values. JavaScript performs no Unicode normalization, and a pair of strings like this is not considered equal to the === or == operators.
7. If both values refer to the same object, array, or function, they are equal. If they refer to different objects, they are not equal, even if both objects have identical properties.

### **What is the difference between == and === operators**

JavaScript provides both strict(===, !==) and type-converting(==, !=) equality comparison. The strict operators take type of variable in consideration, while non-strict operators make type correction/conversion based upon values of variables. The strict operators follow the below conditions for different types,

1. Two strings are strictly equal when they have the same sequence of characters, same length, and same characters in corresponding positions.
2. Two numbers are strictly equal when they are numerically equal. i.e, Having the same number value. There are two special cases in this,
    1. NaN is not equal to anything, including NaN.
    2. Positive and negative zeros are equal to one another.
3. Two Boolean operands are strictly equal if both are true or both are false.
4. Two objects are strictly equal if they refer to the same Object.
5. Null and Undefined types are not equal with ===, but equal with ==. i.e, null===undefined --> false but null==undefined --> true

Some of the example which covers the above cases,

```jsx
0 == false   // true
0 === false  // false
1 == "1"     // true
1 === "1"    // false
null == undefined // true
null === undefined // false
'0' == false // true
'0' === false // false
[]==[] or []===[] //false, refer different objects in memory
{}=={} or {}==={} //false, refer different objects in memory
```

## Comparison Operators

`<` means “less than”
`<=` means “minus than, or equal to”
`>` means “greater than”
`>=` means “greater than, or equal to”

## Comparison and Conversion (p. 170)

- If either operand evaluates to an object, that object is converted to a primitive value;
    - if its valueOf() method returns a primitive value, that value is used;
    - otherwise, the return value of its toString() method is used.
- If, after any required object-to-primitive conversion, both operands are strings, the two strings are compared, using alphabetical order, where “alphabetical order” (defined by Unicode)
- If, after object-to-primitive conversion, at least one operand is not a string, both operands are converted to numbers and compared numerically.
    - 0 and -0 are considered equal
    - Infinity is larger than any number other than itself
    - - Infinity is smaller than any number other than itself
    - If either operand is (or converts to) NaN, then the comparison operator always returns false.
    - Although the arithmetic operators do not allow BigInt values to be mixed with regular numbers, the comparison operators do allow comparisons between numbers and BigInts.

<aside>
☝🏻 String comparison is just a numerical comparison of the values in the two strings. The numerical encoding order is defined by Unicode. String comparison is case-sensitive, and all capital ASCII letters are “less than” all lowercase ASCII letters. For example, according to the `<` operator, the string “Zoo” comes before the string “aardvark”. To avoid that use either `toLowerCase()` or `toUpperCase()` method.

</aside>

```jsx
1 + 2 // => 3: addition.
"1" + "2" // => "12": concatenation.
"1" + 2 // => "12": 2 is converted to "2".
11 < 3 // => false: numeric comparison.
"11" < "3" // => true: string comparison.
"11" < 3 // => false: numeric comparison, "11" converted to 11.
"one" < 3 // => false: numeric comparison, "one" converted to NaN.
```

## in Operator

The in operator expects a left-side operand that is a string, symbol, or value that can be converted to a string. It expects a right-side operand that is an object. It evaluates to true if the left-side value is the name of a property of the right-side object. 

```jsx
let point = {x: 1, y: 1}; // Define an object
"x" in point // => true: object has property named "x"
"z" in point // => false: object has no "z" property.
"toString" in point // => true: object inherits toString method
let data = [7,8,9]; // An array with elements (indices) 0, 1, and 2
"0" in data // => true: array has an element "0"
1 in data // => true: numbers are converted to strings
3 in data // => false: no element 3
```

## instanceof Operator

On the left: operand that is an object.

On the right: operand that identifies a class of objects.

If the object on the left is the instance of the class on the right, then this operator returns true. 

If the left-side operand of instanceof is not an object, instanceof returns false. If the righthand side is not a class of objects, it throws a TypeError.

```jsx
let d = new Date(); // Create a new object with the Date() constructor
d instanceof Date // => true: d was created with Date()
d instanceof Object // => true: all objects are instances of Object
d instanceof Number // => false: d is not a Number object
let a = [1, 2, 3]; // Create an array with array literal syntax
a instanceof Array // => true: a is an array
a instanceof Object // => true: all arrays are objects
a instanceof RegExp // => false: arrays are not regular expressions
```

# Logical Expressions

The logical operators &&, ||, and ! perform Boolean algebra.

## Logical AND

The `&&` operator performs the Boolean AND operation. A truthy value if and only if both of its operands are truthy; it evaluates to a falsy value otherwise. `&&` has higher precedence.

```jsx
x === 0 && y === 0 // true if, and only if, x and y are both 0
```

```jsx
let o = {x: 1};
let p = null;
o && o.x // => 1: o is truthy, so return value of o.x
p && p.x // => null: p is falsy, so return it and don't evaluate p.x

if (a === b) stop(); // Invoke stop() only if a === b
(a === b) && stop(); // This does the same thing
```

## Logical OR

The `||` operator is the Boolean OR. It starts by evaluating the left. 

If left is truthy, doesn’t evaluate the right, returns true. 

If the left is falsy, evaluates the right and returns the right.

```jsx
// If maxWidth is truthy, use that. 
// Otherwise, look for a value in the preferences object. 
// If that is not truthy, use a hardcoded constant.
let max = maxWidth || preferences.maxWidth || 500;
```

## Logical NOT

The unary `!` operator performs the Boolean NOT operation: it evaluates to true if its operand is falsy and evaluates to false if its operand is truthy. 

`!` operator converts its operand to a boolean value

`!` has high precedence and binds tightly

```jsx
// DeMorgan's Laws
!(p && q) === (!p || !q) // => true: for all values of p and q
!(p || q) === (!p && !q) // => true: for all values of p and q
```

# Assignment Expressions

JavaScript uses the = operator to assign a value to a variable or property.

```jsx
i = 0; // Set the variable i to 0.
o.x = 1; // Set the property x of object o to 1.
```

`=` has very low precedence, and parentheses are usually necessary in a larger expression.

The assignment operator has right-to-left associativity. You can write code like this to assign a single value to multiple variables:

```jsx
i = j = k = 0; // Initialize 3 variables to 0
```

## Assignment with Operation

![Untitled](Expressions%20and%20Operators/Untitled.png)

In most cases, the expression: `a op= b` where op is an operator, is equivalent to the expression: `a = a op b` In the first line, the expression a is evaluated once. In the second, it is evaluated twice. 

The two cases will differ only if a includes side effects such as a function call or an increment operator. The following two assignments, for example, are not the same:

```jsx
data[i++] *= 2;
data[i++] = data[i++] * 2;
```

# Evaluation Expressions \\

## eval()

## Global eval()

## Strcit eval()

# Miscellaneous Operators

## The Conditional Operator (?:)

Also called *ternary* operator. 

```jsx
greeting = "hello " + (username ? username : "there");

// is the same as

greeting = "hello ";
if (username) {
greeting += username;
} else {
greeting += "there";
}
```

## First-Defined (??)

If its left operand is not null and not undefined, it returns that value.
Otherwise, it returns the value of the right operand. 

It only evaluates its second operand if the first operand evaluates to null or undefined. If the expression `a` has no side effects, then the expression `a ?? b` is equivalent to:

```jsx
(a !== null && a !== undefined) ? a : b
```

Is an alternative to || when you want to select the first defined operand rather that the first truthy operand. 

```jsx
let max = maxWidth || preferences.maxWidth || 500;
// zero is invalid 

let max = maxWidth ?? preferences.maxWidth ?? 500;
//zero is valid
```

```jsx
let options = { timeout: 0, title: "", verbose: false, n: null };
options.timeout ?? 1000 // => 0: as defined in the object
options.title ?? "Untitled" // => "": as defined in the object
options.verbose ?? true // => false: as defined in the object
options.quiet ?? false // => false: property is not defined
options.n ?? 10 // => 10: property is null
```

### Precedence

```jsx
(a ?? b) || c // ?? first, then ||
a ?? (b || c) // || first, then ??
a ?? b || c // SyntaxError: parentheses are required
```

## The typeof Operator

![DC81123C-DA30-4AF7-8ABB-559D49DB0AF0.jpeg](Expressions%20and%20Operators/DC81123C-DA30-4AF7-8ABB-559D49DB0AF0.jpeg)

```jsx
// If the value is a string, wrap it in quotes, otherwise, convert
(typeof value === "string") ? "'" + value + "'" : value.toString()
```

Although JavaScript functions are a kind of object, the typeof operator considers functions to be sufficiently different that they have their own return value.

In order to distinguish one class of object from another, you must use other techniques, such as the `instanceof` operator, the `class` attribute, or the `constructor` property.

## The delete Operator

`delete` is a unary operator that attempts to delete the object property or array element specified as its operand

delete expects its operand to be an lvalue.

```jsx
let o = { x: 1, y: 2}; // Start with an object
delete o.x; // Delete one of its properties
typeof o.x; // Property does not exist; returns "undefined".
"x" in o // => false: the property does not exist anymore
let a = [1,2,3]; // Start with an array
delete a[2]; // Delete the last element of the array
2 in a // => false: array element 2 doesn't exist anymore
a.length // => 3: note that array length doesn't change, though

delete 1; // This makes no sense, but it just returns true.
// Can't delete a variable; returns false, or SyntaxError in strict mode.
delete o;
// Undeletable property: returns false, or TypeError in strict mode.
delete Object.prototype;
```

## The await Operator

Used for asynchronous programming. 

## The void Operator

`void` is a unary operator that appears before its single operand, which may be of any type. This operator is unusual and infrequently used; it evaluates its operand, then discards the value and returns undefined. Since the operand value is discarded, using the void operator makes sense only if the operand has side effects. 

Example case: when you want to define a function that returns nothing but also uses the arrow function shortcut syntax where the body of the function is a single expression that is evaluated and returned. If you are evaluating the expression solely for its side effects and do not want to return its value, then the simplest thing is to use curly braces around the function body. But, as an alternative, you could also use the void operator in this case:

```jsx
let counter = 0;
const increment = () => void counter++;
increment() // => undefined
counter // => 1
```

## The comma Operator (,)

The lefthand expression is always evaluated, but its value is discarded, which means that it only makes sense to use the comma operator when the lefthand expression has side effects. The only situation in which the comma operator is commonly used is with a for loop that has multiple loop variables:

```jsx
// The first comma below is part of the syntax of the let statement
// The second comma is the comma operator: it lets us squeeze 2 
// expressions (i++ and j--) into a statement (the for loop) that expects 1.
for(let i=0,j=10; i < j; i++,j--) {
	console.log(i+j);
}
```