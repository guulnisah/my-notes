# General

An array is an ordered collection of values. 

Each value is called an *element*, and each element has a numeric position in the array, known as its *index*. 

JavaScript arrays are *untyped*: an array element may be of any type, and different elements of the same array may be of different types. Array elements may even be objects or other arrays, which allows you to create complex data structures, such as arrays of objects and arrays of arrays.

JavaScript arrays are *zero-based* and use 32-bit indexes: the index of the first element is 0, and the highest possible index is 4294967294 (2^32 −2), for a maximum array size of 4,294,967,295 elements.

JavaScript arrays are dynamic: they grow or shrink as needed, and there is no need to declare a fixed size for the array when you create it or to reallocate it when the size changes.

JavaScript arrays may be sparse: the elements need not have contiguous indexes, and there may
be gaps.

Arrays inherit properties from Array.prototype, which defines a rich set of array manipulation methods.

## Creating Arrays

There are four ways to create arrays.

### Array Literals

Comma-separated elements in square brackets.

```jsx
let empty = []; // An array with no elements
let primes = [2, 3, 5, 7, 11]; // An array with 5 numeric elements
let misc = [ 1.1, true, "a", ]; // 3 elements of various types + trailing comma

let base = 1024;
let table = [base, base+1, base+2, base+3]; // the values can be arbitrary

let b = [[1, {x: 1, y: 2}], [2, {x: 3, y: 4}]]; //can contain object literals or other array literals
```

If an array literal contains multiple commas in a row, with no value between, the array is sparse

```jsx
let count = [1,,3]; // Elements at indexes 0 and 2. No element at index 1
let undefs = [,,]; // An array with no elements but a length of 2
```

Array literal syntax allows an optional trailing comma, so [,,] has a length of 2, not 3.

### spread operator

```jsx
let original = [1,2,3];
let copy = [...original];
copy[0] = 0; // Modifying the copy does not change the original
original[0] // => 1
```

Strings are iterable, so you can use a spread operator to turn any string into an array of single-character strings:

```jsx
let digits = [..."0123456789ABCDEF"];
digits // ["0","1","2","3","4","5","6","7","8","9","A","B","C","D","E","F"]
```

### Array() constructor

Can invoke in three different ways:

```jsx
let a = new Array()
// creates an empty array, equivalent to the array literal 

let a = new Array(10)
// creates an array with specified length, no values are stored in the array

let a = new Array(5, 4, 3, 2, 1, 'testing')
// the constructor arguments become the elements of the array 
```

If you pass one argument ⇒ length; if you pass several ⇒ elements.

Using an array literal is almost always simpler than this usage of the Array() constructor.

### Array.of()

If you pass one argument to Array() you specify the length, you can’t make an array with just one element in it. To resolve that we have Array.of(). Arguments you pass will become the elements of the array, no mater the amount.

```jsx
Array.of() // => []; returns empty array with no arguments
Array.of(10) // => [10]; can create arrays with a single numeric argument
Array.of(1,2,3) // => [1, 2, 3]
```

### Array.from()

A simple way to make a copy of an array.

```jsx
let copy = Array.from(original);
```

You can pass a function as a second argument:

```jsx
Array.from([1, 2, 3], (x) => x + x); // [2, 4, 6]
```

The purpose of `Array.from()` is to take a non-array (but array-like) object and make a copy of it into an actual array. This then allows you to use ALL array methods on the copy including things beyond just iterating it such as `.splice()`, `.sort()`, `.push()`, `.pop()`, etc... which is obviously much more capable than just make `.map()` work with array-like things.

# Reading and writing Array elements

You can use this syntax to both read and write the value of an element of an array.

```jsx
let a = ["world"]; // Start with a one-element array
let value = a[0]; // Read element 0
a[1] = 3.14; // Write element 1
let i = 2;
a[i] = 3; // Write element 2
a[i + 1] = "hello"; // Write element 3
a[a[i]] = a[0]; // Read elements 0 and 2, write element 3
```

The fact that array indexes are simply a special type of object property name means that JavaScript arrays have no notion of an “out of bounds” error. When you try to query a nonexistent property of any object, you don’t get an error; you simply get undefined. This is just as true for arrays as it is for objects:

```jsx
let a = [true, false]; // This array has elements at indexes 0 and 1
a[2] // => undefined; no element at this index.
a[-1] // => undefined; no property with thisname.
```

## Sparse arrays

```jsx
let a = new Array(5); // No elements, but a.length is 5.
a = []; // Create an array with no elements and length = 0.
a[1000] = 0; // Assignment adds one element but sets length to 1001.

let a1 = [,]; // This array has no elements and length 1
let a2 = [undefined]; // This array has one undefined element
0 in a1 // => false: a1 has no element with index 0
0 in a2 // => true: a2 has the undefined value at index 0
```

## Array length

An array (sparse or not) will never have an element whose index is greater than or equal to its length.

If you create an element and assign it an index higher than the `length`the length will increase automatically. 

If you set the `length` to a number n smaller than its current value, all of the elements with an index smaller than n will be deleted.

If you set the `length` to a number that is higher than the current value, JS will not add any new elements to your array. You will create a sparse area at the end of your array. 

## Adding and Deleting Array Elements

### Adding

Just assign values to new indexes.

```jsx
let a = []; // Start with an empty array.
a[0] = "zero"; // And add elements to it.
a[1] = "one";
```

You can also use `push()` method.

```jsx
let a = []; // Start with an empty array
a.push("zero"); // Add a value at the end. a =["zero"]
a.push("one", "two"); // Add two more values. a = ["zero", "one", "two"]
```

You can use the `unshift()` method to insert a value at the beginning of an array, shifting the existing array elements to higher indexes. 

### Deleting

To delete from the end use `pop()`, to delete from the beginning use `shift()`.

You can delete array elements with the delete operator.

```jsx
let a = [1,2,3];
delete a[2]; // a now has no element at index 2
2 in a // => false: no array index 2 is defined
a.length // => 3: delete does not affect array length
```

Note that using delete on an array element does not alter the length property and does not shift elements with higher indexes down to fill in the gap that is left by the
deleted property. If you delete an element from an array, the array becomes sparse. BASICALLY you set the deleted value to `undefined`.

You can also remove elements from the end of an
array simply by setting the length property to the new desired length.

`splice()` is the general-purpose method for inserting,
deleting, or replacing array elements.

## Iterating Arrays

### for/of loop

The easiest way to loop through an array is `for/of` 

If you want to use a for/of loop for an array and need to know the index of each array element, use the entries() method of the array, along with destructuring assignment, like this:

```jsx
let letters = [..."Hello world"];
let everyother = "";

for(let [index, letter] of letters.entries()) {
if (index % 2 === 0) everyother += letter; // letters at even indexes
}
everyother // => "Hlowrd"
```

### forEach()

You pass a function to the `forEach()` method of an array, and `forEach()` invokes your function once on each element of the array. 

`forEach()` iterates the array in order, and it actually passes the array index to your function as a second argument.

```jsx
let letters = [..."Hello world"];
let uppercase = "";
letters.forEach(letter => { // Note arrow function syntax here
uppercase += letter.toUpperCase();
});
uppercase // => "HELLO WORLD"
```

Unlike the `for/of` loop, the `forEach()` is aware of sparse arrays and does not invoke your function for elements that are not there.

Ordinary for loop: if you want to skip undefined elements 

```jsx
for(let i = 0; i < a.length; i++) {
if (a[i] === undefined) continue; // Skip undefined + nonexistent elements
// loop body here
}
```

## Multidimensional Arrays

JavaScript does not provide the multidimensional array natively.

But you can create an array inside of an array,... So a JavaScript multidimensional array is an array of arrays.

```jsx
let activities = []; // To declare an empty multidimensional array

//example defines a two-dimensional array named activities:
//the first dimension represents the activity 
//the second one shows the number of hours spent per day for each.
let activities = [
    ['Work', 9],
    ['Eat', 1],
    ['Commute', 2],
    ['Play Game', 1],
    ['Sleep', 7]
];

console.table(activities); 
// show the activ arr in the console, use console.table()

┌─────────┬─────────────┬───┐
│ (index) │      0      │ 1 │
├─────────┼─────────────┼───┤
│    0    │   'Work'    │ 9 │
│    1    │    'Eat'    │ 1 │
│    2    │  'Commute'  │ 2 │
│    3    │ 'Play Game' │ 1 │
│    4    │   'Sleep'   │ 7 │
└─────────┴─────────────┴───┘ // the output 

//To access an element of the multidimensional array,
//use square brackets to access an element of the outer array 
//that returns an inner array; 
//and then use another square bracket to access 
//the element of the inner array.

console.log(activities[0][1]); // 9
```

### **Adding elements to the JavaScript multidimensional array**

```jsx
//to add a new element at the end of the multidimensional array, 
//you use the push()

activities.**push**(['Study', 2]);

┌─────────┬─────────────┬───┐
│ (index) │      0      │ 1 │
├─────────┼─────────────┼───┤
│    0    │   'Work'    │ 9 │
│    1    │    'Eat'    │ 1 │
│    2    │  'Commute'  │ 2 │
│    3    │ 'Play Game' │ 1 │
│    4    │   'Sleep'   │ 7 │
│    5    │   'Study'   │ 2 │
└─────────┴─────────────┴───┘

//insert an element in the middle of the array, you use the splice()

activities.splice(1, 0, ['Programming', 2]);

┌─────────┬───────────────┬───┐
│ (index) │       0       │ 1 │
├─────────┼───────────────┼───┤
│    0    │    'Work'     │ 9 │
│    1    │ 'Programming' │ 2 │
│    2    │     'Eat'     │ 1 │
│    3    │   'Commute'   │ 2 │
│    4    │  'Play Game'  │ 1 │
│    5    │    'Sleep'    │ 7 │
│    6    │    'Study'    │ 2 │
└─────────┴───────────────┴───┘

//This example calculates the percentage of the hours spent 
//on each activity and appends the percentage to the inner array.

activities.forEach(activity => {
    let percentage = ((activity[1] / 24) * 100).toFixed();
    activity[2] = percentage + '%';
});

┌─────────┬───────────────┬───┬───────┐
│ (index) │       0       │ 1 │   2   │
├─────────┼───────────────┼───┼───────┤
│    0    │    'Work'     │ 9 │ '38%' │
│    1    │ 'Programming' │ 2 │ '8%'  │
│    2    │     'Eat'     │ 1 │ '4%'  │
│    3    │   'Commute'   │ 2 │ '8%'  │
│    4    │  'Play Game'  │ 1 │ '4%'  │
│    5    │    'Sleep'    │ 7 │ '29%' │
│    6    │    'Study'    │ 2 │ '8%'  │
└─────────┴───────────────┴───┴───────┘
```

### **Removing elements from the JavaScript multidimensional array**

```jsx
activities.pop();
//removes the last element of the activities array
┌─────────┬───────────────┬───┬───────┐
│ (index) │       0       │ 1 │   2   │
├─────────┼───────────────┼───┼───────┤
│    0    │    'Work'     │ 9 │ '38%' │
│    1    │ 'Programming' │ 2 │ '8%'  │
│    2    │     'Eat'     │ 1 │ '4%'  │
│    3    │   'Commute'   │ 2 │ '8%'  │
│    4    │  'Play Game'  │ 1 │ '4%'  │
│    5    │    'Sleep'    │ 7 │ '29%' │
└─────────┴───────────────┴───┴───────┘

activities.forEach((activity) => {
    activity.pop(2);
});
//you can remove the elements from the inner array of the 
//multidimensional array by using the pop() method. 
//The following example removes the percentage element 
//from the inner arrays of the activities array
┌─────────┬───────────────┬───┐
│ (index) │       0       │ 1 │
├─────────┼───────────────┼───┤
│    0    │    'Work'     │ 9 │
│    1    │ 'Programming' │ 2 │
│    2    │     'Eat'     │ 1 │
│    3    │   'Commute'   │ 2 │
│    4    │  'Play Game'  │ 1 │
│    5    │    'Sleep'    │ 7 │
└─────────┴───────────────┴───┘

```

### **Iterating over elements of the JavaScript multidimensional array**

```jsx
for (let i = 0; i < activities.length; i++) {
    // get the size of the inner array
    var innerArrayLength = activities[i].length;
    // loop the inner array
    for (let j = 0; j < innerArrayLength; j++) {
        console.log('[' + i + ',' + j + '] = ' + activities[i][j]);
    }}

//the output 
[0,0] = Work
[0,1] = 9
[1,0] = Eat
[1,1] = 1
[2,0] = Commute
[2,1] = 2
[3,0] = Play Game
[3,1] = 1
[4,0] = Sleep
[4,1] = 7
[5,0] = Study
[5,1] = 2

//OR you can do this 
activities.forEach((activity) => {
    activity.forEach((data) => {
        console.log(data);
    });
});

//the output 
Work
9
Eat
1
Commute
2
Play Game
1
Sleep
7
Study
2
```

## Array-Like Objects

Arrays have some special features:

- The length property is automatically updated as new elements are added to the list.
- Setting length to a smaller value shortens the array.
- Arrays inherit useful methods from Array.prototype.
- Array.isArray() returns true for arrays.

These “array-like” objects actually do occasionally appear in practice, and although you cannot directly invoke array methods on them or expect special behavior from the length property, you can still iterate through them with the same code you’d use for a true array.

In client-side JavaScript, a number of methods for working with HTML documents (such as document.querySelectorAll(), for example) return array-like objects. Here’s a function you might use
to test for objects that work like arrays:

```jsx
// Determine if o is an array-like object.
// Strings and functions have numeric length properties, but are
// excluded by the typeof test. In client-side JavaScript, DOM text
// nodes have a numeric length property, and may need to be excluded
// with an additional o.nodeType !== 3 test.
function isArrayLike(o) {
if (o && // o is not null,
undefined, etc.
typeof o === "object" && // o is an object
Number.isFinite(o.length) && // o.length is a finite number
o.length >= 0 && // o.length is nonnegative
Number.isInteger(o.length) && // o.length is an integer
o.length < 4294967295) { // o.length < 2^32 - 1
return true; // Then o is arraylike.
} else {
return false; // Otherwise it is
not.
}
}
```

We’ll see in a later section that strings behave like arrays. Nevertheless, tests like this one for array-like objects typically return false for strings—they are usually best handled as strings, not as arrays.

Most JavaScript array methods are purposely defined to be generic so that they work correctly when applied to array-like objects in addition to true arrays. Since array-like objects do not inherit from Array.prototype, you cannot invoke array methods on them directly. You can invoke them indirectly using the Function.call method, however………

## Strings as Arrays

JavaScript strings behave like read-only arrays.

```jsx
// two ways to access characters in a string

let s = "test";
s.charAt(0) // => "t"
s[1] // => "e"
```

The fact that strings behave like arrays also means, however, that we can apply generic array
methods to them. For example:

```jsx
Array.prototype.join.call("JavaScript", " ") // => "J a v a S c r i p t"
```

Keep in mind that strings are immutable values, so when they are treated as arrays, they are read-only arrays. Array methods like push(), sort(), reverse(), and splice() modify an array in place and do not work on strings. Attempting to modify a string using an array method does not, however, cause an error: it simply fails silently.

# Array Methods

Here’s a list of array methods that mutate the array they’re called on:

- **[Array.prototype.pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)**
- **[Array.prototype.push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)**
- **[Array.prototype.shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)**
- **[Array.prototype.unshift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)**
- **[Array.prototype.reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)**
- **[Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)**
- **[Array.prototype.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)**

Slightly confusingly, arrays also have some methods that don’t mutate the original array, but return a new array instead:

- **Array.prototype.slice()**
- **Array.prototype.concat()**
- **Array.prototype.map()**
- **Array.prototype.filter()**

## Array Iterator Methods

General info:
1. all of these methods accept a function as their first argument and invoke that function once for each element
2. if the array is sparse, the function you pass is not invoked for nonexistent elements
3. In most cases, the function you supply is invoked with three arguments: the value of the array element, the index of the array element, and the array itself. Often, you only need the first of these argument values and can ignore the second and third values.

### forEach()

```jsx
let data = [1,2,3,4,5], sum = 0;
// Compute the sum of the elements of the array
data.forEach(value => { sum += value; }); // sum == 15
// Now increment each array element
data.forEach(function(v, i, a) { a[i] = v + 1; }); // data == [2,3,4,5,6]
```

Note that forEach() does not provide a way to terminate iteration before all elements have been passed to the function.

### map()

The map() method passes each element of the array on which it is invoked to the function you specify and returns an array containing the values returned by your function.

```jsx
let a = [1, 2, 3];
a.map(x => x*x) // => [1, 4, 9]: the function takes input x and returns x*x
```

<aside>
⛔ Note that map() returns a new array: it does not modify the array it is invoked on. If that array is sparse, your function will not be called for the missing elements, but the returned array will be sparse in the same way as the original array: it will have the same length and the same missing elements.

</aside>

1. What does the `.map()` array method do?
Returns a new array. Whatever gets returned from the callback
function provided is placed at the same index in the new array.
Usually we take the items from the original array and modify them
in some way.
2. What do we usually use `.map()` for in React?
Convert an array of raw data into an array of JSX elements
that can be displayed on the page.
3. Why is using `.map()` better than just creating the components
manually by typing them out?
It makes our code more "self-sustaining" - not requiring
additional changes whenever the data changes.

### filter()

The `filter()` method returns an array containing a subset of the elements of the array on which it is invoked. The function you pass to it should be predicate: a function that returns true or false. The
predicate is invoked just as for `forEach()` and `map()`. If the return value is true, or a value that converts to true, then the element passed to the predicate is a member of the subset and is added to the array that will become the return value.

```jsx
let a = [5, 4, 3, 2, 1];
a.filter(x => x < 3) // => [2, 1]; values less than 3

//longer 
a.filter(x => return x < 3 ? true : false) // there is no need for that

a.filter((x,i) => i%2 === 0) // => [5, 3, 1]; every other value
```

filter() skips missing elements in sparse arrays and that its return value is always dense. to close the gaps in a sparse array, you can do this:

```jsx
// to close the gaps in a sparse array
let dense = sparse.filter(() => true);

// to close gaps and remove undefined and null elements
a = a.filter(x => x !== undefined && x !== null);
```

It won’t return false values.

### find() and findIndex()

Similar to `filter()`:  they iterate through your array looking for elements for which your
predicate function returns a truthy value

BUT there is a difference: these two methods stop iterating the first time the predicate finds an
element; give you just one result;

If nothing is found: `find()` returns undefined and `findIndex()` returns -1:

```jsx
let a = [1,2,3,4,5];
a.findIndex(x => x === 3) // => 2; the value 3 appears at index 2
a.findIndex(x => x < 0) // => -1; no negative numbers in the array
a.find(x => x % 5 === 0) // => 5: this is a multiple of 5
a.find(x => x % 7 === 0) // => undefined: no multiples of 7 in the array
```

### every() and some()

They apply a predicate function you specify to the elements of the array, then return true or false.

They STOP iterating as soon as they know what value to return. By mathematical convention, every() returns true and some returns false when invoked on an empty array.

`every()` returns true only if your predicate function returns true for all elements in the array

```jsx
let a = [1,2,3,4,5];
a.every(x => x < 10) // => true: all values are < 10.
a.every(x => x % 2 === 0) // => false: not all values are even.
```

`some()` returns true if there exists at least one element in the array for which the predicate returns true and returns false if and only if the predicate returns false for all elements of the array

```jsx
let a = [1,2,3,4,5];
a.some(x => x%2===0) // => true; a has some even numbers.
a.some(isNaN) // => false; a has no non-numbers.
```

### reduce() and reduceRight()

combine the elements of an array, using the function you specify, to produce a single value

```jsx
let a = [1,2,3,4,5];
a.reduce((x,y) => x+y, 0) // => 15; the sum of the values
a.reduce((x,y) => x*y, 1) // => 120; the product of the values
a.reduce((x,y) => (x > y) ? x : y) // => 5; the largest of the values

// the second argument in the first two examples 
// is an initial value to pass to the function
```

reduceRight() works just like reduce(), except that it processes the array from highest index to lowest (right-to-left)

## Flattening arrays

### flat()

creates and returns a new array that contains the same elements as the array it is called on, except that any elements that are themselves arrays are “flattened” into the returned array

```jsx
[1, [2, 3]].flat() // => [1, 2, 3]
[1, [2, [3]]].flat() // => [1, 2, [3]]

// If you want to flatten more levels, pass a number to flat():

let a = [1, [2, [3, [4]]]];
a.flat(1) // => [1, 2, [3, [4]]]
a.flat(2) // => [1, 2, 3, [4]]
a.flat(3) // => [1, 2, 3, 4]
a.flat(4) // => [1, 2, 3, 4]
```

### flatMap()

Works just like the map() method except that the returned array is automatically flattened as if passed to flat(). Calling `a.flatMap(f)` is the same as (but more efficient than) `a.map(f).flat()`.

```jsx
let phrases = ["hello world", "the definitive guide"];
let words = phrases.flatMap(phrase => phrase.split(" "));
words // => ["hello", "world", "the", "definitive", "guide"];
```

## Adding Arrays

### concat()

creates and returns a new array that contains the elements of the original array on which concat() was invoked, followed by each of the arguments to concat().

If any of these arguments is itself an array, then it is the array elements that are concatenated, not the array itself. Note, however, that concat() does not recursively flatten arrays of arrays. concat() does not modify the array on which it is invoked:

```jsx
let a = [1,2,3];
a.concat(4, 5) // => [1,2,3,4,5]
a.concat([4,5],[6,7]) // => [1,2,3,4,5,6,7]; arrays are flattened
a.concat(4, [5,[6,7]]) // => [1,2,3,4,5,[6,7]]; but not nested arrays
a // => [1,2,3]; the original array is unmodified
```

Note that concat() makes a new copy of the array it is called on. In many cases, this is the right thing to do, but it is an expensive operation. If you find yourself writing code like a = a.concat(x),
then you should think about modifying your array in place with push() or splice() instead of creating a new one.

## Stacks and Queues with push(), pop(), shift(), and unshift()

```jsx
let stack = []; // stack == []
stack.push(1,2); // stack == [1,2];
stack.pop(); // stack == [1]; returns 2
stack.push(3); // stack == [1,3]
stack.pop(); // stack == [1]; returns 3
stack.push([4,5]); // stack == [1,[4,5]]
stack.pop() // stack == [1]; returns [4,5]
stack.pop(); // stack == []; returns 1
```

The push() method does not flatten an array you pass to it, but if you
want to push all of the elements from one array onto another array, you
can use the spread operator (§8.3.4) to flatten it explicitly:

```jsx
a.push(...values);
```

## Subarrays with slice(), splice(), fill(), and copyWithin()

### slice()

Returns a slice, or subarray, of the specified array. Has two arguments: (start, end). 

The returned array contains the element specified by the first argument and all subsequent elements up to, but not including, the element specified by the second argument. If only one argument: from start argument - until the end.

slice() does not modify the array on which it is invoked

```jsx
let a = [1,2,3,4,5];
a.slice(0,3); // Returns [1,2,3]
a.slice(3); // Returns [4,5]
a.slice(1,-1); // Returns [2,3,4]
a.slice(-3,-2); // Returns [3]

// a negative argument specifies an array element relative to the length of the array
```

### splice()

Method for inserting or removing elements from an array. Modifies the array on which it is invoked.

Can delete elements from an array, insert new elements, perform both operations at the same time.

First Argument: position at which to start the deletion/insertion;
Second Argument: the length ⇒ number of elements that should be spliced; if you don’t include the second argument it will remove until the end of the array;
Other Arguments: are the elements to be inserted into the array;

splice() returns an array of the deleted elements, or an empty array if no elements were deleted.

```jsx
let a = [1,2,3,4,5,6,7,8];
a.splice(4) // => [5,6,7,8]; a is now [1,2,3,4]
a.splice(1,2) // => [2,3]; a is now [1,4]
a.splice(1,1) // => [4]; a is now [1]
```

```jsx
let a = [1,2,3,4,5];
a.splice(2,0,"a","b") // => []; a is now [1,2,"a","b",3,4,5]
a.splice(2,2,[1,2],3) // => ["a","b"]; a is now [1,2,[1,2],3,3,4,5]

//in the third example 
//splice() inserts arrays themselves, 
//not the elements of those arrays (unlike concat())
```

### fill()

The fill() method sets the elements of an array, or a slice of an array, to a specified value.

```jsx
let a = new Array(5); // Start with no elements and length 5
a.fill(0) // => [0,0,0,0,0]; fill the array with zeros
a.fill(9, 1) // => [0,9,9,9,9]; fill with 9 starting at index 1
a.fill(8, 2, -1) // => [0,9,8,8,9]; fill with 8 at indexes 2, 3
```

### copyWithin()

```jsx
let a = [1,2,3,4,5];
a.copyWithin(1) // => [1,1,2,3,4]: copy array elements up one
a.copyWithin(2, 3, 5) // => [1,1,3,4,4]: copy last 2 elements to index 2
a.copyWithin(0, -2) // => [4,4,3,4,4]: negative offsets work, too
```

## Array Searching and Sorting Methods

### indexOf() and lastIndexOf()

searches the array from beginning to end,

searches the array from end to beginning

```jsx
let a = [0,1,2,1,0];
a.indexOf(1) // => 1: a[1] is 1
a.lastIndexOf(1) // => 3: a[3] is 1
a.indexOf(3) // => -1: no element has value 3
```

### includes()

includes() method takes a single argument and returns true if the array contains that value or false otherwise.

Difference between: `indexOf()` tests equality using the same algorithm that the === operator does, and that equality algorithm considers the not-a-number value to be different from every other value, including itself. includes() uses a slightly different version of equality that does consider NaN to be equal to itself. This means that indexOf() will not detect the NaN value in an array, but includes() will.

```jsx
let a = [1,true,3,NaN];
a.includes(true) // => true
a.includes(2) // => false
a.includes(NaN) // => true
a.indexOf(NaN) // => -1; indexOf can't find NaN
```

### sort()

Sorts the elements of an array basically.

```jsx
let a = ["banana", "cherry", "apple"];
a.sort(); // a == ["apple", "banana", "cherry"]

// if no argument is passed it sorts elements in alphabetical order
// if there are undefined elements - they go to the end of the array
```

```jsx
// to sort array elements into numerical order

let a = [33, 4, 1111, 222];
a.sort(); // a == [1111, 222, 33, 4]; alphabetical order
a.sort(function(a,b) { // Pass a comparator function
return a-b; // Returns < 0, 0, or > 0, depending on order
}); // a == [4, 33, 222, 1111]; numerical order
a.sort((a,b) => b-a); // a == [1111, 222, 33, 4]; reverse numerical order

// perform a caseinsensitive alphabetical sort

let a = ["ant", "Bug", "cat", "Dog"];
a.sort(); // a == ["Bug","Dog","ant","cat"]; casesensitive sort
a.sort(function(s,t) {
let a = s.toLowerCase();
let b = t.toLowerCase();
if (a < b) return -1;
if (a > b) return 1;
return 0;
}); // a == ["ant","Bug","cat","Dog"]; case-insensitive
sort
```

### reverse()

reverses the order of the elements of an array and returns the reversed array

```jsx
let a = [1,2,3];
a.reverse(); // a == [3,2,1]
```

## Array to String Conversions

If you want to save the contents of an array in textual form for later reuse, serialize the array with
`JSON.stringify()`

### join()

converts all the elements of an array to strings and concatenates them

```jsx
let a = [1, 2, 3];
a.join() // => "1,2,3"
a.join(" ") // => "1 2 3"
a.join("") // => "123"
let b = new Array(10); // An array of length 10 with no elements
b.join("-") // => "---------": a string of 9 hyphens
```

It is the inverse of the `String.split()` method, which creates an array by breaking a string into pieces.

### toString()

`toString()` method. For an array, this method works just like the `join()` method with no arguments:

```jsx
[1,2,3].toString() // => "1,2,3"
["a", "b", "c"].toString() // => "a,b,c"
[1, [2,"c"]].toString() // => "1,2,c"
```
