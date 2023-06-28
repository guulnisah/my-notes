# Working with APIs

# Intro to API and BoredBot

-What does API stand for?
Application Programming Interface

-How would you describe an API in your own words?
A tool that allows your code to talk with (use the goodness from) someone else's code. (Web APIs, 3rd-party package, etc.)

-Can you think of an example of an API you've used?

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled.png)

---

-What are some examples of "clients" you've used today?

-How would you explain what a "server" is to a 5 year-old?
A computer that send my own computer things when I ask it to.

-In what way do clients and servers interact with each other?
Client sends a request to a server, and the server sends back a response.

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%201.png)

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%202.png)

---

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%203.png)

---

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%204.png)

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%205.png)

What are 3 things your computer (client) might request from a server?

- JSON array of suggested videos
- Video stream
- HTML page

## Setting up a Fetch request

A basic fetch request is really simple to set up. Have a look at the following code:

```jsx
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```

### Asynchronous JavaScript

```jsx
console.log("The first console log")

fetch("https://dog.ceo/api/breeds/image/random")
    .then(response => response.json())
    .then(data => console.log(data))

console.log("The second console log")

for (let i = 0; i < 100; i++) {
    console.log("I'm inside the for loop")
}

// even though the fetch is second it's the last one 
// the output...
// ›The first console log
// ›The second console log
// › 100 "I'm inside the for loop"
// ›{message: "https://images.dog.ceo/breeds/retriever-curly/n02099429_1342.jpg", status: "success"}
```

### Using Fetch API and putting an image from an API into HTML

```html
<html>
    <head>
        <link rel="stylesheet" href="index.css">
    </head>
    <body>
        <div id="image-container"></div>
        <script src="index.js"></script>
    </body>
</html>
```

```jsx
fetch("https://dog.ceo/api/breeds/image/random")
    .then(response => response.json())
    .then(data => {
        console.log(data)
        document.getElementById("image-container").innerHTML = 
              `<img src="${data.message}" />`
    })
```

### Using Fetch API and putting text from the API into HTML

```html
<html>
    <head>
        <link rel="stylesheet" href="index.css">
    </head>
    <body>
        <h1 id="activity-name"></h1>
        <script src="index.js"></script>
    </body>
</html>
```

```jsx
fetch("https://apis.scrimba.com/bored/api/activity")
    .then(response => response.json())
    .then(data => {
        console.log(data)
        document.getElementById("activity-name").textContent = data.activity
    })
```

### Using Fetch API inside a function for a button to display a different text from the API

```html
<html>
    <head>
        <link rel="preconnect" href="https://fonts.gstatic.com">
        <link href="https://fonts.googleapis.com/css2?family=Oxygen:wght@300&display=swap" rel="stylesheet">
        <link rel="stylesheet" href="index.css">
    </head>
    <body>
        <h1>🤖 BoredBot 🤖</h1>
        <h4 id="activity">Find something to do</h4>
        <button id="get-activity"></button>
        <script src="index.js"></script>
    </body>
</html>
```

```jsx
document.getElementById("get-activity").addEventListener("click", function() {
  fetch("https://apis.scrimba.com/bored/api/activity")
    .then(response => response.json())
    .then(data => {
      document.getElementById("activity").textContent = data.activity
    })
})
```

# URLs, REST & BlogSpace

## Components of a request

Components of a Request 
1. Path (URL)
2. Method (GET, POST, PUT, DELETE, PATCH, OPTIONS, ETC)
3. Body 
4. Header

Path(URL)
Address where your desired resource ‘lives’. BaseURL and Endpoint. 

Method
GET - getting data
POST - adding new data 
PUT - updating existing data 
DELETE - removing data 

Body 
The data we want to send to the server
Only makes sense with POST and PUT requests
Needs to be turned into JSON first 

Headers
Extra-meta information about the outgoing request
Auth, body info, client info, etc.

 - How would you describe what a protocol is to a complete newbie?
An agreed-upon, standard way of doing something
- Which part of this URL describes the protocol?
https:// 
- What's the difference between a Base URL and an Endpoint?
Base URL is the part of the URL that won't change, no matter which resource we want to get from the API
Endpoint specifies exactly which resource we want to get from the API

## Blogspace

### API has a lot of posts but we only need the first 5

```jsx
fetch("https://apis.scrimba.com/jsonplaceholder/posts")
    .then(res => res.json())
    .then(data => {
        const postsArr = data.slice(0, 5)
        console.log(postsArr)
    })
```

### To display the posts on the page

```jsx
fetch("https://apis.scrimba.com/jsonplaceholder/posts")
    .then(res => res.json())
    .then(data => {
        const postsArr = data.slice(0, 5)
        let html = ""
        for (let post of postsArr) {
            html += `
                <h3>${post.title}</h3>
                <p>${post.body}</p>
                <hr />
            `
        }
        document.getElementById("blog-list").innerHTML = html
    })
```

### Form submit event listener

```jsx
document.getElementById("new-post").addEventListener("submit", function(e) {
    e.preventDefault()
    const postTitle = document.getElementById("post-title").value
    const postBody = document.getElementById("post-body").value
    const data = {
        title: postTitle,
        body: postBody
    }
    console.log(data)
})
```

### Send New Post to the server

```jsx
document.getElementById("new-post").addEventListener("submit", function(e) {
    e.preventDefault()
    const postTitle = document.getElementById("post-title").value
    const postBody = document.getElementById("post-body").value
    const data = {
        title: postTitle,
        body: postBody
    }
    console.log(data)
})
const options = {
        method: "POST",
        body: JSON.stringify(data),
        headers: {
            "Content-Type": "application/json"
        }
    }
     
fetch("https://apis.scrimba.com/jsonplaceholder/posts", options)
        .then(res => res.json())
        .then(data => console.log(data))
```

### Add new post to list of posts

```jsx
fetch("https://apis.scrimba.com/jsonplaceholder/posts")
    .then(res => res.json())
    .then(data => {
        const postsArr = data.slice(0, 5)
        let html = ""
        for (let post of postsArr) {
            html += `
                <h3 class="blah">${post.title}</h3>
                <p>${post.body}</p>
                <hr />
            `
        }
        document.getElementById("blog-list").innerHTML = html
    })

document.getElementById("new-post").addEventListener("submit", function(e) {
    e.preventDefault()
    const postTitle = document.getElementById("post-title").value
    const postBody = document.getElementById("post-body").value
    const data = {
        title: postTitle,
        body: postBody
    }
    
    const options = {
        method: "POST",
        body: JSON.stringify(data),
        headers: {
            "Content-Type": "application/json"
        }
    }
    
    fetch("https://apis.scrimba.com/jsonplaceholder/posts", options)
        .then(res => res.json())
        .then(post => {
           document.getElementById("blog-list").innerHTML = `
                <h3 class="blah">${post.title}</h3>
                <p>${post.body}</p>
                <hr />
                ${document.getElementById("blog-list").innerHTML}
            `
        })
})
```

### Code refactoring

```jsx
let postsArray = []

function renderPosts() {
    let html = ""
    for (let post of postsArray) {
        html += `
            <h3>${post.title}</h3>
            <p>${post.body}</p>
            <hr />
        `
    }
    document.getElementById("blog-list").innerHTML = html
}

fetch("https://apis.scrimba.com/jsonplaceholder/posts")
    .then(res => res.json())
    .then(data => {
        postsArray = data.slice(0, 5)
        renderPosts()
    })

document.getElementById("new-post").addEventListener("submit", function(e) {
    e.preventDefault()
    const postTitle = document.getElementById("post-title").value
    const postBody = document.getElementById("post-body").value
    const data = {
        title: postTitle,
        body: postBody
    }
    
    const options = {
        method: "POST",
        body: JSON.stringify(data),
        headers: {
            "Content-Type": "application/json"
        }
    }
    
    fetch("https://apis.scrimba.com/jsonplaceholder/posts", options)
        .then(res => res.json())
        .then(post => {
            postsArray.unshift(post)
            renderPosts()
        })
})
```

### Reset the form after sending it

```jsx
let postsArray = []
const titleInput = document.getElementById("post-title")
const bodyInput = document.getElementById("post-body")
const form = document.getElementById("new-post")

function renderPosts() {
    let html = ""
    for (let post of postsArray) {
        html += `
            <h3>${post.title}</h3>
            <p>${post.body}</p>
            <hr />
        `
    }
    document.getElementById("blog-list").innerHTML = html
}

fetch("https://apis.scrimba.com/jsonplaceholder/posts")
    .then(res => res.json())
    .then(data => {
        postsArray = data.slice(0, 5)
        renderPosts()
    })

form.addEventListener("submit", function(e) {
    e.preventDefault()
    const postTitle = titleInput.value
    const postBody = bodyInput.value
    const data = {
        title: postTitle,
        body: postBody
    }
    
    const options = {
        method: "POST",
        body: JSON.stringify(data),
        headers: {
            "Content-Type": "application/json"
        }
    }
    
    fetch("https://apis.scrimba.com/jsonplaceholder/posts", options)
        .then(res => res.json())
        .then(post => {
            postsArray.unshift(post)
            renderPosts()
            titleInput.value = ""
            bodyInput.value = ""
            // form.reset()
        })
})
```

# REST - Representational State Transfer

Design pattern to provide a standard way for clients and servers to communicate. HTTP and REST are similar things basically.

## Principles of REST:

### Client and server separation

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%206.png)

Here client and server interact. Here we do not adhere to REST principles. Server sends not just JSON file, but the whole HTML page. It works with computers and phone, they have browsers. But it doesn’t work with devices like smart watch for example. It can’t just take an HTML page and render it (generally we don’t really use browsers because it isn’t comfortable).

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%207.png)

This is RESTful setup. Server responds with JSON data. Server doesn’t care how the client will use the data. Client will insert the data on its page. It won’t rely on the server to render and HTML page and other stuff. And here any device can use the data. It just takes it and does what it pleases with it. 

1. How would you describe what REST is to your non-technical friend?
A standarized way to have your computer, like your laptop, get or send information to another computer (like a server)
2. What does a RESTful API usually return in response to incoming requests?
JSON data
3. What kind of client devices can make use of a RESTful API?
ALL OF THEM.

### Statelessness

When the client makes the request, the server doesn’t maintain any memory about that request. The server sends back the response and forgets about the request and the client. So if the client needs the server to know more information it will send that every time it makes a request. It’s called session state. 

### Accessing “Resources”

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%208.png)

1. What does it mean for the server to be "Stateless"?
It forgets the interaction after the response is sent.
2. In the Mike's Bikes example (https://mikesbikes.com/api), what URL endpoint (and request method)
would you expect to use in order to accomplish the following:
    1. Retrieve a list of all the bikes that are sold?
    GET /bikes
    2. Retrieve detailed information about the bike with an ID of 42?
    GET /bikes/42
    3. Update the price of the bike with an ID of 21?
    PUT /bikes/21
    4. Add a new bike to the list of bikes being sold?
    POST /bikes
    5. Remove the bike with an ID of 56 from the list of bikes?
    DELETE /bikes/56

### Nested Resources

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%209.png)

1. How is a nested resource URL like /bikes/123/reviews
different from an endpoint like /reviews?
/bikes/123/reviews - return an array of reviews about that specific bike
/reviews - return an array of all reviews on the site
2. What URL might you use to GET the review with an ID of 5 on the bike
with the ID of 123?
/bikes/123/reviews/5
3. Describe a "URL Parameter" in your own words:
Variable inside the URL that acts as a placeholder for the real value
(Oftentimes they replace the ID of the resource)

### Query Strings

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2010.png)

A way to filter results.

Different APIs implement query strings in different ways. Read the documentation. 

At Mike's Bikes, they also sell bike racks (endpoint is /bikeracks).

What would you expect the endpoints to be for the following tasks:

1. Get a list of all bike racks sold on the site?
/bikeracks
2. Get a list of all bike racks available in the physical store right now?
(Assume a query called "available" that is a boolean true/false)
/bikeracks?available=true ==> {available: "true"} (Will be parsed as a string)
3. Get a list of all "Thule"-brand bike racks that can hold 4 bikes?
(Assume there are "brand" and "numBikes" queries)
/bikeracks?brand=thule&numBikes=4

# Async JS

## General Information

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2011.png)

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2012.png)

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2013.png)

## setTimeout()

```jsx
const question = 'What is the capital of Peru?'
const answer = 'Lima!'

console.log(question)

// setTimeout(()=> console.log(answer), 3000)
setTimeout(revealAnswer, 3000)

function revealAnswer() {
    console.log(answer)
}

// output
// ›What is the capital of Peru?
// ›Lima! - appears 3 seconds later!
```

It’s important to call a callback function inside the setTimeout 
WITHOUT the `()`

```jsx
function callback() {
    console.log("I finally ran!")
}

setTimeout(callback(), 2000)
// it will work incorrectly
// it instantly displays text in the console

function callback() {
    console.log("I finally ran!")
}

setTimeout(callback, 2000)
// it will work correctly
// the function will still work, but it will display the result 2 seconds later
```

## setInterval()

```jsx

```

## Promises

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2014.png)

Fetch request is seen as a Promise object in the console

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2015.png)

![Untitled](Working%20with%20APIs%20913e0bb3342a436baf5d300dfe0d116e/Untitled%2016.png)

# Async / Await

## Handling Errors

You need to use `try ... catch`

```jsx
const fakeRequest = (url) => {
    return new Promise((resolve, reject) => {
        const delay = Math.floor(Math.random() * (4500)) + 500;
        setTimeout(() => {
            if (delay > 2000) {
                reject('Connection Timeout :(')
            } else {
                resolve(`Here is your fake data from ${url}`)
            }
        }, delay)
    })
}

async function makeTwoRequests() {
    try {
        let data1 = await fakeRequest('/page1');
        console.log(data1);
        let data2 = await fakeRequest('/page2');
        console.log(data2);
    } catch (e) {
        console.log("CAUGHT AN ERROR!")
        console.log("error is:", e)
    }
}
```