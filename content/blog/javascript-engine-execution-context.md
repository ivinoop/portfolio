---
title: "Javascript Engine and Execution Context"
date: 2023-09-15T21:36:56+05:30
draft: false
description: "Let's study about the JS Engine and Execution Context in this article. Warning - we take a pretty deep dive into the concepts 🔍"
tags: ["javascript"]
cover:
    image: "/blog/javascript-engine-execution-context/3.png"
---

> This post is the 3rd in [The Complete JavaScript Series](https://vinoo.hashnode.dev/series/the-complete-javascript).

## JavaScript Engine

A JavaScript engine is simply put - a program that executes JavaScript code. Each browser has its own JS engine. Firefox has SpiderMonkey, Microsoft Edge has Chakra, Opera has Caraken, etc. These are all customised JavaScript and WebAssembly implementations depending on the browser's architecture. The most widely used is of course, Google Chrome browser and its engine is called V8.

V8 is built mainly with C++, and it powers Chrome as well as NodeJS which is a JS runtime to build server-side applications.

### What does the JS Engine consist of?

The JS Engine has two main components - a Call Stack and a Heap.

The Call Stack is where our gets executed using something called the Execution Context. The Heap is an unstructured memory pool which stores all the objects that our application needs. We will discuss this in detail soon below, but let's rewind for a bit here.

Remember in our [previous post](https://vinoo.hashnode.dev/javascript-under-the-hood-one) we had read that JavaScript is an interpreted language? Well, it is not 100% true since interpreted languages are much slower than compiled languages and modern applications cannot afford to be that slow. This is why the modern JS Engine now has a mix of compilation and interpretation - called Just-In-Time compilation.

> Compilation - The entire source code is converted to machine code once, and a binary file is created which contains this converted machine code; finally this file will be executed later by a computer. Compilation is done "ahead of time".
> 
> Interpretation - The source code is "interpreted" and executed line by line by the interpreter. In other words, compilation and execution are done sequentially, line by line. The downside is that multiple executions of the same source code will be interpreted again and again.

Now that this above difference is clear, JIT is a bit easier to understand. The ahead-of-time compilation still exists, but no file exists to be executed. The execution happens immediately after compilation - which is perfect for JavaScript to do compilation and execution simultaneously rather than line-by-line interpretation.

### How does the code get executed?

As discussed before, our code first enters the JS engine to begin the process of execution.

Once the code enters the engine, it is `parsed` - which means the code is being read. This code is parsed into a data structure called the "AST" - Abstract Syntax Tree.

This AST is then `compiled` into machine code - and finally is `executed` immediately in the Call Stack.

Now this is the most important piece of the puzzle - what is the Call Stack and where did it come from? To understand this, let's start off with a diagram -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694358438254/3b14df0e-6c9b-401c-b13d-074b38cf8bf1.png)

<mark>MDN defines call stack like </mark> [<mark>this</mark>](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack) <mark>- A </mark> **<mark>call stack</mark>** <mark>is a mechanism for an interpreter (like the JavaScript interpreter in a web browser) to keep track of its place in a script that calls multiple </mark> [<mark>functions</mark>](https://developer.mozilla.org/en-US/docs/Glossary/Function) <mark>— what function is currently being run and what functions are called from within that function, etc.</mark>

Essentially, it is used to keep a record of all the execution contexts (more on this below) - global execution context, function execution context, etc - in an order which are then executed by the JS engine.

Consider the following code -

```javascript
function calculate() {
    // random code
    // ..
    add()
    // some other code after add() function
    // ..
}

function add(a, b) {
    return a + b
}

calculate()
```

Here, all the functions, both global and inner, are collected and put into the call stack in a certain order to be executed later on. If there are any Web APIs like fetch, timer, etc then they will be pushed into the call stack for execution only once the call stack is empty - meaning once all the functions within call stack are processed, only then will these API methods be pushed into the call stack. This is because these web APIs are not part of the native JS engine itself; rather, JS gets access to these APIs through the global `window` object. This process can be visualised like so -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694359277270/75dbeacc-3b60-4509-995b-a4cfaf4c6f81.png)

All the callback functions that add and enhance interactivity to our web apps - `onClick`, `setInterval`, etc are first put into a data structure called the Callback Queue. And then, as mentioned before, once the Call Stack is empty, the callback functions are pushed to the Call Stack for execution.

How exactly do these callback functions get pushed to the Call Stack, and when does the Callback Queue know when to push?

That's exactly where the Execution Context as a concept wins 😎 Let's see how it works, below.

## Execution Context

Simply put, Execution Context is an environment created by the JS Engine when code begins executing. This environment is for top level code - variable and function declarations only. Take a look at the diagram below -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694715595272/c4e38902-86e2-4967-a8d5-0ad586b986e9.png)

As you can see, the Execution Context here is named "Global Execution Context" (GEC) - which is what gets created by the JS Engine when the program is run for the first time.

GEC has two sections - one for storing data (variables, functions, etc), and another for execution of the program (returning values). Now that we know what an Execution Context is, let's understand how code is actually executed by the GEC.

Following the creation of a Global Execution Context at the beginning of the program execution, the thread of execution of the code happens in two phases -

1. Declaration Phase
    
2. Execution Phase
    

Consider the following code -

```javascript
let numOne = 21

function square(num) {
  return num * num
}

let result = square(numOne)

let username = 'Arya'

function greet() {
  return `Hello ${username}`
}

greet()
```

Let us examine how this code gets executed phase by phase in the Execution Context. As soon as we run the program, the JS Engine creates a Global EC as we saw earlier.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694714750225/889c67ee-ff2b-44d9-b34f-8a99f5d0f7bc.png)

### Declaration Phase

Once the GEC is created, we enter the Declaration Phase -

* Now, the first line of code - `let numOne = 21` is a variable declaration. Note that declaration and assignment are two different things. `let numOne` is a variable declaration, while `numeOne = 21` is a variable assignment. So, in the GEC, within the Memory, a variable with the name `numOne` is created with its value being currently `undefined` - since we are still in the declaration phase.
    
* The next line, `function square(num) ...` is also a function declaration which has an argument `num` - so, for all purposes, this function is a declaration of variable again. Again, in the memory, a function variable with the name `square` gets created with. Whatever is within the function block is ignored for now.
    
* Next, we have `let result = square(numOne)` which is a variable declaration since we are assigning the function call of `square(numOne)` to a variable called `result`. So, `result` gets stored in the Memory - again with the value `undefined` for now.
    
* Next up, we have the variable `let username = 'Arya'` - same as the above step.
    
* Lastly, we have the function declaration `greet()` - which gets created in the Memory.
    

This entire process of Declaration Phase can be visualised through the illustration below -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694716607362/868d2ba3-7d3c-43e2-ae6c-b1af17b60e0b.png)

### Execution Phaase

Once the Declaration Phase is done, we enter into the Execution Phase. The execution context looks for all function executions like function calls, return statements, etc here. This is the thread of execution that follows for the above code -

* `numOne` is assigned a value of 21, so in the Memory, the value of `numOne` changes from `undefined` to `21`
    
* `function square(num` is just a declaration so this block is skipped.
    
* Next up, we have the `result` variable being assigned a function call `square(numOne)` - this is where an important part of the Execution Context comes into picture.
    

<mark>Every time the thread of execution encounters a function call, the Global Execution Context creates a separate environment - Function Execution Context (FEC).</mark> This means that an FEC is created for each function call in the code execution. And once the function execution is done - which is indicated by return statements - that particular FEC is deleted from the GEC.

So now, continuing the above thread of execution for the variable `result` -

* A separate FEC is created, with its own Memory and Execution sections. Again, this FEC block follows its own Declaration Phase and Execution Phase.
    
    * `result` is assigned a function execution of `square(numOne).`
        
    * Now, `square` was declared earlier with the argument `num` - this argument `num` is also considered a variable declaration for the function `square`.
        
    * A declaration phase begins in the FEC created within the GEC for this particular function.
        
    * `num` is stored in Memory with initial value undefined.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694718753829/065f8837-28e3-451b-b0c6-09b576c36281.png)
        
    * No other declarations are available in `square(numOne)`, so we enter into Execution Phase.
        
    * Now in Execution Phase, we have the value for `num` as 21 from the GEC's Memory; so the value for `num` changes from `undefined` to 21 here.
        
    * The function `square` returns `num * num` so this is executed in the Execute section of the FEC. Since we have the value for `num` now, the value of `21 * 21 = 441` is calculated and returned.
        
    * This returned value of `441` is now assigned to the variable `result` for which this separate FEC was started in the first place.
        
    * Once this function is done executing, the FEC instance for `square` is deleted.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694719108839/09667a6a-d895-4318-b843-a9f0cd4c4bf4.png)
        
* Moving on from `result` now - we have the variable `username` which gets the value `Arya` assigned to it.
    
* Next, the function `greet` is already declared, so we skip this step.
    
* Finally, the `greet` function is called, which means another FEC is created within the GEC for this function.
    
    * Within the FEC, we first go into declaration phase - but since there is no declarations in the function `greet`, we move on to the execution phase.
        
    * The return statement here calls in the value of the variable `username` - which just got a value assigned to it above.
        
    * So the statement `Hello Arya` is returned, which means the execution phase is done, which means the FEC for `greet` function is done, which means that the statement `Hello Arya` is returned to the GEC's Memory, and this particular FEC instance is now deleted.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694719927468/4ad3b013-f35f-42d0-bf46-6ca4188d5938.png)
        

---

THAT. WAS. EXHAUSTIVE.

![Joey Tribbiani Friends GIF](https://media4.giphy.com/media/zm0wMAolIdDvG/giphy.gif?cid=ecf05e47virolmkq5tg0ktc79bwltm76784kiuchkx232fxk&ep=v1_gifs_search&rid=giphy.gif&ct=g)

But in a good way though 🤓

These concepts can take some time, and repetition, to grasp a solid understanding of, but these are important to lay a solid foundation of how we write native JavaScript in various scenarios, and also helpful in debugging.

In the next post, let's study about Scope and the Scope Chain - and why it is important to understand how it works.

Keep shipping 🚀
