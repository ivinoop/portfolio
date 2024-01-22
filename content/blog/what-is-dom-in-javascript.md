---
title: 'What is DOM in JavaScript?'
date: 2023-08-16T20:57:48+05:30
draft: false
description: "DOM forms the core of everything we see on the web page in a browser. Let's understand what it is and how it works."
tags: ['javascript', 'javascript-series']
cover:
  image: '/blog/what-is-dom-in-js/1.png'
---

## Intro

DOM - stands for Document Object Model. Think of it as a tree that with multiple branches that fan out to indicate and serve different elements on the web page. Now, each branch can be referred to by its own "name".

How do we give the name? This is done through the HTML document of the web page we just talked about. So essentially, the DOM is a structured representation of these documents, and by using JavaScript and its various built-in methods, we can access these documents to manipulate them.

Now, that's a strong word. What exactly do we "manipulate" here? Simply put, we can access, modify, and control the behaviour, look, and feel of all the elements on a web page represented by the DOM tree. This includes changing text, HTML attributes, CSS styles, and more. Yay!

## Where is DOM located?

But where does the DOM come from? The DOM is automatically created by the browser as soon as our web loads, and is then stored in a tree structure somewhat visually imagined as below -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691504302433/0e25ea02-c2b5-4a26-918a-2fd03d740a1b.png)

The "document" - essentially the entire web page once it is loaded by the browser - is an object that we can access, in JavaScript. Think of it as the entry point into the DOM. There are various methods (functions) available in the document object, `querySelector` being the most common one. Hierarchically, the `html` element is the first "child" element of the document object. It serves as the root element for all other elements on the page.

From there, it's a complete tree of elements with all the sibling and child elements nested according to their semantic value.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note that the <code>document</code> object and its inherent functions are NOT equivalent to the language JavaScript itself. JavaScript is a "dialect" of the larger <a target="_blank" rel="noopener noreferrer nofollow" href="https://www.ecma-international.org/publications-and-standards/standards/ecma-262/" style="pointer-events: none">ECMAScript</a> ecosystem. The DOM and its functions &amp; properties actually come from <code>Web APIs</code> which are libraries available through the browser, and which can be accessed by JavaScript.</div>
</div>

There is an official DOM specification that browsers implement which is why DOM manipulation works the same across all browsers.

## How to perform DOM operations?

Now that we a basic understanding what the DOM is, let us see how we can work with the DOM and apply various methods to make our web page interactive.

As mentioned before, the DOM comes with a collections of functions and properties which we can invoke in any ECMAScript dialect. Which means, JavaScript as a language, is able to call these functions just like any other method.

### DOM Properties and Methods

A few of the most common `document` methods are -

- `querySelector` - enables us to query the DOM and select a particular element with its class name. It can either be a single element with a unique class name or multiple elements with the same class name. In case there are multiple elements with the same class name, then they are all queried with `querySelectorAll` and stored in something called a `Nodelist` - which is an array of all the items/elements we have queried.
  Suppose we have a `div` HTML element with the class name `main-content`, we query it like below -
  ```javascript
  // Query the element and assign it to variable
  const mainContent = document.querySelector('.main-content')

  /** NOTE - we can see that there is a "." (dot) before the class name 
  in the query method. This is mandatory to be applied to query 
  all class names
  */
  ```
- `getElementById` - this is similar to `querySelector` but as the method name suggests, it is used to the query the element by its unique `id` name. `id` names are unique since no two elements can have the same `id`. If our `div` element above had an `id` of `#main-content`, then we query it by -
  ```javascript
  const mainContentID = document.getElementById('main-content')

  /** NOTE - there is no need to have a "." or a "#" to specify
  the id name since getElementById does the work for us 
  */
  ```
- `addEventListener` - all interactivity that we would need for our web application, is going to be defined in and handled by this method. Think of this as the one major method that we can use to keep track of various interactions - touch, single click, double click, triple click (we can even customise the number of clicks! 😁) mouse hover/movements, etc. There are a huge number of properties that this method has to offer and we will possible dive deeper into it in the future articles. Example -
  ```javascript
  const btnElement = document.querySelector('.main-btn')
  btn.addEventListener('click', function() {
     // define interaction/function here
  }
  ```
  As can be seen here, we attach the `addEventListener` method to the element with which we want to interact and then define the action we want performed once the "event" (click, touch, etc) happens.

We can even add, remove, and toggle classes dynamically after an event/action happens. Few of the methods are -

- `classList.add()` and `classList.remove()` - as the name suggests, `classList` is a property that returns the CSS classes for the element. We can then use methods like `add()` and `remove()` to manipulate the same. A very abstract random example -
  ```javascript
  const mainDiv = document.querySelector('.main-div')
  if (isLoggedIn) {
    mainDiv.classList.add('login-bg')
  } else {
    mainDiv.classList.remove('header-bg')
  }
  ```
- `classList.toggle()` - this is an interesting one, and very useful too. This helps us check if the class we want for an element is already added, and if not, adds it - and vice-versa. Removes a class if it already exists. A simple example to switch players when a certain condition is met -
  ```javascript
  const playerOne = document.querySelector('.player-one')
  const playerTwo = document.querySelector('.player-two')
  document.getElementById(`current--${activePlayer}`).textContent = 0
  activePlayer = activePlayer === 0 ? 1 : 0
  player0El.classList.toggle('player--active')
  player1El.classList.toggle('player--active')
  ```
- `createElement()` - we don't always have to have an HTML structure defined beforehand modifying it via DOM. The `createElement()` method helps us create new elements and then add them to the DOM by using other methods (discussed in the next point). Example -
  ```javascript
  const paragraphSection = document
    .createElement('section')
    .classList.add('my-section')
  const paragraphWrapper = document.createElement('div').classList.add('my-div')
  const paragraph = document.createElement('p').classList.add('my-paragraph')
  ```
- `appendChild()` - once we have created an element from the above method, we then add or "append" it to the DOM using the `appendChild()` method. This adds the new element as the last child of the parent element. If the element we are adding already exists, somewhere higher up in the tree, then using this method will remove it from its existing position and add it to the bottom. Continuing the previous example -
  ```javascript
  mainEle.appendChild(paragraphSection)
  paragraphSection.appendChild(paragraphWrapper)
  paragraphWrapper.appendChild(paragraph)
  ```
  This would create the below structure in the DOM - (notice in the earlier example that we added class names to each element using the `classList` property)
  ```xml
  <main>
    <section class='my-section'>
      <div class='my-div'>
        <p class='my-paragraph'></p>
      </div>
    </section>
  </main>
  ```
- `removeChild()` - as the name makes it clear, it simple removes the specified element from the DOM. More specifically, the parent HTML element calls this method with the child element's name specified, which then removes the child element from the tree.

There are a lot more properties and methods that the `document` object has to offer - a thorough explanation and references can be found at MDN documentation. This post has covered the basics of DOM and how to get started with making simple modifications in DOM tree to add any kind of interactivity we need for our application.

> This is the first post in [The Complete JavaScript Series](https://vinoo.in/tags/javascript-series/). Stay tuned for more!

In the upcoming posts, we will dive deeper into the workings of JavaScript and explore a few rarely encountered concepts.

Keep shipping 🚀
