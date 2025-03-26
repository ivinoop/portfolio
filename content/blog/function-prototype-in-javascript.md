---
title: "Function Prototype in Javascript"
date: 2021-11-13T20:42:13+05:30
draft: false
description: "What is Function Prototype in JavaScript? Learn how to create objects in an optimised way."
tags: ["javascript"]
cover:
    image: "/blog/function-prototype/function-prototype.png"
---

In the [previous post](https://vinoo.hashnode.dev/oops-in-javascript-what-is-dunder-proto) , we saw how objects behave and what Dunder Proto means. In this post, let us discuss why Dunder Proto is used and how it can help us write better, optimised code. 

Dunder Proto is mainly used for two cases - 

- To manage user methods for the objects that are created at runtime
- To increase usability through better memory management

### So how exactly does this efficiency happen? 

We know that `__proto__` is a special property present in every object that we create in JavaScript. This property presents (and holds) different methods/key-value pairs for each object being created. 

And since every function is also an object, each function holds a set of methods as well that can be invoked right off the bat (like `map()`, `filter()`, etc). 

Here lies the advantage - you can (and should!) put all your methods in one place, in the Dunder Proto. 

Why?

Because since it is already an existing property present in every object, there is no need to explicitly create a variable to manage those methods. Think about it - with each object you create, a whole list of methods get attached to it, leading to a bigger mess in terms of memory management. But by putting it all in the special bag that is the Dunder Proto, it is implicitly managed. 

Example -

```js
let userMethods = {
  sayHello: function() {
    alert(`Welcome ${obj.name}!`);
  },
 changeName: function(newName) {
   this.name = newName;
   return this.name;
 }
};

function createUser(name, age) {
  let obj = Object.create(userMethods);
  obj.name = name;
  obj.age = age;
  return obj;
}
```

As can be seen, the methods `sayHello()` and `changeName()` are both put in a single object variable, which is then assigned to a variable using `Object.create()` method that accepts this object of methods as a parameter. These are now stored in `__proto__` as shown below -


![dunder-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636803878263/d0olmMees.png)

Neat, right? 🥳

There is another property that exists to make your job even more easy, and the code more organised. Say hello to `F.prototype`.


## Function.Prototype

In the previous ways of creating and using object and methods, we used separate variables to store methods and object data. As our application becomes more complex, there are chances of code going out of hand. Using function prototypes can help us organise our code better.

### What is function prototype?

Just like every object has a special property called Dunder Proto, every function in JavaScript also has a property called the Function Protoype. The use case for this property is that since it is a property of a function, it has its own Dunder Proto as well. Take a look below for clarity - 


![dunder-2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636817109034/9V9eh9TIg.png)

Here, the function `foo()` has the `prototype()` property which in turn contains the `__proto__` property. 

This means that there is an even better bag to hold all our object data and methods in one place without the need for a separate function to create objects and a separate variable/object to hold methods. Using the `function_name.protoype` property, we can push all the data and methods to be stored in one single bag, for any number of objects that are created in future. 

Example - 

```js
function Hobbit(name, age) {
 this.name = name;
 this.age = age;
}

Hobbit.prototype = {                  // Using the function.prototype property to put object's methods
  displayName: function() {
  alert(`Hobbit's name is ${this.name}`);
 }
}
```
Below, we can see that the methods as well as data are collected inside this `F.prototype` property. 


![dunder-3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636818162937/8go6iszeM.png)

The question here is - we used the `F.prototype` property to store the methods and data; but how did we instantiate the data to be stored along with the methods?

This is where the `new` keyword in JavaScript comes into picture. 

`new` keyword is used to create an "instance" of an object. An instance here means that -

- A new object is created
- The methods are added to the `prototype` property of the function
- The `this` keyword automatically binds the newly created property to the object (and its prototype)
- The newly created object is then returned as well

Like below - 

```js
let hobbitOne = new Hobbit('Samwell', 120);
```

That's it - a simple `new` keyword to make code (and life) easier 😁

Notice that the code above looks almost the same as the previous method of object creation, except that the object is now returned implicitly with the usage of `new` keyword, as well as the data and methods are managed under a single property with the use of `Function.prototype`.

---

Confused much? 😐 I would expect so; JavaScript is by no means easy. However, the way it offers such flexibility in writing code is what makes it interesting.

In the next article, I go into `class`, `constructor` and how our code can be even more organised than ever, binding all this knowledge neatly. Stay tuned to clear all your confusion 🙂

Until next time 🤠 Keep shipping 🚀