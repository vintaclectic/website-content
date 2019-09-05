---
title: How to add item to an array at a specific index in JavaScript
date: 2019-09-13T07:00:00+02:00
description: "Find out how to add item to an array at a specific index in JavaScript"
tags: js
---

Say you want to add an item to an array, but you don't want to append an item at the end of the array. You want to explicitly add it at a particular place of the array.

That place is called the *index*.

> Array indexes start from `0`, so if you want to add the item first, you'll use index `0`, in the second place the index is `1`, and so on.

To perform this operation you will use the `splice()` method of an array. This function is very powerful and in addition to the use we're going to make now, it also allows to delete items from an array. So, proceed with caution.

`splice()` takes 3 or more arguments. The first is the start index: the place where we'll start making the changes. The second is the delete count parameter. We're *adding* to the array, so the delete count is 0 in all our examples. After this, you can add one or many items to add to the array.

Here's an example. Take this array:

```js
const colors = ['yellow', 'red']
```

You can add an item after `yellow` using:

```js
colors.splice(1, 0, 'blue')
//colors === ['yellow', 'blue', 'red']
```

You can add multiple items, after `yellow`, using:

```js
colors.splice(1, 0, 'blue', 'orange')
//colors === ['yellow', 'blue', 'orange', 'red']
```

> Note: the result assumes `colors` is still `['yellow', 'red']`

To add at the first position, use `0` as the first argument:

```js
colors.splice(0, 0, 'blue')
//colors === ['blue', 'yellow', 'red']
```
