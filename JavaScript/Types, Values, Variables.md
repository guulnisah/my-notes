# Overview and Definitions

JavaScript is a *loosely typed* and *dynamic* language. Variables in JavaScript are not directly associated with any particular value type, and any variable can be assigned (and re-assigned) values of all types *data* is anything that is meaningful to the computer.

Eight different *data types* which are:
Primitive data types:`boolean string symbol bigint number`
Special types: `undefined null`
Objects (collection of properties) - if it’s not primitive it’s an `object`

ES6 has added a new special-purpose type called `Symbol`. 

Primitives are *immutable*. Object types are *mutable*.

Functions and classes are not just syntax, they are values. They are a specialized kind of object.

Generally, any value that’s not of a primitive type (a string, a number, a boolean, null or undefined) is an object. Object types have properties and also have methods that can act on those properties.

`undefined null` are the only values that methods cannot be invoked on

Some other useful object types: `Set Map RegExp Date Error`

The JavaScript interpreter performs **automatic garbage collection** for memory management. We do not need to worry about destruction or deallocation of objects or other values. When a value is no longer reachable the interpreter automatically reclaims the memory it was occupying.

# Types

Primitive Types

Whenever we try to access a method or property on a primitive, that primitive is wrapped into an object. That's called autoboxing. ***Autoboxing*** will connect the primitive to the related built-in prototype object.

```jsx
let num = 0;
let obj = new String('0');
let str = '0';

console.log(num === num); // true
console.log(obj === obj); // true
console.log(str === str); // true

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false
console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```

```jsx
let num = 0;
var obj = new String('0');
var str = '0';

console.log(num === num); // true
console.log(obj === obj); // true
console.log(str === str); // true

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false
console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```

## Number - numeric literal

JavaScript recognizes base-10 integers, hexadecimal (base-16) values. In ES6 and later, you can also express integers in binary (base 2) or octal (base 8) using the prefixes 0b and 0o (or 0B and 0O) instead of 0x.

### Integer literals

base-10 integer

```jsx
0
3
10000000
```

hexadecimal values begin with 0x

```jsx
0xff // => 255: (15*16 + 15)
0xBADCAFE // => 195939070
```

binary (base 2) - begins with 0b (or 0B)

octal (base 8) - begins with 0o (or 0O)

```jsx
0b10101 // => 21: (1*16 + 0*8 + 1*4 + 0*2 + 1*1)
0o377 // => 255: (3*64 + 7*8 + 7*1)
```

### Floating-Point Literals

```jsx
3.14
2345.6789
.333333333333333333
6.02e23 // 6.02 × 10²³
1.4738223E-32 // 1.4738223 × 10⁻³²
// e represents exponential notation
```

**ECMAScript 2021:** **Numeric separators** - used for bigger numbers to visually separate them. They don't change the actual code or its meaning.

```jsx
let billion = 1_000_000_000; // Underscore as a thousands separator.
let bytes = 0x89_AB_CD_EF; // As a bytes separator.
let bits = 0b0001_1101_0111; // As a nibble separator.
let fraction = 0.123_456_789; // Works in the fractional part, too.
```

### Arithmetic

Math Object

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3290c9a-9095-452c-89bd-869f78d5205b/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f98a4bc6-571b-4bae-bc0d-38006241f07d/Untitled.png)

No errors in case of overflow, underflow or division by zero. If overflow: `Infinity`. If underflow:`-Infinity`

Division by zero is not an error in JavaScript: it simply returns infinity or negative infinity. 

You get `NaN` if you 

- divide zero by zero
- divide infinity by infinity,
- take the square root of a negative number,
- use arithmetic operators with non-numeric operands that cannot be converted to numbers.

`NaN` is not equal to any other value, including itself. This means that you can’t write `x === NaN` to determine whether the value of a variable x is `NaN`. Instead, you must write `x != x` or `Number.isNaN(x)`. Those expressions will be true if, and only if, x has the same value as the global constant `NaN`.

`isNaN()` is similar to `Number.isNaN()`. Returns true if:

- its argument is NaN
- if that argument is a nonnumeric value that cannot be converted to a number.

`Number.isFinite()` returns true if 

- its argument is a number other than NaN, Infinity, or -Infinity.

The global `isFinite()` function returns true if 

- its argument is, or can be converted to, a finite number.

### Binary Floating-Point and Rounding Errors \\\

### Arbitrary Precision Integers with BigInt \\\

### Dates and Times

JavaScript defines a simple `Date` class for representing and manipulating the numbers that represent dates and times. JavaScript Dates are objects, but they also have a numeric representation as a timestamp that specifies the number of elapsed milliseconds since January 1, 1970.

```jsx
let timestamp = Date.now(); // The current time as a timestamp (a number).
let now = new Date(); // The current time as a Date object.
let ms = now.getTime(); // Convert to a millisecond timestamp.
let iso = now.toISOString(); // Convert to a string in standard format.
```

### Methods

The `**toString()**` returns a number as a String

```jsx
let x = 123;
x.toString();
(123).toString();
(100 + 23).toString();
```

The `toExponential()` returns a string, with a number rounded and written using exponential notation

```jsx
let x = 9.656;
x.toExponential(2);
x.toExponential(4);
x.toExponential(6);

// 9.656e+0
// 9.66e+0
// 9.6560e+0
// 9.656000e+0
```

The **`toFixed()`** method formats a number using fixed-point notation.

```jsx
function financial(x) {
  return Number.parseFloat(x).toFixed(2);
}

console.log(financial(123.456));
// expected output: "123.46"

console.log(financial(0.004));
// expected output: "0.00"

console.log(financial('1.23e+5'));
// expected output: "123000.00"
```

The `toPrecision()` returns a string, with a number written with a specified length:

```jsx
let x = 9.656;
x.toPrecision();
x.toPrecision(2);
x.toPrecision(4);
x.toPrecision(6);
```

### Converting Variables to Numbers

There are 3 JavaScript methods that can be used to convert variables to numbers:

| Method | Description |
| --- | --- |
| Number() | Returns a number, converted from its argument. |
| parseFloat() | Parses its argument and returns a floating point number |
| parseInt() | Parses its argument and returns an integer |

```jsx
Number(true);
Number(false);
Number("10");
Number("  10");
Number("10  ");
Number(" 10  ");
Number("10.33");
Number("10,33"); //NaN
Number("10 33"); //NaN
Number("John"); //NaN

//If the number cannot be converted, NaN (Not a Number) is returned.
```

```jsx
parseInt("-10");
parseInt("-10.33");
parseInt("10");
parseInt("10.33");
parseInt("10 20 30");
parseInt("10 years");
parseInt("years 10"); // NaN
```

```jsx
parseFloat("10");
parseFloat("10.33");
parseFloat("10 20 30");
parseFloat("10 years");
parseFloat("years 10"); //NaN
```

### Number Properties

| Property | Description |
| --- | --- |
| MAX_VALUE | Returns the largest number possible in JavaScript |
| MIN_VALUE | Returns the smallest number possible in JavaScript |
| POSITIVE_INFINITY | Represents infinity (returned on overflow) |
| NEGATIVE_INFINITY | Represents negative infinity (returned on overflow) |
| NaN | Represents a "Not-a-Number" value |

## Text - string literals

The JavaScript type for representing text is the string.

The length of a string is the number of 16-bit values it contains. JavaScript’s strings (and its arrays) use zerobased indexing: the first 16-bit value is at position 0, the second at position 1, and so on. The empty string is the string of length 0. JavaScript does not have a special type that represents a single element of a string. 

```jsx
"" // The empty string: it has zero characters
'testing'
"3.14"
'name="myform"'
"Wouldn't you prefer O'Reilly's book?"
"τ is the ratio of a circle's circumference to its radius"
`"She said 'hi'", he said.`
```

When combining JavaScript and HTML, it is a good idea to use one style of quotes for JavaScript and the other style for HTML. In the following example, the string “Thank you” is single-quoted within a JavaScript expression, which is then double-quoted within an HTML event-handler attribute: `<button onclick="alert('Thank you')">Click Me</button>`

### Escape Sequences in String Literals

```jsx
'You\'re right, it can\'t be a quote'
"You're right, it can't be a quote"
```

- `\b`: backspace (U+0008 BACKSPACE)
- `\f`: form feed (U+000C FORM FEED)
- `\n`: line feed (U+000A LINE FEED)
- `\r`: carriage return (U+000D CARRIAGE RETURN)
- `\t`: horizontal tab (U+0009 CHARACTER TABULATION)
- `\v`: vertical tab (U+000B LINE TABULATION)
- `\0`: null character (U+0000 NULL) (only if the next character is not a decimal digit; else it’s an [octal escape sequence](https://mathiasbynens.be/notes/javascript-escapes#octal))
- `\'`: single quote (U+0027 APOSTROPHE)
- `\"`: double quote (U+0022 QUOTATION MARK)
- `\\`: backslash (U+005C REVERSE SOLIDUS)

If the \ character precedes any other character, the backslash is simply ignored (although future versions of the language may, of course, define new escape sequences).

### Working with Strings

```jsx
let s = "Hello, world"; // Start with some text.

// Obtaining portions of a string
s.substring(1,4) // => "ell": the 2nd, 3rd, and 4th characters.
s.slice(1,4) // => "ell": same thing
s.slice(-3) // => "rld": last 3 characters
s.split(", ") // => ["Hello", "world"]: split at delimiter string

// Searching a string
s.indexOf("l") // => 2: position of first letter l
s.indexOf("l", 3) // => 3: position of first "l" at or after 3
s.indexOf("zz") // => -1: s does not include the substring "zz"
s.lastIndexOf("l") // => 10: position of last letter l

// Boolean searching functions in ES6 and later
s.startsWith("Hell") // => true: the string starts with these
s.endsWith("!") // => false: s does not end with that
s.includes("or") // => true: s includes substring "or"

// Creating modified versions of a string
s.replace("llo", "ya") // => "Heya, world"
s.toLowerCase() // => "hello, world"
s.toUpperCase() // => "HELLO, WORLD"
s.normalize() // Unicode NFC normalization: ES6
s.normalize("NFD") // NFD normalization. Also "NFKC", "NFKD"

// Inspecting individual (16-bit) characters of a string
s.charAt(0) // => "H": the first character
s.charAt(s.length-1) // => "d": the last character
s.charCodeAt(0) // => 72: 16-bit number at the specified position
s.codePointAt(0) // => 72: ES6, works for codepoints > 16 bits

// String padding functions in ES2017
"x".padStart(3) // => " x": add spaces on the left to a length of 3
"x".padEnd(3) // => "x ": add spaces on the right to a length of 3
"x".padStart(3, "*") // => "**x": add stars on the left to a length of 3
"x".padEnd(3, "-") // => "x--": add dashes on the right to a length of 3

// Space trimming functions. trim() is ES5; others ES2019
" test ".trim() // => "test": remove spaces at start and end
" test ".trimStart() // => "test ": remove spaces on left. Also trimLeft
" test ".trimEnd() // => " test": remove spaces at right. Also trimRight

// Miscellaneous string methods
s.concat("!") // => "Hello, world!": just use + operator instead
"<>".repeat(5) // => "<><><><><>": concatenate n copies. ES6
```

Strings are immutable, so these methods return NEW strings, they don’t change the original string.

Strings can also be treated like read-only arrays, and you can access individual characters (16-bit values) from a string using square brackets instead of the charAt() method:

```jsx
let s = "hello, world";
s[0] // => "h"
s[s.length-1] // => "d"
```

### Template Literals

Everything between the ${ and the matching } is interpreted as a JavaScript expression. Everything outside the curly braces is normal string literal text. The expression inside the braces is evaluated and then converted to a string and inserted into the template, replacing the dollar sign, the curly braces, and everything in between them.

```jsx
let word1 = 'Dylan';
let word2 = 'Israel';

const fullName = `${word1} ${word2}`;
// you can easily combine variables 

let example = `${word1}
${word2}
`;

//OUTPUT 
// Dylan
//Israel 

//basically you can create multilign elements
```

Tagged Template Literals 

If a function name (or “tag”) comes right before the opening backtick,
then the text and the values of the expressions within the template
literal are passed to the function. The value of this “tagged template
literal” is the return value of the function. This could be used, for
example, to apply HTML or SQL escaping to the values before
substituting them into the text.

### Pattern Matching

RegExp is used for describing and matching patterns in strings of text.

## Boolean

In computer science, a **Boolean**  is a logical data type that can have only the values `true` or `false.`

The following values (falsy values) convert to, and therefore work like, false:
`undefined
null
0 -
0
NaN
"" // the empty string`

All other values are truthy.

Boolean values have a toString() method that you can use to convert them to the strings “true” or “false”. 

### Boolean Operators

The `&&` operator performs the Boolean AND operation. A truthy value if and only if both of its operands are truthy; it evaluates to a falsy value otherwise. `&&` has higher precedence.

The `||` operator is the Boolean OR. It evaluates to a truthy value if either one (or both) of its operands is truthy and evaluates to a falsy value if both operands are falsy. 

The unary `!` operator performs the Boolean NOT operation: it evaluates to true if its operand is falsy and evaluates to false if its operand is truthy. 

```jsx
if ((x === 0 && y === 0) || !(z === 0)) {
// x and y are both zero or z is non-zero
}
```

### null and undefined

null:

- used to indicate the absence of a value.
- Using the typeof operator on null returns the string “object”, indicating that null can be thought of as a special object value that indicates “no object”.
- In practice null is typically regarded as the sole member of its own type, and it can be used to indicate “no value” for numbers and strings as well as objects.

undefined: 

- represents a deeper kind of absence.
- It is the value of variables that have not been initialized and the value you get when you query the value of an object property or array element that does not exist.
- It is also the return value of functions that do not explicitly return a value
- it is the value of function parameters for which no argument is passed
- undefined is a predefined global constant (not a language keyword like null, though this is not an important distinction in practice) that is initialized to the undefined value.
- If you apply the typeof operator to the undefined value, it returns “undefined”, indicating that this value is the sole member of a special type.

I consider undefined to represent a system-level, unexpected, or error-like absence of value and null to represent a program-level, normal, or expected absence of value.

```jsx
typeof null          // "object" (not "null" for legacy reasons)
typeof undefined     // "undefined"
null === undefined   // false
null  == undefined   // true
null === null        // true
null  == null        // true
!null                // true
isNaN(1 + null)      // false
isNaN(1 + undefined) // true
```

## Symbols \\

To obtain a Symbol value, you call the Symbol() function.

This function never returns the same value twice, even when called with the same argument.

## Global Object

The global object is a regular JavaScript object that serves a very important purpose: the properties of this object are the globally defined identifiers that are available to a JavaScript program.

When the JavaScript interpreter starts (or whenever a web browser loads a new page), it creates a new global object and gives it an initial set of properties that define:

/code

- Global constants like undefined, Infinity, and NaN
- Global functions like isNaN(), parseInt() and eval()
- Constructor functions like Date(), RegExp(), String(), Object(), and Array()
- Global objects like Math and JSON

In web browsers, the Window object serves as the global object for all JavaScript code contained in the browser window it represents. This lobal Window object has a self-referential window property that can be used to refer to the global object.

ES2020 finally defines globalThis as the standard way to refer to the global object in any context. As of early 2020, this feature has been implemented by all modern browsers and by Node.

## Immutable Primitive Values and Mutable Object References

Primitives are immutable. They are also compared by value: two values are the same only if they have the same value.

Objects are mutable. They are not compared by value: two distinct objects are not equal even if they have the same properties and values. And two distinct arrays are not equal even if they have the same elements in the same order.

```jsx
let o = {x: 1}, p = {x: 1}; // Two objects with the same properties
o === p // => false: distinct objects are never equal
let a = [], b = []; // Two distinct, empty arrays
a === b // => false: distinct arrays are never equal
```

Objects are sometimes called reference types. Object values are references, and we say that objects are compared by reference: two object values are the same if and only if they refer to the same underlying object.

```jsx
let a = []; // The variable a refers to an empty array.
let b = a; // Now b refers to the same array.
b[0] = 1; // Mutate the array referred to by variable b.
a[0] // => 1: the change is also visible through variable a.
a === b // => true: a and b refer to the same object, so they are equal.
```

If you want to make a new copy of an object or array, you must explicitly copy the properties of the object or the elements of the array. This example demonstrates using a for loop:

```jsx
let a = ["a","b","c"]; // An array we want to copy
let b = []; // A distinct array we'll copy into
for(let i = 0; i < a.length; i++) { // For each index of a[]
b[i] = a[i]; // Copy an element of a into b
}
let c = Array.from(b); // In ES6, copy arrays with Array.from()
```

If we want to compare two distinct objects or arrays, we must compare their properties or elements.

```jsx
function equalArrays(a, b) {
	if (a === b) return true; // Identical arrays are equal
	if (a.length !== b.length) return false; // Differentsize arrays not equal
	for(let i = 0; i < a.length; i++) { // Loop through all elements
		if (a[i] !== b[i]) return false; // If any differ, arrays not equal
	}
	return true; // Otherwise they are equal
}
```

## Type Conversion \\

There are two types of type conversion in JavaScript.

- **Implicit Conversion** - automatic type conversion
- **Explicit Conversion** - manual type conversion

Implicit Conversion

```jsx
result = '3' + 2; 
console.log(result) // "32"

result = '3' + true; 
console.log(result); // "3true"

result = '3' + undefined; 
console.log(result); // "3undefined"

result = '3' + null; 
console.log(result); // "3null"

result = '4' - '2'; 
console.log(result); // 2

result = '4' - 2;
console.log(result); // 2

result = '4' * 2;
console.log(result); // 8

result = '4' / 2;
console.log(result); // 2

result = '4' - 'hello';
console.log(result); // NaN
```

```jsx
result = '4' - true;
console.log(result); // 3

result = 4 + true;
console.log(result); // 5

result = 4 + false;
console.log(result); // 4

result = 4 + null;
console.log(result);  // 4

result = 4 - null;
console.log(result);  // 4

result = 4 + undefined;
console.log(result);  // NaN

result = 4 - undefined;
console.log(result);  // NaN

result = true + undefined;
console.log(result);  // NaN

result = null + undefined;
console.log(result);  // NaN
```

Explicit Conversions

```jsx
result = Number('324');
console.log(result); // 324

result = Number('324e-1')  
console.log(result); // 32.4

result = Number(true);
console.log(result); // 1

result = Number(false);
console.log(result); // 0

result = Number(null);
console.log(result);  // 0

let result = Number(' ')
console.log(result);  // 0

result = Number('hello');
console.log(result); // NaN

result = Number(undefined);
console.log(result); // NaN

result = Number(NaN);
console.log(result); // NaN

[result = parseIn](https://www.notion.so/JavaScript-Basics-f5e285515eb24b7b8628e6d18de32561?pvs=21)t('20.01');
console.log(result); // 20

result = parseFloat('20.01');
console.log(result); // 20.01

result = +'20.01';
console.log(result); // 20.01

result = Math.floor('20.01');
console.log(result); // 2

result = String(324);
console.log(result);  // "324"

result = String(2 + 4);
console.log(result); // "6"
```

```jsx
result = String(null);
console.log(result); // "null"

result = String(undefined);
console.log(result); // "undefined"

result = String(NaN);
console.log(result); // "NaN"

result = String(true);
console.log(result); // "true"

result = String(false);
console.log(result); // "false"

result = (324).toString();
console.log(result); // "324"

result = true.toString();
console.log(result); // "true"

result = Boolean('');
console.log(result); // false

result = Boolean(0);
console.log(result); // false

result = Boolean(undefined);
console.log(result); // false

result = Boolean(null);
console.log(result); // false

result = Boolean(NaN);
console.log(result); // false

result = Boolean(324);
console.log(result); // true

result = Boolean('hello');
console.log(result); // true

result = Boolean(' ');
console.log(result); // true
```

JavaScript is very flexible about the types of values it requires. 

When JavaScript expects a boolean value, you may supply a value of any type, and JavaScript will convert it as needed. 

Some values (“truthy” values) convert to true and others (“falsy” values) convert to false. 

If JavaScript wants a string, it will convert whatever value you give it to a string. 

If JavaScript wants a number, it will try to convert the value you give it to a number (or to NaN if it cannot perform a meaningful conversion).

```jsx
10 + " objects" // => "10 objects": Number 10 converts to a string
"7" * "4" // => 28: both strings convert to numbers
let n = 1 - "x"; // n == NaN; string "x" can't convert to a number
n + " objects" // => "NaN objects": NaN converts to string "NaN"
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/41aecd7e-82ef-4036-9bca-f9cd5f6a7809/Untitled.png)

The following values are **always falsy**:

- `false`
- `0` (zero)
- `0` (minus zero)
- `0n` (`BigInt` zero)
- `''`, `""`, ```` (empty string)
- `null`
- `undefined`
- `NaN`

Everything else is **truthy**. That includes:

- `'0'` (a string containing a single zero)
- `'false'` (a string containing the text “false”)
- `[]` (an empty array)
- `{}` (an empty object)
- `function(){}` (an “empty” function)

### Object to Primitive Conversions \\ read later

# Variable Declaration and Assignment

```jsx
let firstName = "nice"
//can be changed

var firstName = "nice"
//can be changed

const firstName = "nice"
//can not be changed
```

*Variables* allow computers to store and manipulate data in a dynamic fashion.

They do this by using a "label" to point to the data rather than using the data itself. Any of the eight data types may be stored in a variable. 

Variable names can be made up of numbers, letters, and `$` or `_`, but may not contain spaces or start with a number. When naming a function/property/variable - the first character ALWAYS a **letter/$/_.** Other symbols are unacceptable. Don’t forget the semicolon.

## Declaration with let, const and var

`const` ****does not mean the value cannot change - it means it cannot be reassigned. If the variable points to an object or an array the content of the object or the array can freely change.

```jsx
// const variables must be initialized at the declaration time
const a = 0

// const can not be reassigned to a new value
a = 'smth' // WRONG

// let values can be initialized later
// they will be undefined until you assigne them a value
let a
a = 0

// can declare multiple variables at once
const a = 1,  b = 2, c = 3

// cannot redeclare the same variable more than one time 
// you will get duplicate declaration error
let a = 1
let a = 2 // WRONG
a = 2 // OK
```

```jsx
// const variables must be initialized at the declaration time
const a = 0

// const can not be reassigned to a new value
a = 'smth' // WRONG

// let values can be initialized later
// they will be undefined until you assigne them a value
let b
b = 0

// can declare multiple variables at once
const c = 1,  d = 2, e = 3

// cannot redeclare the same variable more than one time 
// you will get duplicate declaration error
let f = 1
let f = 2 // WRONG
f = 2 // OK
```

General advice: **always use** `const` **and only use** `let` **when you know you’ll need to reassign a value to that variable.**

If you create a variable with `const` it is a signal to another developer that this variable is not to be reassigned, it is to stay the same. But if you use `let` they understand that this variable will be reassigned later in your code.

It is a common (but not universal) convention to declare constants using names with all capital letters such as `H0` or `HTTP_NOT_FOUND` as a way to distinguish them from variables.

In `for` loops we create a loop variable. We can declare it as a part of the loop syntax.

```jsx
for(let i = 0, len = data.length; i < len; i++)
console.log(data[i]);
for(let datum of data) console.log(datum);
for(let property in object) console.log(property);
```

### VARIABLE AND CONSTANT SCOPE

let and const are block scoped. This means that they are only defined within the block of code in which the let or const statement appears. JavaScript class and function definitions are blocks, and so are the bodies of if/else statements, while loops, for loops, and so on. ROUGHLY speaking, if a variable or constant is declared within a set of curly braces, then those curly braces delimit the region of code in which the variable or constant is defined.

When a declaration appears at the top level, outside of any code blocks, we say it is a global variable or constant and has global scope.

### REPEATED DECLARATIONS

It is a syntax error to use the same name with more than one let or const declaration in the same scope. It is legal (though a practice best avoided) to declare a new variable with the same name in a nested scope:

```jsx
const x = 1; // Declare x as a global constant
if (x === 1) {
let x = 2; // Inside a block x can refer to a different value
console.log(x); // Prints 2
}
console.log(x); // Prints 1: we're back in the global scope now
let x = 3; // ERROR! Syntax error trying to redeclare x
```

### DECLARATIONS AND TYPES

A JavaScript variable can hold a value of any type. For example, it is perfectly legal (but generally poor programming style) in JavaScript to assign a number to a variable and then later assign a string to that variable

USING UNDECLARED VARIABLES

In strict mode, if you attempt to use an undeclared variable, you’ll get a reference error when you run your code. Outside of strict mode, if you assign a value to a name that has not been declared with let, const, or var, you’ll end up creating a new global variable. It will be a global no matter now deeply nested within functions and blocks your code is, which is almost certainly not what you want, is bug-prone, and is one of the best reasons for using strict mode!!!

### **What is the difference between let and var**

| var | let |
| --- | --- |
| It is been available from the beginning of JavaScript | Introduced as part of ES6 |
| It has function scope, it is scoped to the body of the containing function no matter how deeply nested it is inside that function. | It has block scope. To simplify, curly braces are gates for it.  |
| Variables will be hoisted | Hoisted but not initialized |
| Globals declared with var are implemented as properties of the global object | Global variables and constants declared with let and const are not properties of the global object. |

Let's take an example to see the difference

```jsx
function userDetails(username) {
  if (username) {
    console.log(salary); // undefined due to hoisting
    console.log(age); // ReferenceError: Cannot access 'age' before initialization
    let age = 30;
    var salary = 10000;
  }
  console.log(salary); //10000 (accessible to due function scope)
  console.log(age); //error: age is not defined(due to block scope)
}
userDetails("John");
```

### **What is the Temporal Dead Zone**

The Temporal Dead Zone is a behavior in JavaScript that occurs when declaring a variable with the let and const keywords, but not with var. In ECMAScript 6, accessing a `let` or `const` variable before its declaration (within its scope) causes a ReferenceError. The time span when that happens, between the creation of a variable’s binding and its declaration, is called the temporal dead zone.

Let's see this behavior with an example

```jsx
function somemethod() {
  console.log(counter1); // undefined
  console.log(counter2); // ReferenceError
  var counter1 = 1;
  let counter2 = 2;
}
```

## Destructuring Assignment

```jsx
let [x,y] = [1,2]; // Same as let x=1, y=2
[x,y] = [x+1,y+1]; // Same as x = x + 1, y = y + 1
[x,y] = [y,x]; // Swap the value of the two variables
[x,y] // => [3,2]: the incremented and swapped values
```

Here is a code that loops over the name/value pairs of all properties of an object and uses destructuring assignment to convert those pairs from two-element arrays into individual variables:

```jsx
let o = { x: 1, y: 2 }; // The object we'll loop over
for(const [name, value] of Object.entries(o)) {
console.log(name, value); // Prints "x 1" and "y 2"
}
```

The number of variables on the left of a destructuring assignment does not have to match the number of array elements on the right. Extra variables on the left are set to undefined, and extra values on the right are ignored.

```jsx
let [x,y] = [1]; // x == 1; y == undefined
[x,y] = [1,2,3]; // x == 1; y == 2
[,x,,y] = [1,2,3,4]; // x == 2; y == 4
```

If you want to collect all unused or remaining values into a single
variable when destructuring an array, use three dots (...) before the
last variable name on the left-hand side:

```jsx
let [x, ...y] = [1,2,3,4]; // y == [2,3,4]
```

Another example with the rest parameter

```jsx
const source = [1,2,3,4,5,6,7,8,9,10];
function removeFirstTwo(list) {
  const [a, b, ...arr] = list; 
// first two values are stored inside a and b 
// other values are stored inside of an array called arr
  return arr;
// you get your new array without the first two variables
}
const arr = removeFirstTwo(source);
```

Destructuring assignment can be used with nested arrays. In this case,
the lefthand side of the assignment should look like a nested array
literal:

```jsx
let [a, [b, c]] = [1, [2,2.5], 3]; // a == 1; b == 2; c == 2.5
```

You can use any iterable object on the righthand side of the assignment; any object that can be used with a for/of loop  can also be destructured:

```jsx
let [first, ...rest] = "Hello"; // first == "H"; rest == ["e","l","l","o"]
```

Even with objects

```jsx
const player = {
  name: 'Lebron James',
  club: 'LA Lakers',
  address: {
    city: 'Los Angeles'
  }
};

// console.log( player.address.city );

const { name, club, address: { city } } = player;

// console.log(`${name} plays for ${club}`);

console.log(`${name} lives in ${city}`);
```

```jsx
const favouriteFilm = {
    title: "Top Gun",
    year: "1986",
    genre: "action",
    star: "Tom Cruise",
    director: "Tony Scott"
} 

const {title, year, genre, star, director} = favouriteFilm

// const title = favouriteFilm.title
// const year = favouriteFilm.year
// const genre = favouriteFilm.genre 
// const star = favouriteFilm.star
// const director = favouriteFilm.director

console.log(`My favourite film is ${title} starring ${star}. It's an ${genre} film that was directed by ${director} and released in ${year}`)

//My favourite film is Top Gun starring Tom Cruise. It's an action film that was directed by Tony Scott and released in 1986
```

Destructuring allows you to assign a new variable name when extracting values. You can do this by putting the new name after a colon when assigning the value.

```jsx
const HIGH_TEMPERATURES = {
  yesterday: 75,
  today: 77,
  tomorrow: 80
};

const {today: highToday, tomorrow: highTomorrow} =  HIGH_TEMPERATURES
// highToday = HIGH_TEMPERATURES.today
// highTomorrow = HIGH_TEMPERATURES.tomorrow
```

The next example copies global functions of the Math object into variables, which might simplify code that does a lot of trigonometry. 

Notice in the code here that the Math object has many properties. Those that are not named are simply ignored. If the lefthand side of this assignment had included a variable whose name was not a property of Math, that variable would simply be assigned undefined.

```jsx
// Same as const sin=Math.sin, cos=Math.cos, tan=Math.tan
const {sin, cos, tan} = Math;
```

<aside>
⛔ It’s better no to do the kind of complex destructuring you’ll see next. It’s hard to read.

</aside>

Nested Object 

```jsx
const LOCAL_FORECAST = {
  yesterday: { low: 61, high: 75 },
  today: { low: 64, high: 77 },
  tomorrow: { low: 68, high: 80 }
};

const {today: {low: lowToday, high: highToday}} = LOCAL_FORECAST

// lowToday = LOCAL_FORECAST.today.low;
// highToday = LOCAL_FORECAST.today.high;

```

Destructuring an array of objects

```jsx
let points = [{x: 1, y: 2}, {x: 3, y: 4}]; // An array of two point objects
let [{x: x1, y: y1}, {x: x2, y: y2}] = points; // destructured into 4 variables.
(x1 === 1 && y1 === 2 && x2 === 3 && y2 === 4) // => true
```

Destructuring an object of arrays
```jsx
let points = { p1: [1,2], p2: [3,4] }; // An object with 2 array props
let { p1: [x1, y1], p2: [x2, y2] } = points; // destructured into 4 vars
(x1 === 1 && y1 === 2 && x2 === 3 && y2 === 4) // => true
```