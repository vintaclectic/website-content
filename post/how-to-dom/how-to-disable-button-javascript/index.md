---
title: "How to disable a button using JavaScript"
date: 2019-09-16T07:00:00+02:00
description: "Find out how to programmatically disable or enable a button using JavaScript"
tags: browser
---

An HTML button is one of the few elements that has its own **state**. Along with almost all the form controls.

One common thing that's needed is to disable / enable the button programmatically using JavaScript.

For example you want to only enable the button when a text input element is filled.

Or when a specific checkbox is clicked, like the ones you see to say "I read the terms and conditions", something that no one actually reads.

Here's how to do it.

You select the element, using `document.querySelector()` or `document.getElementById()`:

```js
const button = document.querySelector('button')
```

If you have multiple buttons you might want to use `document.querySelectorAll()` and loop through the results.

Anyway, once you have the element reference, you set its disabled property to `true` to disable it:

```js
button.disabled = true
```

To enable it back again, you set it to `false` to enable it again:

```js
button.disabled = false
```