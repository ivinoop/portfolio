---
title: "Dunder Proto &  Nature of Objects"
date: 2021-10-27T20:38:18+05:30
draft: false
description: "What is Dunder Proto in JavaScript?"
tags: ["javascript"]
cover:
    image: "/blog/dunder-proto/dunder-proto.png"
---

In this post, we shall dive a bit deeper into nature of objects, and cover the Dunder Proto concept. 

### Nature of Objects

Consider the below object - 

```js
let details = {
name: 'Richard Hendricks',
company: 'Pied Piper',
};
```

In the above object, if we try to access the property `company`, it is possible since `company` is an existing property of the `details` object.

However, the below snippet would return `undefined` - 

```js
console.log(details.designation); //undefined
```

This is because there is no property named `designation` inside `details`. This is exactly how we would expect an object to behave. 

However, take a look at the example below - 

```js
let arr = [1, 2, 4, 5, 7];

console.log(arr.map( () => 21 );
```
The output would be as below - 

![objects in detail - 1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635327489653/-cf1poKOp.png)

But `map()` is not a method inside `arr`. So how is this being computed and where is this coming from?

### Dunder Proto `__proto__` 

Inside every object in JavaScript lies a special property called `Dunder Proto`. The name is coined due to the way this object is represented - `__proto__` (accompanied by double underscore on both sides of the word `proto`). 

As we can see in the above image, the object `arr` (and basically every object you create in JS), has the `[[Prototype]]:Array` property, inside which lies `__proto__`. If we expand this `[[Prototype]]: Array` property in our example, we should be able to see `__proto__`, which in turn contains a huge list of methods like `every`, `forEach`, `map`, `splice`, etc. 


![objects in detail -2 .png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635329616441/aaK7d_B78.png)

The point to be noted here is that each object we create has a different set of key-value pairs in the `__proto__` property. 

Whenever we try to call/access a property that does not exist in the defined object, the JS engine goes down the `__proto__` chain (or a rabbit 🐇 hole), to search for that property. In the above case, we tried to compute the `map()` method on an array (which is an object), and it went down the `__proto__` chain to look for the same. 

This is how the hidden nature of object allows for all array, object, and string methods to be carried out. 

Since `__proto__` is a special property of an object, it can be accessed as well. Suppose you want to add a new property under `__proto__` to the `details` object above, this is how to do it -

```js
details.__proto__.alertMsg = function () {
alert(`Hello Dunder Proto => __proto__`);
}
```

This function is now added to the `__proto__` property as can be seen below - 


![objects in detail -3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635333264025/PAWNhCcBp.png)

---

We learnt a hidden nature of objects in JavaScript, and the basics of Dunder Proto. In the next post, we shall learn about why and where Dunder Proto can be used to make our code more efficient. 

Until next time! 🙌