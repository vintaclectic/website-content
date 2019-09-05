---
title: How to copy to the clipboard using JavaScript
date: 2019-09-15T07:00:00+02:00
description: "Find out how to copy to the clipboard using JavaScript using the Clipboard API"
tags: browser
---

Sometimes I use sites that provide something I need to copy and paste somewhere. Maybe an API key. Maybe an activation token for an application I just bought.

Anyway, they let you click inside a box, and the text inside it is copied to the clipboard, so I can directly go and paste it somewhere.

How can we implement that functionality in our sites? Using the Clipboard API!

> There's another way to make copy/paste work, using the `document.execCommand()` functionality. I'm not going to cover that option here. The Clipboard API is meant to be the successor of that command.

The Clipboard API is available on the `navigator.clipboard` object:

```js
navigator.clipboard
```

The Clipboard API is relatively recent and not all browsers implement it. It works on Chrome, modern Edge (chromium-based), Firefox and Opera.

You can check for the existence of this object to make sure the functionality is implemented:

```js
if (!navigator.clipboard) {
  // Clipboard API not available
  return
}
```

One thing you must now understand is that you can't copy / paste from the clipboard without the user's permission.

The permission is different if you're reading or writing to the clipboard. In case you are writing, all you need is the user's intent: you need to put the clipboard action in a response to a user action, like a click.

## Writing to the clipboard

Say you have a `p` element in an HTML page:

```html
<p>Some text</p>
```

You create a click event listener on it, and you first check if the Clipboard API is available:

```js
document.querySelector('p').addEventListener('click', async e => {
  if (!navigator.clipboard) {
    // Clipboard API not available
    return
  }
})
```

Now, we want to copy the content of that `p` tag to the Clipboard. We do so by looking up the `innerText` of the element, identified by `event.target`:

```js
document.querySelector('p').addEventListener('click', async e => {
  if (!navigator.clipboard) {
    // Clipboard API not available
    return
  }
  const text = event.target.innerText
})
```

Next, we call the `navigator.clipboard.writeText()` method, wrapping it in a try/catch to handle any error that might happen.

This is the full code of the example:

```js
document.querySelector('p').addEventListener('click', async e => {
  if (!navigator.clipboard) {
    // Clipboard API not available
    return
  }
  const text = event.target.innerText
  try {
    await navigator.clipboard.writeText(text)
    e.target.textContent = 'Copied to clipboard'
  } catch (err) {
    console.error('Failed to copy!', err)
  }
})
```

Here you can see and try the example in Codepen

<p class="codepen" data-height="372" data-theme-id="0" data-default-tab="js,result" data-user="flaviocopes" data-slug-hash="yLBPaVY" style="height: 372px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="A Clipboard API Write example">
  <span>See the Pen <a href="https://codepen.io/flaviocopes/pen/yLBPaVY/">
  A Clipboard API Write example</a> by Flavio Copes (<a href="https://codepen.io/flaviocopes">@flaviocopes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


## Reading from the clipboard

Here's how to read from the clipboard. We have a `p` element:

```html
<p>Some text</p>
```

and when clicking it we want to change the element content with the content stored in your clipboard.

First we create a click event listener and we check for the Clipboard API availability:

```js
document.querySelector('p').addEventListener('click', async event => {
  if (!navigator.clipboard) {
    // Clipboard API not available
    return
  }
})
```

Then we call `navigator.clipboard.readText()`. Using async/await we store the text result into a `text` variable and we use it as the `event.target.textContent` value:

```js
document.querySelector('p').addEventListener('click', async event => {
  if (!navigator.clipboard) {
    // Clipboard API not available
    return
  }
  try {
    const text = await navigator.clipboard.readText()
    event.target.textContent = text
  } catch (err) {
    console.error('Failed to copy!', err)
  }
})
```

The first time you execute this code on your site, you'll see this box appearing:

![Permission to access the Clipboard API](permission-clipboard.png)

You need to grant the permission to the site to read from the clipboard, otherwise if any site could read your clipboard without your explicit permission it would be a huge security issue.

See it on Codepen:

<p class="codepen" data-height="377" data-theme-id="0" data-default-tab="js,result" data-user="flaviocopes" data-slug-hash="JjPORbr" style="height: 377px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="A Clipboard API Read example">
  <span>See the Pen <a href="https://codepen.io/flaviocopes/pen/JjPORbr/">
  A Clipboard API Read example</a> by Flavio Copes (<a href="https://codepen.io/flaviocopes">@flaviocopes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>