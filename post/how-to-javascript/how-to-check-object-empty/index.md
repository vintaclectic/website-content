---
title: "How to check if an object is empty in JavaScript"
date: 2019-09-10T07:00:00+02:00
description: "Find out how to see if a variable is equivalent to an empty object"
tags: js
---

Say you want to check if a value you have is equal to the empty object, which can be created using the object literal syntax:

```js
const emptyObject = {}
```

How can you do so?

Use the `Object.entries()` function.

It returns an array containing the object's enumerable properties.

It's used like this:

```js
Object.entries(objectToCheck)
```

If it returns an empty array, it means the object does not have any enumerable property, which in turn means it is empty.

```js
Object.entries(objectToCheck).length === 0
```

You should also make sure the object is actually an object, by checking its constructor is the `Object` object:

```js
objectToCheck.constructor === Object
```

Lodash, a popular library, makes it simpler by providing the [`isEmpty()`](https://lodash.com/docs/#isEmpty)  function:

```js
_.isEmpty(objectToCheck)
```