---
title: "What does the double negation operator !! do in JavaScript?"
description: "You might find the `!!` operator in used the wild. What does it mean?"
date: 2019-09-01T07:00:00+02:00
tags: js
---

Suppose you have an expression, which gives you a result.

You want this result to be a boolean. Either `true` or `false`.

Not a string, 0, an empty string, undefined, NaN or whatever. `true` or `false`.

The `!!` operator does that.

And in reality it's two negation operators one after the other. There's no `!!` operator in JavaScript. But there's `!`.

It first negates the result of the expression, then it negates it again. In this way if you had a non-zero number, a string, an object, an array, or anything that's truthy, you'll get `true` back.

Otherwise you'll get `false`.
