---
title: "Hoisting and Temporal Dead Zone In JavaScript"
date: 2023-09-17T21:45:31+05:30
draft: false
description: "What is Hoisting? How are we able to invoke certain variables an functions before they are even declared? Where do other variables go?"
tags: ["javascript"]
cover:
    image: "/blog/hoisting-temporal-dead-zone/5.png"
---

> This is the 5th post in [The Complete JavaScript Series](https://vinoo.hashnode.dev/series/the-complete-javascript).

Before we define Hoisting, you should know that we have already seen Hoisting in action in our previous article and code snippets, and you would probably be writing code every day that implements Hoisting without consciously being aware of it! It is a very tricky concept, one that is simple enough in practice but takes some time to wrap our heads around, given the idiosyncrasies of JavaScript 😃

Let's do a quick recap by examining the code below -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694929325661/33b06d61-a109-4c97-807c-184f45f98725.png)

As we know from our previous [post on Execution Context](https://vinoo.hashnode.dev/javascript-under-the-hood-part-2), we have two phases of code execution - Declaration Phase and Execution Phase. According to that flow -

* In line 1 of above code, `console.log()` is a function call with the variable `username` - and since it is a function call, it will be skipped, and the Execution Context goes to line 2, where variable `sum` is assigned the result of the function call `add(2,3)` . So within the Memory of the Global Execution Context, the variable `sum` is instantiated, with the initial value being `undefined`
    
* Next, we have the function `add(numA, numB)` - so `add` is defined in the Memory, but it is not `undefined` like a variable is. Instead, in the Declaration Phase, variable declaration and function declaration are different in this manner. Variables are initiated with the initial value `undefined` whereas function variables like `add()` in the above code, are initiated as functions - the Execution Context knows that function variables need to be initiated as functions.
    
* Moving on to line 6, `username` is a variable that gets initiated with `undefined` again.
    
* Line 7 again has the `console.log()` function that calls `username` variable, so it is skipped for now.
    
* Finally in line 8, we have a function call `add(10,12)` that is skipped for the Declaration Phase.
    

The execution flow changes from Declaration to Execution phase, and now the flow is like this -

* Since line 1 is a function call for `console.log()`, a Function Execution Context is created for `console.log()`. The value for username is not found in this execution context, so it bubbles out to find it in the parent memory, which is Global EC. Here, the value is found to be `undefined` and so the `console.log()` outputs it as such. The Function EC for `console.log()` is then deleted.
    
* Moving on, now the variable `sum` looks for its value - which is the return value of the function `add(2, 3)`. As usual, a function execution context (EC) gets created for `add()`. How? Because earlier, we saw that function variables are initiated as functions in the declaration phase, unlike normal variables which get initiated as `undefined`. Now in the Declaration phase for this particular EC for `add(2,3)`, it immediately knows that `add()` is a function. And they have the variables `numA` and `numB` - both of which get initiated as `undefined`. Now that declaration phase is over, execution phase begins, and both `numA` and `numB` get the values 2 and 3 respectively - since those are what the arguments are during the function call. It performs that return statement which 2 + 3 - the result 5 is finally stored as the value for the variable `sum` in the GEC's memory and this FEC gets deleted immediately.
    
* Next, in line 6, the variable `username` is assigned the value 'bifrost', so its previous value of `undefined` changes to `bifrost` now.
    
* In line 7, the `console.log(username)` function has the value for `username`, so it logs `bifrost` to the console.
    
* Finally in line 8, `add(10,12)` will return the relevant value since the function has already been declared before, and only the variable values are changed now - previously they were 2 and 3, now they are 10 and 12.
    

<mark>Observe something very crucial here</mark> - in line numbers 1 and 2, we are calling a variable and a function that have not even been declared yet. And still, we do not run into errors. How? This is possible due to Hoisting.

## What is Hoisting?

As the word suggests, it is a concept where variables and functions are "hoisted" up in the flow of execution so that they can be called/invoked even before they are actually declared. And as mentioned earlier, it is not a new concept again, but an existing one which we have already defined and seen in practice - the Declaration Phase in Execution Context.

Because of this Declaration Phase where variables and functions are initiated with their respective values (variables as `undefined` and function variables as functions), the JavaScript engine knows that these values already exist, and hence does not throw any error.

That is it. That is all Hoisting is.

Proof? Here you go -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694941604304/43a7f336-3f5b-49f9-b3b9-9c66da7cf499.png)

In the snippet below, notice how, the `console.log()` for line number 1 already logs a statement with the value `undefined`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695439598484/ce0278e3-b369-40f7-a45a-096fbe2ea7e1.png)

By the time the flow is deeper into the code, and we arrive at line number 8, `username` now has the value assigned as `bifrost` and hence displays the same instead of `undefined`.

Notice how for the variable assignment `sum` - which is a function call of `add()` - it does not throw an error that the variable/function is not defined. Instead, since EC already knows that `sum` is a function variable, it waits for the function declaration also to happen later on, and then returns the value to be assigned to `sum` and then logs it to the console.

In practice, this is how the variables are hoisted by the JS Engine -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694944592265/68c0f549-dd1b-4ca1-b36a-2a75399d244b.png)

Lines 13 - 17 represent declaration phase and lines 19 - 22 represent execution phase.

## But.. we aren't done yet

Notice how the variable `username` was declared with `var` throughout our previous article and this one too - and it was intentional, because there is a subtle difference in how `var`, `let`, and `const` create variables.

The entire premise of using modern ES6 syntax to write our web applications, is to remove inconsistencies and weed out possible bugs/errors during development phase. How so?

As we saw above, functions with the keyword `function` and variables with `var` can be called before declaring them. This is because in both these cases, the functions/variables are *initialised* either with `undefined` (for variables), or as functions for function variables - and hence can be accessed later on. This can seem beneficial for haphazard coding, but in the real world (production), this can lead to huge unintended errors.

Take this small example -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694955611564/99779885-3059-416d-8885-8b1814b26e2b.png)

* As we can see, `cartItems` is declared with `var` in line 5 - but it is already being accessed in line 1. Still, there are no errors, and as usual, it would be initiated with `undefined` - making it possible to be accessed before it is declared.
    
* We have an `if` block that checks for the `truthy` or `falsy` state of `cartItems` and then, only if found to be `falsy`, we want the `deleteCart()` function to be invoked.
    
* Now the problem arises - in line 5, we declare `cartItems` as 10 but still, the `deleteCart()` function is invoked and the damage, so to speak, is done. The cart items are deleted.
    
* Only in line 10, do we see that finally when the execution context has moved out of the declaration phase, and into the execution phase, the value of cart items is updated from `undefined` to 10.
    

Why does the above scenario happen? Hoisting.

Function declarations with the keyword `function` and variables with `var` are both hoisted and this can and will lead to bad errors in a huge application.

But what happens to variables with `let` and `const`? Welcome to Temporal Dead Zone.

## Temporal Dead Zone

`let` and `const` variables are not initialised during Declaration Phase. They do not get an `undefined` or anything of the sort when they are created. Instead, from the beginning of the flow of execution right until the time of their declaration, `let` and `const` variables are said to be in a zone often called the **Temporal Dead Zone.**

Well, it may sound ominous and eerie, but it does suit the state and situation of the variables perfectly. A small illustration -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694962880258/7de9352b-4b6e-4942-ae23-f88860f119d8.png)

As can be seen, the variable `job` declared with `var` has been initiated to `undefined` but the variables `numOne` and `sum` declared with `let` and `const` respectively, are both empty "boxes". And when it comes to execution phase, we get the console statement for `job` but not for the other two variables. In fact, the execution stops as soon as it encounters the second console statement, since it cannot find the initialisation for `numOne` and hence, it terminates the execution flow with the error message - `Error: Cannot access 'numOne' before initialisation` -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694963754820/e9efd0ea-3a9e-4f68-b74b-46bfe289cdc9.png)

This behavioural difference holds true for Function Declarations vs Function Expressions as well.

Simply put - Function Declarations are hoisted, while Function Expressions and Arrow Functions are not. Like so -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694964060113/8c8e6428-1f2c-4d6a-86bf-fb68a292c16b.png)

Here, `add()` is a function declared with `var` so it hoisted and no errors are found. But when it comes to `subtract()`, it a function expression that is assigned to the variable `sub` - and even though it is declared with `var`, it it still an expression, so it does not get hoisted/initialised. The execution stops here with an error message.

Same is the case with `multiply()` - with a different error message like so -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694964237839/fad47de7-b046-4a7b-b5b7-6e83f540a93d.png)

Here, we can clearly see that from the error message, the `` `multiply` `` variable declared with `let`, is a function expression and therefore has not been initialised at all.

## Closing Notes

All of this is a solid showcase of why we need best practices, and why we must adhere to it in "strict" mode while writing real world code. To summarise -

* Use `let` and `const` everywhere. Using `var` is outdated, and unnecessary in almost all modern use cases.
    
* Always, always declare variables and functions before using/invoking them.
    
* Again, use `const` for all function expressions.
    
* You cannot prevent Hoisting from happening, but can certainly control when and where you would need it.
    

---

We have almost made it through the muddy waters - these behind the scenes functionalities and concepts are building blocks for what's ahead - but for now, maybe a small celebratory dance is in order?

![Appu Appurajosh GIF - Appu Appurajosh GIFs](https://media.tenor.com/ZtCb2TdGS0EAAAAC/appu-appurajosh.gif)

Or not 😃

See you in the next one! Until then,

Keep shipping 🚀