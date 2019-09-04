---
title: "How to return the result of an asynchronous function in JavaScript"
date: 2019-09-09T07:00:00+02:00
description: "Find out how to return the result of an asynchronous function, promise based or callback based, using JavaScript"
tags: js
---

Say you have this problem: you are making an asynchronous call, and you need the result of that call to be returned from the original function.

Like this:

```js
const mainFunction = () => {
  const result = asynchronousFunction()
  return result
}
```

but `asynchronousFunction()` performs some asynchronous call in it (for example a `fetch()` call), and can't directly return the result value. Perhaps internally it has a promise it needs to wait for, or a callback. Like this:

```js
const asynchronousFunction = () => {
	fetch('./file.json').then(response => {
    return response
  })
}
```

What can you do instead?

Async/await is the most straightforward solution. You use the `await` keyword instead than a promise-based approach, like the one we used before:


```js
const asynchronousFunction = async () => {
	const response = await fetch('./file.json')
  return response
}
```

In this case `mainFunction` does not need to change anything:

```js
const mainFunction = () => {
  const result = asynchronousFunction()
  return result
}
```

The code _looks_ like synchronous code you are used to from other languages, but it's completely async.

Another approach is to use callbacks. But while with async/await we could change just the `asynchronousFunction()` code, in this case we have to

1. modify the `asynchronousFunction()` code
2. modify the `mainFunction()` code
3. modify the calling code, too

Here's an example. `asynchronousFunction()` receives a new function as a parameter, which we call `callback`. It invokes it passing the `response` object:

```js
const asynchronousFunction = callback => {
	fetch('./file.json').then(response => {
    callback(response)
  })
}
```

This function is passed by `mainFunction`:

```js
const mainFunction = () => {
  const callback = result => {
    console.log(result)
  }

  asynchronousFunction(callback)
}
```

The final piece of the puzzle is in the function that calls mainFunction. Since we can't return the response straight from `mainFunction`, because we get that asynchronously, the calling function must change how it processes it.

So instead of `const result = mainFunction()`, we could use



```js
const callbackFunction = result => {
  console.log(result)
}

const mainFunction = callback => {
  asynchronousFunction(callback)
}

//call the code

mainFunction(callbackFunction)
```
