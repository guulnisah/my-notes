# JavaScript in Web Browsers

# Web Programming Basics

JavaScript code can appear inline within an HTML file between
<script> and </script> tags. But it is more common to instead use the src attribute of the <script> tag to specify the URL (an absolute URL or a URL
relative to the URL of the HTML file being displayed) of a file containing JavaScript code.

Using `src` attribute is better, because:

- simple HTML
- if multiple pages use the same JS file, you maintain only one copy
- JavaScript program or web page from one web server can employ code exported by other web servers

### Modules

If you are using modules (and have not used a code-bundling tool to combine all your modules into a single nonmodular file of JavaScript), then you must
load the top-level module of your program with a `<script>` tag that
has a `type="module"` attribute. 

### When Scripts Run: async and defer

The `<script>` tag can have `defer` and `async` attributes, which cause scripts to be executed differently.

The `defer`attribute causes the browser to defer execution of the script until after the document has been fully loaded and parsed and is ready to be manipulated. 

The `async` attribute causes the browser to run the script as soon as possible but does not block document parsing while the script is being downloaded. 

If a `<script>` tag has both attributes, the `async` attribute takes precedence.

Deferred scripts run in the order in which they appear in the document. Async scripts run as they load, which means that they may execute out of order.

Scripts with the `type="module"` attribute by default have a defer attribute. You
can override this default with the async attribute, which will cause the code to be executed as soon as the module and all of its dependencies have loaded.

An alternative: just put your script at the end of your file. 

## The DOM

The DOM is central to the client-side JS.

The DOM API mirrors the tree structure of an HTML document. For each HTML tag in the document, there is a corresponding JavaScript Element object, and for each run of text in the document, there is a corresponding Text object. The Element and Text classes, as well as the Document class itself, are all subclasses of the more general Node class, and Node objects are organized into a tree structure that JavaScript can query and traverse using the DOM API. The DOM representation of this document is the tree pictured in Figure 15-1.

The node directly above a node is the parent of that node. The nodes one level directly below another node are the children of that node. Nodes at the same level, and with the same parent, are siblings. The set of nodes any number of levels below another node are the descendants of that node. And the parent, grandparent, and all other nodes above a node are the ancestors of that node.

There is a JavaScript class corresponding to each HTML tag type, and each occurrence of the tag in a document is represented by an instance of the class. The <body> tag, for example, is represented by an instance of HTMLBodyElement, and a <table> tag is represented by an instance of HTMLTableElement. The JavaScript element objects have properties that correspond to the HTML attributes of the tags. For example, instances of HTMLImageElement, which represent <img> tags, have a src property that corresponds to the src attribute of the tag. The initial value of the src property is the attribute value that appears in the HTML tag, and setting this property with JavaScript changes the value of the HTML attribute (and causes the browser to load and display a new image). Most of the JavaScript element classes just mirror the attributes of an HTML tag, but some define additional methods. The HTMLAudioElement and HTMLVideoElement classes, for example, define methods like play() and pause() for controlling playback of audio and video files.

# Most important DOM methods

Notes from [this](https://www.youtube.com/watch?v=y17RuWkWdn8&t=142s&ab_channel=WebDevSimplified) video 

## Adding elements

```jsx
const body = document.body
body.append("Hello") // with just append you can append strings
body.appendChild("Hello") // ERROR, with appendChild can only append Elements

const div = document.createElement('div') // we have just created it 
body.append(div) // now we have appended it to the body

```

## Modifying text

****What is the Difference Between textContent and innerText?****

1. `innerText` was non-standard, `textContent` was standardized earlier.
2. `innerText` returns the **visible** text contained in a node, while `textContent` returns the **full** text. For example, on the following HTML `<span>Hello <span style="display: none;">World</span></span>`, `innerText` will return 'Hello', while `textContent` will return 'Hello World'. For a more complete list of differences, see the table at [link](http://perfectionkills.com/the-poor-misunderstood-innerText/) (further reading at ['innerText' works in IE, but not in Firefox](https://stackoverflow.com/questions/1359469/innertext-works-in-ie-but-not-in-firefox/1359822#1359822)).
3. As a result, `innerText` is much more performance-heavy: it requires layout information to return the result.
4. `[innerText](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/innerText)` is defined only for `HTMLElement` objects, while `[textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)` is defined for all `Node` objects.

## Modifying Element HTML

```jsx
div.innerHTML = '<strong>Hello World</strong>'
```

There are security issues connected to this method. [Video](https://www.youtube.com/watch?v=ns1LX6mEvyM&ab_channel=WebDevSimplified)

## Removing elements

```jsx
div.remove()
body.append(div) // to get back that element
```

## Modifying attributes

```jsx
// two ways to get an attribute of an element
console.log(div.getAttribute('id')
console.log(div.id())

div.setAttribute('id', 'container')
div.removeAttribute('id', 'container')
// or you can update it another way 
div.id = 'container'
```

## Modifying data attributes

```jsx

```

## Modifying classes

```jsx
div.classList.add()
div.classList.remove()
div.classList.toggle()
```

## Modifying Style

```jsx
div.style.color = "red"

```

Browser environemtn

WINDOW 3 things: DOM BOM Javascript

BOM ⇒ navigator screen location history frames location XMLHttpRequest

possibilities with DOM

change, add, remove create etc.

each block is called a node

it’s all inside window object 

```jsx
const myEmojis = ["👨‍💻", "⛷", "🍲"]
const emojiContainer = document.getElementById("emoji-container")
const emojiInput = document.getElementById("emoji-input")
const pushBtn = document.getElementById("push-btn")

// function to display the emojis from the array 
function renderEmojis() {
emojiContainer.innerHTML = "" // get rid of the old array, otherwise you'll have two array - original and one with the new element - and so on 
    for (let i = 0; i < myEmojis.length; i++) {
        const emoji = document.createElement('span')
        emoji.textContent = myEmojis[i]
        emojiContainer.append(emoji)
    }
}

renderEmojis() // you render Emojis 

pushBtn.addEventListener("click", function(){
    if (emojiInput.value) { // if the input has smth inside 
        myEmojis.push(emojiInput.value) // push that value to the original array 
        emojiInput.value = "" // clear the input when the button is clicked
        renderEmojis() // and now you render the new array 
    }
})
```

\

DOM manipulation always comes at a cost! It’s heavy. The least you can do - the better.

```jsx
if (player1Turn) {
         player1Turn = false
     } else {
         player1Turn = true
     }

// is the same as 

player1Turn = !player1Turn
```

## The Global Object

In web browsers, the global object does double duty: in addition to defining built-in types and functions, it also represents the current web browser window.

You can simply type window to refer to the global object in your client-side code.

## Scripts Share a Namespace

To summarize: in modules, top-level declarations are scoped to the module and can be explicitly exported. In nonmodule scripts, however, top-level declarations are scoped to the containing document, and the declarations are shared by all scripts in the document. Older var and function declarations are shared via properties of the global object. Newer const, let, and class declarations are also shared and have the same document scope, but they do not exist as properties of any object that JavaScript code has access to.

## Execution of JS Programs

## Program Input and Output

## Program Errors

## The Web Security Model

# Events

*event type / event name:* mousemove, click, load, etc.

*event target*: the object on which the vent occurred

*event handler / event listener*: the function that handles or responds to an event

*event object*: has the info about a particular event, contains details about it

*event propagation*:  the process by which the browser decides which objects to
trigger event handlers on

*event capturing*: a form of event propagation; 

*event bubbling*: 

## Event Categories

Too many… But we can group them!

### Device-dependent input events

Are tied to a specific input device(mouse, keyboard). 

They include event types such as “mousedown,” “mousemove,” “mouseup,” “touchstart,” “touchmove,” “touchend,” “keydown,” and “keyup.” 

### Device-independent input events

Not tied to devices. 

The “click” event, for example, can be activated via a mouse click, by keyboard or (on touchsensitive devices) with a tap. 

The “input” event is a device-independent alternative to the “keydown” event and supports keyboard input as well as alternatives such as cut-and-paste and input methods used for ideographic scripts. 

The “pointerdown,” “pointermove,” and “pointerup”: alternatives to mouse and touch events. They work for mouse-type pointers, for touch screens, and for pen- or stylus-style input as well. 

### User interface events

UI events are higher-level events

The “focus” event (when a text input field gains keyboard focus).

The “change” event (when the user changes the value displayed by a form element).

The “submit” event (when the user clicks a Submit button in a form). 

### State-change events

Some events are triggered by network or browser activity.

They indicate some kind of life-cycle or state-related change. 

The “load” and “DOMContentLoaded” events are probably the most commonly used of these events (see “Client-side JavaScript timeline”). 

Browsers fire “online” and “offline” events on the Window object when network connectivity changes. 

The browser’s history management mechanism fires the “popstate” event in response to the browser’s Back button. 

### API-specific events

A number of web APIs defined by HTML and related specifications include their own event types. 

The HTML <video> and <audio> elements define a long list of associated event types such as “waiting,” “playing,” “seeking,” “volumechange,” and so on, and you can use them to customize media playback. 

Generally speaking, web platform APIs that are asynchronous and were developed before Promises were added to JavaScript are eventbased and define API-specific events. 

The IndexedDB API, for example, fires “success” and “error” events when database requests succeed or fail. 

Although the new fetch() API for making HTTP requests is Promise-based, the XMLHttpRequest API that it replaces defines a number of APIspecific event types.

## Registering Event Handlers

### Event Handler Properties

Not a really good way to register an event.

```jsx
// Set the onload property of the Window object to a function.
// The function is the event handler: it is invoked when the document loads.
window.onload = function() {
// Look up a <form> element
let form = document.querySelector("form#shipping");
// Register an event handler function on the form that will be invoked
// before the form is submitted. Assume isFormValid() is defined elsewhere.
form.onsubmit = function(event) { // When the user submits the form
if (!isFormValid(this)) { // check whether form inputs are valid
event.preventDefault(); // and if not, prevent form submission.
}
};
};
```

### Event Handler Attributes

The event handler properties of document elements can also be defined directly in the HTML file as attributes on the corresponding HTML tag.

```jsx
<button onclick="console.log('Thank you');">Please Click</button>
```

JavaScript code in HTML attributes is never strict.

### addEventListener()

Three arguments: event type, function, optional (look below for more info)

```jsx
<button id="mybutton">Click me</button>
<script>
let b = document.querySelector("#mybutton");
b.onclick = function() { console.log("Thanks for clicking me!"); };
b.addEventListener("click", () => { console.log("Thanks again!"); });
</script>
```

Paired with `removeEventListener()`.

```jsx
document.removeEventListener("mousemove", handleMouseMove);
document.removeEventListener("mouseup", handleMouseUp);
```

### Third Argument of addEventListener()

It is a boolean value or object.

If you pass true: your handler function is registered as a capturing event handler and is invoked at a different phase of event dispatch. If you passed it in `addEventListener()` you must pass it in `removeEventListener()` also.

If it’s an object:

```jsx
document.addEventListener("click", handleClick, {
capture: true,
once: true,
passive: true
});
```

If `capture` is `true`, then the event handler will be registered as a capturing handler. If `false` or is omitted, then the handler will be non-capturing. 

If `once` property is `true`, then the event listener will be automatically removed after it is triggered once. If `false` or is omitted, then the handler is never automatically removed. 

If `passive` is `true`, it indicates that the event handler will never call `preventDefault()` to cancel the default action. Very important for touch events on mobile devices.

## Event Handler Invocation

### Event handler argument

Event handlers have a single argument: Event Object. Its properties:

*type:* The type of the event that occurred. 

*target:* The object on which the event occurred. 

*currentTarget:* For events that propagate, this property is the object on which the current event handler was registered. 

*timeStamp:* A timestamp (in milliseconds) that represents when the event occurred but that does not represent an absolute time. You can determine the elapsed time between two events by subtracting the timestamp of the first event from the timestamp of the second. 

*isTrusted*:  This property will be true if the event was dispatched by the web browser itself and false if the event was dispatched by JavaScript code.

Some events have specific properties. For example: mouse and pointer event have `clientX` and `clientY`, to specify where the event occurred.

### Event handler context

That is, within the body of an event handler, the this keyword refers to the object on which the event handler was registered. Handlers are invoked with the target as their this value, even when registered using `addEventListener()`. This does not work for handlers defined as arrow functions, however: arrow functions always have the same this value as the scope in which they are defined.

### Handler return value\\

Event handlers should not return anything.

### Invocation order

When an event of that type occurs, the browser invokes all of the handlers in the order in which they were registered.

## Event Propagation

## Event Cancellation

## Dispatching Custom Events

# Scripting Documents

## Selecting Document Elements

## Document Structure and Traversal

## Attributes

## Element Content

# Scripting CSS

## CSS Classes

The simplest way to use JavaScript to affect the styling of document content is to add and remove CSS class names from the class attribute of HTML tags. This is easy to do with the `classList` property of Element objects

```jsx
// Assume that this "tooltip" element has class="hidden" in
the HTML file.
// We can make it visible like this:
document.querySelector("#tooltip").classList.remove("hidden");
// And we can hide it again like this:
document.querySelector("#tooltip").classList.add("hidden");
```

## Inline Styles

When adding and removing classes is not enough, use inline CSS.

Example: we want to add style to a tooltip. `style` property is not a string; it’s a `CSSStyleDeclaration` object. 

```jsx
function displayAt(tooltip, x, y) {
tooltip.style.display = "block";
tooltip.style.position = "absolute";
tooltip.style.left = `${x}px`;
tooltip.style.top = `${y}px`;
}
```

CSS

```jsx
display: block; 
font-family: sans-serif; 
background-color:
#ffffff;
```

Javascript

```jsx
e.style.display = "block";
e.style.fontFamily = "sans-serif";
e.style.backgroundColor = "#ffffff";
```

Be careful with the units:

```jsx
e.style.marginLeft = 300; // Incorrect: this is a number, not a string
e.style.marginLeft = "300"; // Incorrect: the units are missing
e.style.marginLeft = "300px"; // Correct
e.style.left = `${x0 + left_border + left_padding}px`; // If you want some computation
```

## Computed Styles

If you want to query the styles of an element, you probably want the computed style. Computed styles are ALSO represented with a `CSSStyleDeclaration` object, BUT they are read-only.

To get the computed style:

```jsx
let title = document.querySelector("#section1title");
let styles = window.getComputedStyle(title);
let beforeStyles = window.getComputedStyle(title,"::before");
```

The return value of `getComputedStyle()` is a `CSSStyleDeclaration` object that represents all the styles that apply to the specified element:

- Computed style properties are read-only.
- Computed style properties are absolute: relative units like percentages and points are converted to absolute values, colors will be returned in “rgb()” or “rgba()” format.
- Shortcut properties are not computed. Don’t query the margin property, for example, but use marginLeft, marginTop, and so on.

To determine size and position of an element use othe methods (discussed later)

## Scripting Stylesheets

JavaScript can also manipulate stylesheets themselves.

The Element objects for both <style> and <link> tags have a disabled property that you can use to disable the entire stylesheet. You might use it with code like this:

```jsx
// This function switches between the "light" and "dark" themes
function toggleTheme() {
let lightTheme = document.querySelector("#light-theme");
let darkTheme = document.querySelector("#dark-theme");
if (darkTheme.disabled) { // Currently light, switch to dark
lightTheme.disabled = true;
darkTheme.disabled = false;
} else { // Currently dark, switch to light
lightTheme.disabled = false;
darkTheme.disabled = true;
}
}
```

Another examples:

```jsx
function setTheme(name) {
// Create a new <link rel="stylesheet"> element to load the named stylesheet
let link = document.createElement("link");
link.id = "theme";
link.rel = "stylesheet";
link.href = `themes/${name}.css`;
// Look for an existing link with id "theme"
let currentTheme = document.querySelector("#theme");
if (currentTheme) {
// If there is an existing theme, replace it with the new one.
currentTheme.replaceWith(link);
} else {
// Otherwise, just insert the link to the theme stylesheet.
document.head.append(link);
}
}
```

Or this fun trick:

```jsx
document.head.insertAdjacentHTML("beforeend",
"<style>body{transform:rotate(180deg)}</style>"
);
```

Read more: “CSSStyleSheet”, “CSS Object Model.”

## CSS Animations and Events \\\

# Location, Navigation, History

`location` refers to Location object, which represents the current URL of the document. 

```jsx
location.href         // https://example.com:81/path?argument=value#hash
location.protocol     // https
location.hostname     // example.com
location.port         // 81
location.host         // example.com:81
location.pathname     // /path
location.search       // ?argument=value (see URLSearchParams to parse)
location.hash         // #hash
location.origin       // https://example.com"
```

URL objects have a `searchParams` property that is a parsed representation of the search property. The Location object does not have a `searchParams` property, but if you want to parse `window.location.search`, you can simply create a URL object from the Location object and then use the URL’s `searchParams`:

```jsx
let url = new URL(window.location);
let query = url.searchParams.get("q");
let numResults = parseInt(url.searchParams.get("n") || "10");
```

In addition to the Location object that you can refer to as `window.location` or `document.location`, and the `URL()` constructor that we used earlier, browsers also define a `document.URL` property. Surprisingly, the value of this property is not a URL object, but just a string. The string holds the URL of the current document.

## Loading New Documents

If you assign a string to `window.location` or to `document.location`, that string is interpreted as a URL and the browser loads it, replacing the current document with a new one:

```jsx
window.location = "http://www.oreilly.com"; 
```

You can also assign relative URLs to location. They are resolved relative to the current URL:

```jsx
document.location = "page2.html"; // Load the next page
```

Bare fragment identifier: 
- does not cause the browser to load a new document 
- causes the browser to scroll so that the document element with id or name that matches the fragment is visible at the top of the browser window

As a special case, the fragment identifier #top makes the browser jump to the start of the document (assuming no element has an id="top" attribute):

```jsx
location = "#top"; // Jump to the top of the document
```

The individual properties of the Location object are writable, and setting them changes the location URL and also causes the browser to load a new document (or, in the case of the hash property, to navigate within the current document):

```jsx
document.location.path = "pages/3.html"; // Load a new page
document.location.hash = "TOC"; // Scroll to the table of contents
location.search = "?page=" + (page+1); // Reload with new query string
```

### assign() of Location object

You can also load a new page by passing a new string to the assign() method of the Location object. This is the same as assigning the string to the location property, however, so it’s not particularly interesting.

### replace() of Location object

The replace() method of the Location object, on the other hand, is quite useful. When you pass a string to replace(), it is interpreted as a URL and causes the browser to load a new page, just as assign() does. The difference is that replace() replaces the current document in the browser’s history. If a script in document A sets the location property or calls assign() to load document B and then the user clicks the Back button, the browser will go back to document A. If you use replace() instead, then document A is erased from the browser’s history, and when the user clicks the Back button, the browser returns to whatever document was displayed before document A. 

When a script unconditionally loads a new document, the replace() method is a better choice than assign(). Otherwise, the Back button would take the browser back to the original document, and the same script would again load the new document. Suppose you have a JavaScript-enhanced version of your page and a static version that does not use JavaScript. If you determine that the user’s browser does not support the web platform APIs that you want to use, you could use location.replace() to load the static version:

```jsx
// If the browser does not support the JavaScript APIs we need,
// redirect to a static page that does not use JavaScript.
if (!isBrowserSupported())
location.replace("staticpage.html");
```

Notice that the URL passed to replace() is a relative one.

### reload()

Simply makes the browser reload the document.

## Browsing History

`history`property of the Window object  models the browsing history of a window.

The length property of the History object specifies the number of elements in the browsing history list, but for security reasons, scripts are not allowed to access the stored URLs. (If they could, any scripts could snoop through your browsing history.)

The History object has `back()` and `forward()` methods that behave like the browser’s Back and Forward buttons do: they make the browser go backward or forward one step in its browsing history.

`go()`takes an integer argument and can skip any number of pages forward (for positive arguments) or backward (for negative arguments) in the history list:

```jsx
history.go(-2); // Go back 2, like clicking the Back button twice
history.go(0); // Another way to reload the current page
```

# Networking

# LocalStorage

`localStorage`
 is similar to `[sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)`

- `localStorage` data has no expiration time
- `sessionStorage` data gets cleared when the page session ends — that is, when the page is closed.
- `localStorage` data for a document loaded in a "private browsing" or "incognito" session is cleared when the last "private" tab is closed

```jsx
localStorage.getItem("key")
localStorage.getItem("key", value)
```

Value has to be a string. So if you have a more complex value like an array or object, you need to use:

```jsx
JSON.stringify(value)
// to make it a string
JSON.parse(stringifiedValue)
// to reverse it 

```

```jsx
/*
if you want the whole video it's here https://scrimba.com/learn/learnreact/lazy-state-initialization-cod784faab248ffe738e900a2
there we learnt to save the notes info into localstorage 

*/

const [notes, setNotes] = React.useState(
        () => JSON.parse(localStorage.getItem("notes")) || []
//here we wrote it as a function becuase localStorage is a heavy thing to do 
//so by making a function the browser won't render the whole app component every time changes are made
    )

React.useEffect(() => {
        localStorage.setItem("notes", JSON.stringify(notes))
//here we put the info into localstorage every time the arrya changes
    }, [notes])
```