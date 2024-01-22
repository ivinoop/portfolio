---
title: "Javascript in Snippets: Intro and Fundamentals"
date: 2021-03-05T19:59:22+05:30
draft: false
description: "There are cults. There are followers. And then, there is an entire world embroiled in JavaScript."
tags: ["javascript"]
cover:
    image: "/blog/javascript-in-snippets/javascript-in-snippets.png"
---

## The History

There are cults. 

There are followers. 

And then, there is an entire world embroiled in JavaScript. A world full of engineers, developers, and designers - all ardently worshipping a language that has taken the web world by storm. Again and again.

JS was introduced to the world 26 years ago. I was introduced to it 9 years ago in a college classroom, and I felt.. nothing. It was taught as just another programming language, with almost the same syntax as the couple of other languages I knew, and to perform the familiar operations of "finding prime numbers", "generating Fibonacci sequence", etc. I learned the "how" but never the "what" or "why" of JS. 

I admit, rather embarrassingly, that back then I did not even know that JS was a language that was developed for the web. Ignorance is **NOT** bliss 😐.

It was originally created for a browser called Netscape Navigator which was competing with Internet Explorer in 1995-1996 to take over the browser domain. Eventually, Internet Explorer won the battle and became the dominant browser at large (back then). 

I know right? **IE**, of all browsers. Yikes!


![confused-nick-young.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1614962260846/H9eOoeqwO.jpeg)

JS slowly evolved to become a higher end programming language that could breathe interactivity to the browser world, and thus began its many avatars to come.

As popularity for JS started growing, ECMA (European Computer Manufacturers Association) was handed over the responsibility of overseeing JS' development, restructuring, and maintenance. The name was changed from JavaScript to ECMAScript, but the former name has remained attached to the language to this day.

## The Present

Undeniably, JS has taken over the world of web as the main language, arguably beating its competitors by a huge factor. Gone are the days when it was only used for just interactivity. Today, we have a plethora of libraries and frameworks that have helped create an entire ecosystem around JS.

Such is the capability and demand that the masses and the indie hackers no less than revere this language for the intuitive, sleek, and nearly flawless experience the web provides today. All hail JavaScript.



![spongebob.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1614962590216/9Deny2RNx.jpeg)

---

## The Learning

After years of forgetting the language and its fundamentals, I started my journey into the world of web development, this time with renewed processes. I now understand the fundamentals in a clearer manner. This post will cover a few of the fundamentals, and the next series of posts will document my learning journey as I tackle the web world with JS. 

Let's dive in!


> **Note:** This post is not a comprehensive one, nor explains the underlying concepts. This is, as the title suggests, a collection of snippets to refer to as a documentation in times of "developer peril😶".

## Say Hello

The first order of things while learning a new language (or relearning for that matter), is to say Hello to the World. It's a time tested tradition for all newbies. However, considering JS is a whole other world (universe?) in itself, let's greet it instead!


![hellojs.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614962786956/t6MIX0plx.png)

**alert()** is a utility function that is essentially used to display a message in the browser. We shall come back to this later.

Another nifty little operator that is helpful is **typeof()**. It is used to identify the data type of a particular expression or variable in use.


![typeof.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614962850706/_0KKwvWX6.png)

In the above example, the variable **a** holds a value of 23. When we test with the **typeof()** operator, the value is identified as a **number.**

## Value Types

There are 2 types of value types in JS.

### Primitive Value Types

These are types that can collect /  hold only one value. There are 5 Primitive types:

* **Number** -  A number type is any integer or whole number, including decimals.
*Examples: 44, -67, 41.67844, 3.1417258, 0.56*

* **String** - String type consists of letters and words encased in any of these quotes:

![string.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614963586301/IT6BG0LUs.png)

* **Boolean** - Boolean data type is a function that can have only one of two values. These are binary in nature. Examples: true or false, 0 or 1, ON or OFF

* **Undefined** - Undefined is a type that indicates that the variable in question is either not assigned a value or is not declared. This is better illustrated below -


![undefined.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614963236717/40B6I3mM1.png)

In the example above, we see that when we declare variable **a**, the status is **undefined** since there is no assigned value. When we check for the **type** of this undefined variable, the value returned is **"undefined"**, which is the primitive value of `undefined`.

* **Null** - Much like undefined, null is also a type that indicates the absence of any value to a variable. However, unlike undefined, null does not have a value "type" to return. When tested for its type in the console, it returns the type as "object" which is what it is treated as while calling objects that are often not relevant.

### Non-Primitive Value Type

* **object** - This is collection of different values of different data types in a single variable. The syntax is as follows - 


![object.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614964197800/jK3bvwx-9.png)

As can be seen above, the values for the variable **userDetails** are enclosed in curly brackets. The values themselves are stored in what is known as **key-value** pairs. Here, **userName** is a key and "Vinoo" its corresponding value. Together, they are a **key-value** pair. Same goes with the **userID** value as well. In the image below, we can see that the console shows the result for the **typeof** value of **userDetails** as "object" data type.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614964415714/08HYNfmIA.png)

***

This concludes the JS In Snippets post. Thanks for reading, and stay tuned for more articles on JavaScript. Keep Shipping!
