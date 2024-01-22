---
title: "Javascript Under the Hood"
date: 2023-08-17T21:04:55+05:30
draft: false
description: "It is fun and interesting to learn about what makes up JavaScript's building blocks as a language."
tags: ["javascript", "javascript-series"]
cover:
    image: "/blog/javascript-under-the-hood/2.png"
---

> This is the 2nd post in [The Complete JavaScript Series](https://vinoo.in/tags/javascript-series/).

Let's take a breather from the code editor today - in the next immediate post, we would be reading concepts along with code examples, but for this post, let's make some time to simply read through some interesting things about JavaScript as a language. Let's go 👇

## What Actually IS JavaScript?

### High Up There

JavaScript is one of the "high level" languages in the programming ecosystem. The reason is that it abstracts away a lot of system level configuration that we would need to do in C language - creating and storing variables, hardware resource allocation, memory management etc.

Languages like Python and JS are designed in such a way that we do not need to manage any of these. While this is the upside, the downside is that we compromise on performance, efficiency, and speed.

One of these abstractions is "Garbage Collection" that is present in the JS engine. It automatically removes old, unused objects from the memory.

### Interpreting It Right

Computers only understand binary and in order for us to write machine code that is both understandable to the computer and readable by humans, programming languages are designed to accommodate this. And JavaScript, is no different. Its abstraction layers in the JS engine convert our readable code into machine code.

This makes JS an "interpreted" language.

### OOP(s?)

How do arrays get all their methods (push, shift, splice, etc) ready to use as soon as we define them? We never defined the "properties" of an array, let alone instruct what functions/methods that can be called upon it.

Well - to highly abstract it to the point of intellectual danger, here is an explanation - this is possible due to ALMOST everything being an object in JS (stress on the word "almost"!).

Except for primitive values like numbers and strings, entities like arrays are treated as objects too. Arrays are created from the "prototype" in JS - a blueprint/template that contains all the methods defined in it. The arrays that we create then "inherit" these methods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692294741492/d875f852-1a15-4897-a32c-037d5df2e49a.gif)

We will cover a lot more in detail about objects and their workings in a later post where this abstraction - and confusion - will be replaced with the actual insight into how objects and methods work.

### Functional First

In addition, functions in JS are known as "first-class functions" - a term used when functions are treated as variables. This is what gives a programming language like JS the ability to pass functions into other functions (callbacks, references, etc), return functions from functions, etc. More commonly, this is known as `functional programming`.

### Type Free?

One other important feature (or a bug?) is that JavaScript is a dynamically typed language. Now, not to spark debates - but there are pros and cons to it, and with JS having a strong hold over all kinds of applications (seriously - JS is almost everywhere in every major app now 🥲), the need for "strongly" typed languages is very much the case.

We do not explicitly assign data types to variables. We just declare variables with the necessary scope, but the type is assigned when the JS engine executes our code.

Quirky fact - a `let x = 23` can look like it's a number, but when we use `typeof` to console `x` while playing around with the DOM - surprise, it's a string.

We can reassign the type of variables when we reassign values to those variables. This is what a dynamically typed language looks and works like.

This is a major reason why scalability takes a hit when apps use pure JS. And also why TypeScript is fast gaining popularity and in fact, becoming a de facto choice for apps across the developer ecosystem.

Still - not to worry - we will still be studying everything about JS, building secure, fully functioning apps with JavaScript like everyone has been for the last 2 decades 📈, and JS will still continue to be awesome for many years to come.

> Very soon, I will be creating a complete TypeScript series as well, so watch out for it 🤓

### Thread-ing Lightly

In Operating System terminology, a thread is like a set of instructions that is executed in the CPU. The thread is where our code is executed in a machine's processor.

Now, JavaScript is a single threaded language. This means at any given time, only one single task (or one set of tasks) can be executed. This is obviously a huge pitfall - what if there is a thread that fetches data from an API and the response takes a very long time? This would block the processor from running any other tasks.

This is where the concurrency model comes into play. A concurrency model defines how multiple tasks should be handled by a programming language. In the case of JS, we need a non-blocking behaviour which can be achieved by using an "event loop".

The `event loop` takes complex/long running tasks, executes them in the background, and then puts them back in the main thread once they are done.

Again, this is simply an oversimplification and we will definitely dive deeper in the next post.

---

Well, that was fun (I hope!). Studying a programming language's core fundamentals is very essential since it helps us "think" in that language in the long run. The next post will be up soon and it will cover concepts like the JS Engine, Runtime, Execution Context, the Call Stack, Scoping, Hoisting, and much more.

Keep shipping 🚀

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692295179658/d48d278b-a926-4cee-a16e-14b93c46693f.gif)