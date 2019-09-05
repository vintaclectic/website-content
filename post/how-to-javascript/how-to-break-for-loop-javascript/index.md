---
title: How to break out of a for loop in JavaScript
date: 2019-09-11T07:00:00+02:00
description: "Find out the ways you can use to break out of a for or for..of loop in JavaScript"
tags: js
---

Say you have a `for` loop:

```js
const list = ['a', 'b', 'c']
for (let i = 0; i < list.length; i++) {
  console.log(`${i} ${list[i]}`)
}
```

If you want to break at some point, say when you reach the element `b`, you can use the `break` statement:

```js
const list = ['a', 'b', 'c']
for (let i = 0; i < list.length; i++) {
  console.log(`${i} ${list[i]}`)
  if (list[i] === 'b') {
    break
  }
}
```

You can use `break` also to break out of a for..of loop:

```js
const list = ['a', 'b', 'c']

for (const value of list) {
  console.log(value)
  if (value === 'b') {
    break
  }
}
```

> Note: there is no way to break out of a `forEach` loop, so (if you need to) use either `for` or `for..of`.