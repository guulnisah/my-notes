# Classes

Class declarations are not hoisted.

```jsx
// what we did before 
function Circle(radius) {
	this.radius = radius;
	this.draw = function() {
		console.log('draw');
	}
}

// what we should do now
class Circle {
	constructor(radius) {
		this.radius = radius;
	}

	draw() {
		console.log('draw')
	}
}
```

Another example from Scrimba

```jsx
class Module {
    constructor(){
        this.courseName = "Learn JS"
        this.studentsEnrolled = 5600
        this.studentsCompleted = 5100      
    }  
}

const learnJs = new Module
console.log(learnJs)
console.log(learnJs.studentsEnrolled)

// ›Module {courseName: "Learn JS", studentsEnrolled: 5600, studentsCompleted: 5100}
// ›5600
```

Challenge from Scrimba 

```jsx
const adData = {
    facebook: {
        site: 'Facebook',
        adViews: 23477,
        clicks: 210,
        conversions: 5,
    },
    twitter: {
        site: 'Twitter',
        adViews: 13904,
        clicks: 192,
        conversions: 9,
    },
    instagram: {
        site: 'Instagram',
        adViews: 3256,
        clicks: 200,
        conversions: 2,
    }
}

class AdvertisingChannel {
// setting up a constructor method that takes data as a parameter
    constructor(data){ 
// now you set up name, adViews, adClicks and conversionproperties on "this"
        Object.assign(this, data) 
// now you set up a property to hold the percentage of clicks 
// that resulted in someone subscribing
        this.conversionRate = (this.conversions / this.clicks * 100).toFixed(1) 
    }
// now you create a method which returns HTML
    getAdvertisingChannelHtml(){
// you can destructure the object for your comfort 
// so that you don't write this. all the time 
        const {site, adViews, clicks, conversions, conversionRate} = this
        return `
            <div class="site-name"> ${site} </div>
            <div>Views: ${adViews} </div>
            <div>Clicks: ${clicks} </div>
            <div>Conversions: ${conversions} </div>
            <div>Conv. Rate: <span class="highlight"> ${conversionRate} %</span></div> 
        `
    }
}
// now you create an instance of AdvertisingChannel for each channel
const facebook = new AdvertisingChannel(adData.facebook)
const twitter = new AdvertisingChannel(adData.twitter)
const instagram = new AdvertisingChannel(adData.instagram)

// now you render their html to the page using the method from the class 
document.getElementById('fb').innerHTML = facebook.getAdvertisingChannelHtml()
document.getElementById('twit').innerHTML = twitter.getAdvertisingChannelHtml()
document.getElementById('insta').innerHTML = instagram.getAdvertisingChannelHtml()
```

## Method Chaining

```jsx
class Human {
	constructor(name, age) {
		this.name = name;
		this.age = age
	}
	speak() {
		console.log("Hi")}
	eat() {
		console.log("Yummy")}
}

const bill = new Human('bill', 25)
bill.speak().eat() // this is method chaining 
// this will give you an error because JS basically sees undefined.eat()
// cause your function doesn't return anything

// to escape this error you can add 
// return this 
// to the function 
// then you won't get the error 
```

# Subclasses

## extends and super keywords

```jsx
class Pet {
	constructor(name, age) {
		console.log('IN PET CONSTRUCTOR!');
		this.name = name;
		this.age = age;
	}
	eat() {
		return `${this.name} is eating!`;
	}
}

class Cat extends Pet {
	constructor(name, age, livesLeft = 9) {
		console.log('IN CAT CONSTRUCTOR!');
		super(name, age);
		this.livesLeft = livesLeft;
	}
	meow() {
		return 'MEOWWWW!!';
	}
}

class Dog extends Pet {
	bark() {
		return 'WOOOF!!';
	}
	eat() {
		return `${this.name} scarfs his food!`;
	}
}
```

# Classes and Prototypes / Constructors

Compare these two examples:

```jsx
// Example 9-1. A simple JavaScript class
// This is a factory function that returns a new range object.
function range(from, to) {
// Use Object.create() to create an object that inherits from the
// prototype object defined below. The prototype object is stored as
// a property of this function, and defines the shared methods (behavior)
// for all range objects.
let r = Object.create(range.methods);
// Store the start and end points (state) of this new range object.
// These are noninherited properties that are unique to this object.
r.from = from;
r.to = to;
// Finally return the new object
return r;
}
// This prototype object defines methods inherited by all range objects.
range.methods = {
// Return true if x is in the range, false otherwise
// This method works for textual and Date ranges as well as numeric.
includes(x) { return this.from <= x && x <= this.to; },
// A generator function that makes instances of the class iterable.
// Note that it only works for numeric ranges.
*[Symbol.iterator]() {
for(let x = Math.ceil(this.from); x <= this.to; x++)
yield x;
},
// Return a string representation of the range
toString() { return "(" + this.from + "..." + this.to +
")"; }
};
// Here are example uses of a range object.
let r = range(1,3); // Create a range object
r.includes(2) // => true: 2 is in the range
r.toString() // => "(1...3)"
[...r] // => [1, 2, 3]; convert to an array via iterator
```

```jsx
//Example 9-2. A Range class using a constructor

// This is a constructor function that initializes new Range objects.
// Note that it does not create or return the object. It just initializes this.
function Range(from, to) {
// Store the start and end points (state) of this new range object.
// These are noninherited properties that are unique to this object.
this.from = from;
this.to = to;
}
// All Range objects inherit from this object.
// Note that the property name must be "prototype" for this to work.
Range.prototype = {
// Return true if x is in the range, false otherwise
// This method works for textual and Date ranges as well as numeric.
includes: function(x) { return this.from <= x && x <=
this.to; },
// A generator function that makes instances of the class iterable.
// Note that it only works for numeric ranges.
[Symbol.iterator]: function*() {
for(let x = Math.ceil(this.from); x <= this.to; x++)
yield x;
},
// Return a string representation of the range
toString: function() { return "(" + this.from + "..." +
this.to + ")"; }
};
// Here are example uses of this new Range class
let r = new Range(1,3); // Create a Range object; note the use of new
r.includes(2) // => true: 2 is in the range
r.toString() // => "(1...3)"
[...r] // => [1, 2, 3]; convert to an array via iterator
```

# Classes with the class Keyword

Example from above with class Keyword

```jsx
Example 9-3. The Range class rewritten using class
class Range {
constructor(from, to) {
// Store the start and end points (state) of this new range object.
// These are noninherited properties that are unique to this object.
this.from = from;
this.to = to;
}
// Return true if x is in the range, false otherwise
// This method works for textual and Date ranges as well as numeric.
includes(x) { return this.from <= x && x <= this.to; }
// A generator function that makes instances of the class iterable.
// Note that it only works for numeric ranges.
*[Symbol.iterator]() {
for(let x = Math.ceil(this.from); x <= this.to; x++)
yield x;
}
// Return a string representation of the range
toString() { return `(${this.from}...${this.to})`; }
}
// Here are example uses of this new Range class
let r = new Range(1,3); // Create a Range object
r.includes(2) // => true: 2 is in the range
r.toString() // => "(1...3)"
[...r] // => [1, 2, 3]; convert to an array via iterator
```

The introduction of the class keyword to the language does not alter the fundamental nature of JavaScript’s prototype-based classes. The new class syntax is clean and convenient but is best thought of as “syntactic sugar”.

<aside>
⚠️ The class is declared with the class keyword, which is followed by the name of class and a class body in curly braces.
No commas are used to separate the methods from each other.

If your class does not need to do any initialization, you can omit the constructor keyword and its body, and an empty constructor function will be implicitly created for you.

</aside>

If you want to define a class that subclasses—or inherits from another class, you can use the extends keyword with the class keyword:

```jsx
// A Span is like a Range, but instead of initializing it
with
// a start and an end, we initialize it with a start and a
length
class Span extends Range {
	constructor(start, length) {
		if (length >= 0) {
			super(start, start + length);
		} else {
			super(start + length, start);
		}
	}
}
```

There are class expressions as well

```jsx
let Square = class { constructor(x) { this.area = x * x; } };
new Square(3).area // => 9
```

Class definition expressions are not something that you are likely to use much unless you find yourself writing a function that takes a class as its argument and returns a subclass.

<aside>
⚠️ Class declarations are not hoisted!

</aside>

```jsx
class Circle {
    constructor(radius) {
        this.radius = radius; 
    }

    // These methods will be added to the prototype. 
    draw() {
    }

    // This will be available on the Circle class (Circle.parse())
    static parse(str) {
    }
}

// Using symbols to implement private properties and methods
const _size = Symbol();
const _draw = Symbol();

class Square {
    constructor(size) {
        // "Kind of" private property 
        this[_size] = size; 
    }

    // "Kind of" private method 
    [_draw]() {
    }

    // By "kind of" I mean: these properties and methods are essentally
    // part of the object and are accessible from the outside. But accessing
    // them is hard and awkward. 
}

// using WeakMaps to implement private properties and methods
const _width = new WeakMap();

class Rectangle {
    constructor(width) {
        _width.set(this, width);
    }

    draw() {
        console.log('Rectangle with width' + _width.get(this));
    }
}

// WeakMaps give us better protection than symbols. There is no way 
// to access private members implemented using WeakMaps from the 
// outside of an object. 

// Inheritance 
class Triangle extends Shape {
    constructor(color) {
        // To call the base constructor 
        super(color);
    }

    draw() {
        // Call the base method 
        super.draw();

        // Do some other stuff here
    }
}
```

## Static Methods

You can define a static method within a class body by prefixing the method declaration with the `static`keyword. Static methods are defined as properties of the constructor function rather than properties of the prototype object.

```jsx
class Car {
  constructor(name) {
    this.name = name;
  }
  static hello() {
    return "Hello!!";
  }
}

let myCar = new Car("Ford");

// You can call 'hello()' on the Car Class:
document.getElementById("demo").innerHTML = Car.hello();

// But NOT on a Car Object:
// document.getElementById("demo").innerHTML = myCar.hello();
// this will raise an error.
```

## Getters, Setters, and other Method Forms

In class bodies, you don’t put a comma after the getter or setter.

# Adding Methods to Existing Classes