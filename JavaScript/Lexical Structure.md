## Main Concepts:

JavaScript does not consider white space meaningful. Spaces and line breaks can be added in any fashion you might like.

JavaScript is case sensitive. A variable named something is different from Something.

We define as **literal** a value that is written in the source code, for example, a number, a string, a boolean or also more advanced constructs, like Object Literals or Array Literals.

An **identifier** is a sequence of characters that can be used to identify a variable, a function, or an object. It can start with a letter, the dollar sign $ or an underscore _, and it can contain digits. Using Unicode, a letter can be any allowed char, for example, an emoji 😄.

Semicolons: use them per your wish.

A `hello` string is a **value**. A number like `12` is a **value**. `hello` and `12` are values. `string` and `number` are the **types** of those values.

### The text

JS is case-sensitive.

JS ignores spaces and for the most part line breaks.

In addition to the regular space character (\u0020), JavaScript also recognizes tabs, assorted ASCII control characters, and various Unicode space characters as whitespace. JavaScript recognizes newlines, carriage returns, and a carriage return/line feed sequence as line terminators.

### Comments

```jsx
// This is a single-line comment.
/* This is also a comment */ // and here is another comment.
/*
* This is a multi-line comment. The extra * characters at
the start of
* each line are not a required part of the syntax; they just
look cool!
*/
```

### Literals

A literal is a data value that appears directly in the program. 

```jsx
12 // The number twelve
1.2 // The number one point two
"hello world" // A string of text
'Hi' // Another string
true // A Boolean value
false // The other Boolean value
null // Absence of an object
```

## Identifiers and Reserved words

### Legal Identifiers

```jsx
i my_variable_name
v13
_dummy
$str
```

### Reserved words

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a55cd53c-418c-46d5-8c37-cac4a5aefbb4/Untitled.png)

Words not currently used, but reserved for future…

`enum`, `implements`, `interface`, `package`, `private`, `protected`, `public`, `arguments`, `eval`

## Unicode

JS allows Unicode letters, digits, and ideographs (but not
emojis) in identifiers. But the programming convention is to use **only ASCII** letters and digits. 

```jsx
const π = 3.14;
const sí = true;

// this is accepted in the language
// but it's not a good idea to use them 
```

### Unicode Escape Sequences

Some computes cannot display Unicode, so we have escape sequences. They allow you to use Unicode only with ASCII characters. 

```jsx
let café = 1; // Define a variable using a Unicode character
caf\u00e9 // => 1; access the variable using an escape sequence
caf\u{E9} // => 1; another form of the same escape sequence
```

Before ES6 JS supported only four-digit escape seq. Now we can support more with curly braces. 

```jsx
console.log("\u{1F600}"); // Prints a smiley face emoji
```

### Unicode Normalization

```jsx
const café = 1; // This constant is named "caf\u{e9}"
const café = 2; // This constant is different: "cafe\u{301}"
café // => 1: this constant has one value
café // => 2: this indistinguishable constant has a different value
```

If you plan to use Unicode characters in your JavaScript programs, you should ensure that your editor or some other tool performs Unicode normalization of your source code to prevent you from ending up with different but visually indistinguishable identifiers.

## Optional Semicolons

You can omit semicolons.

```jsx
// here you can omit semicolons, becuase they're on diff lines
a = 3;
b = 4;
// here you can't omit the semicolon
a = 3; b = 4;
```

JavaScript treats a line break as a semicolon if the next nonspace character cannot be interpreted as a continuation of the current statement.

```jsx
let a
a
=
3
console.log(a)

// js will interpret it like this

let a; a = 3; console.log(a);

// js treats the first line break as a semicolon 
// because it cannot parse the code let a a without a semicolon
// the second a could stand alone as the statement a
// but JavaScript does not treat the second line break as a semicolon 
// because it can continue parsing the longer statement a = 3;.
```

Surprising case:

```jsx
let y = x + f
(a+b).toString()

// will be interpreted  like this 

let y = x + f(a+b).toString(); // which is a problem!
```

If a statement begins with `( [ / + -` there is a chance that it could be interpreted as a continuation of the statement before.

Three exceptions to the general rule:

- `return throw yield break continue` statements - if a line break appears after any of these words (before any other tokens), JavaScript will always interpret that line break as a semicolon, so DO NOT INSERT A LINE BREAK

```jsx
return 
true;

//js sees as

return; true;
```

- `++ --` operators - if you want to use either of these operators as postfix operators, they must appear on the same line as the expression they apply to.
- `=>` arrow function syntax - must appear on the same line as the parameter list

You must not insert a line break between return, break, or continue and the expression that follows the keyword.

The second exception involves the ++ and −− operators