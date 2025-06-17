---
title: "Destructuring in JavaScript: Arrays"
date: 2024-01-15T21:54:05+05:30
draft: false
description: "What is destructuring in JavaScript? How do we extract values from arrays and objects?"
tags: ["javascript", "arrays", "javascript-series"]
cover:
    image: "/blog/destructuring-in-js-arrays/8.png"
---

In simple terms, Destructuring is an ES6 feature that helps us unpack values from an array or object into separate variables.

We use destructuring to break a complex data structure down into a smaller data structure like a single dimensional array or a variable.

## What is Array Destructuring?

Array destructuring is a process of extracting values from complex array data structures. Destructuring does not mutate the original array and helps us perform multiple such destructuring operators

For arrays, we use destructuring to retrieve elements from the array and store them into variables.

## Why do we need array destructuring?

Let us look at a practical example -

```javascript
const arr = [2, 3, 4]
const a = arr[0]
const b = arr[1]
const c = arr[2]
```

Notice how we had to individually create a single variable and assign each array value to this variable. Cumbersome right?

With destructuring, the process becomes VERY simple -

```javascript
const [a, b, c] = [2, 3, 4]
console.log(a, b, c) // 2, 3, 4
```

Note that the original array is not changed in any way.

One more thing to observe in the above example is that since the array values are in a sequential order, destructuring them into variables is quite straightforward. As in, `a` will be automatically assigned `2`, `b` = `3`, and `c` = `4` without having to explicitly state it.

But what if there are values somewhere further ahead in the array and we need to skip a few values to access it and assign it to a variable?

There is a quirky yet helpful way to do this.

Take this example where we want to assign a fruit to an individual. `Richard` should be assigned `banana` but the desired fruit for `Bighetti` - `kiwi` - is further ahead in the array.

```javascript
const fruits = ['banana', 'apple', 'orange', 'kiwi']
```

This is how we do it -

```javascript
const [Richard, , , Bighetti] = fruits
console.log(Richard, Bighetti)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705312489223/85ec3b42-4ca2-4119-a0a1-daff1601226e.png)

We simply put "gaps" during destructuring after the single comma operator `,` - which signifies that we intend to skip the values for that particular value. So, `apple` and `orange` are skipped and the variable `Bighetti` is assigned the value of `kiwi`.

## How to swap values using destructuring?

Swapping values is really easy with array destructuring. No more temp variables to be used! Although, if we want to perform more operations on the swapped values later on, we may need to assign them to a variable.

Swapping is as simple as -

```javascript
[Richard, Bighetti] = [Bighetti, Richard]
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705314188433/d514ef9b-9f2b-4ed0-998e-bb0ad608a267.png)

There we go. Previously assigned values of `banana` and `kiwi` to `Richard` and `Bighetti` are now swapped.

## How to destructure nested arrays?

The examples we saw above were for linear arrays or "shallow" arrays. What about destructuring a nested array? Arrays within arrays are a common data structure in real-life codebases. Consider the example -

```javascript
const nested = [1, [2, 3, 4]]
const [i, j] = nested
console.log(i, j)
```

The console statement would this -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705321217292/d603b979-5625-41dd-b7c8-d5a11bee168c.png)

As you can see, the value for `i` is 1 whereas the value for `j` is an array of values - 2, 3, 4. What if we want to return this array of values for `j` as a "flattened" array - as in, the values would be destructured again? Here is how we do that -

```javascript
const nested = [1, [2, 3, 4]]
const [i, [j, k, l]] = nested
console.log(i, j, k, l)
```

And this will return the values as expected -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705321496710/7c56a6b3-9935-438d-80e7-988528ff9514.png)

## Destructuring with default values

Many times, we may not know the exact size/length of the array we are dealing with, and while destructuring, we might try to assign non-existent values to variables. Consider this example -

```javascript
const [p, q, r] = [8, 9]
console.log(p, q, r)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705320912842/79d1bc60-3f12-408d-8777-a6196f41d0f0.png)

While the values for `p` and `q` would be displayed as `8` and `9`, the value for `r` is returned to be `undefined`. As evident, a value for `r` does not exist in the array we are trying to destructure since it only contains two elements, so naturally it returns `undefined`, which is not something we want to deal with while working with large applications since returning `undefined` might break the flow of execution.

In such cases, simply specifying a default value to fall back into, can resolve the situation -

```javascript
const [p, q, r = 1] = [8, 9]
console.log(p, q, r)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705321859734/36858195-7ce0-4a7d-aa65-9b89f02d9717.png)

We can even set default values for all the variables if the array we are working with is dynamic and we are not sure of its values being generated properly.

There are multiple other ways to deal with this situation, especially with libraries like React where we perform a conditional render based on a value's existence or similar conditional check.

---

Destructuring is great on many levels due to its simplicity with the use of assignment operator `=` to extract values and immediately assign them to variables.

As mentioned, destructuring in no way changes the original array; instead it simply helps us assign its elements as values to other variables.

![Trick Experiment GIF by Discovery Europe](https://media4.giphy.com/media/RdJIM4Uesg37eN1ZNL/giphy.gif?cid=ecf05e47a8jtcr9z33k3j8q4fuel6goa7ii31vodt08ygxih&ep=v1_gifs_search&rid=giphy.gif&ct=g)

Keep shipping 🚀