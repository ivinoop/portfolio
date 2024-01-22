---
title: "Scope in Javascript"
date: 2023-09-16T21:42:59+05:30
draft: false
description: "Where do variables live in JavaScript? Let's learn about Scoping and the Scope Chain in this post."
tags: ["javascript", "javascript-series"]
cover:
    image: "/blog/scope-in-js/4.png"
---

> This is the 4th post in [The Complete JavaScript Series](https://vinoo.in/tags/javascript-series/).

# What is Scoping?

Scoping is a way to organise and access variables. In JavaScript, there is a concept called Lexical Scoping - which is a way of controlling Scope of variables by the placement of functions and blocks in the code.

## What is Scope?

*Scope* is simply the space or environment in which it is declared.

*Scope of a variable* is a region of the code where the variable can be **accessed**.

In the case of functions, Scope is the variable environment which is stored in the function's execution context.

There are 3 types of scope -

1. Global Scope - for all top level code
    
    * The variables are declared outside of any function or block.
        
    * These variables that are declared in global scope are accessible everywhere in the code.
        
2. Function Scope - this type of scope is created each time a function is declared.
    
    * Variables declared in this function are only accessible within this function, and not outside of it.
        
    * This is also called local scope
        
        ```javascript
        function funcScope() {
          const a = 123
          const b = 321
          const diff = b - a
          return diff
        }
        
        console.log(b) // ReferenceError
        ```
        
        As can be seen above, trying to access the variable `b` gives a `ReferenceError` since we are trying to access it outside its scope.
        
    * It also does not matter what type of function we are using - function declarations, function expressions, arrow functions - all create their own scope.
        
3. Block Scope (ES6) - beginning from ES6, blocks can also create scopes. Blocks are created by curly braces in JavaScript, and any variable within these braces is said to be scoped to that particular block.
    
    * Blocks are created by `if` statements, `for` loops, etc.
        
    * **NOTE:** Block scope is only applicable to variables declared with `let` and `const`.
        
    * `var` variables declared within a block are still accessible outside of it too.
        
    * From ES6 onwards, all functions are also block scoped - *only in strict mode.*
        
        ```javascript
        if(login) {
          const greet = 'Welcome'
        }
        console.log(greet) // ReferenceError
        ```
        
        Here, the `if` condition has created a block so scope of the variable `greet` is restricted to this block. Trying to access it outside of the block results in a `ReferenceError`.
        

## Scope Chain

Let us walk through the below code snippet and examine the scope chain implemented for global and function scopes -

```javascript
const myName = 'Vinoo' // Global variable 

// The below function "first()" is also technically a global variable 
// since all functions and function expressions are considered 
// variables; but for this code snippet we will consider only variable
// declarations for global scope

const myName = 'Vinoo'

function first() {
  const experience = 4

  if (experience >= 4) {
    var seniorDev = true
  }

  function second() {
    const job = 'Developer'

    console.log(`${myName} is a ${experience} years experienced 
    ${job}`)
  }

  second()
}

first()
```

As we can see, `myName` has global scope and functions `first()` and `second()` have function scopes set for the respective variables declared in them. However, notice that the variables `myName` and `age` are being referenced within the function `second` - but their scope is supposed to be limited to the function block of `second()` right?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694854942208/119a3902-052b-4c80-a295-5ea4759b722c.png)

This is where the scope chain comes into play - when one scope needs to use variables from another scope, it will "look up" in the scope chain and see if it can be used. This is called the "variable lookup" in the scope chain. In other words, a scope has access to variables declared in the outer scopes. In this case, the function `second()` looks outside of its scope to access the variable values from outer scopes - from `first()` and the global scope for `myName`.

This is also known as **Lexical Scope** - the ability of an inner function scope to access variables from the parent scope.

> NOTE: Scopes simply look up for the variable's value in the scope chain and never actually copy the variables itself to use in their own scopes.
> 
> <mark>Also, the scope chain only works upward - meaning, only an inner scope can look up the scope chain for variables and not the other way around; an outer scope will never have access to the inner scope variables.</mark>

We are now clear with the scope chain implemented for function scopes. However, in the above snippet, line numbers 6 to 8 contain an `if` block. Remember, only starting from ES6 implementation, we have block scopes functionality with `let` and `const` variable declarations. But, we can see in the snippet, we have a variable declared within the `if` block with `var`.

This means that `var` has no block scope - but it is still limited to the function that it is declared within; it has function scope.

> `let` and `const` are block scoped.
> 
> `var` is function scoped.

Therefore, in our code snippet - the variable `seniorDev` is not scoped to the `if` condition within which it is declared; instead, it is scoped to the function `first()`. Incidentally, the function `second()` can access this since it is declared within the function `first()`.

In essence, the below illustration captures the Scope Chain for the above code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694861549810/5b54d107-77b3-4a68-b92a-1e0f4295916f.png)

---

That was it for Scope and Scope Chain in JavaScript! We have given enough lunges for the brain with this topic.

![Season 3 Friends Tv Show GIF by Friends](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExNjI4ZDhwaWVoOHBzNDY2c3Btc3FkMHo0b3NmaTE5ZDR4amxpaG5jZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/iHsW7t3U7aHkQ9rPva/giphy.gif)

To really get basics right, you should alter the above code snippet and experiment with it by declaring variables in different scopes and tinkering with the flow.

In the next post, we will dive into Hoisting and the `this` keyword - thereby working towards cementing our foundational knowledge of how JavaScript works.

See you in the next one 👋

Keep shipping 🚀