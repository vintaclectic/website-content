---
title: Why you should not modify a JavaScript object prototype
date: 2019-09-14T07:00:00+02:00
description: "Find out why you should not modify a JavaScript object prototype and what to do instead"
tags: js
---

As programmers, one of the skills we must first learn is how to search for a solution.

Google is your friend. And most of the times a StackOverflow answer back from 2009 is the perfect solution to your 2019+ problem.

On that specific site, or on personal blogs, I sometimes see code that modifies built-in Objects prototypes.

Like in this example, where the Array object prototype is enhanced by adding an `insert` method:

```js
Array.prototype.insert = function(index, item) {
  this.splice(index, 0, item)
}
```

In this way, you can have any array and call insert() on it:

```js
['red', 'blue'].insert(0, 'yellow')
```

It's handy. Instead of having to define such function and worry about the scope of it, you attach it to the Array object, so that every array has it available.

But just because you *can*, it does not mean you *should*.

What's wrong with this approach?

## Possible conflicts

Suppose a library you use implements such thing. And another library you import does the same. Perhaps the methods work slightly differently, and things seem to work fine until they don't.

You have a big problem here, because you can't modify those libraries but you still want to use them.

## Future-proofing your code

Suppose the next version of JavaScript implements the `Array.insert` method. With a different signature. Now what happens? You need to go back and rewrite all the code you wrote. Perhaps for a client you don't work for anymore.

Or maybe you did this in a library that's used by other people in their own projects. That would be even worse.

This approach only creates technical debt and pretty much invites problems.

## What should you do instead?

Create a function in a library file and import it when you want to use it. Don't modify objects you have no control over.