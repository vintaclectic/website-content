---
title: "How to create an HTML attribute using vanilla Javascript"
date: 2019-08-30T07:00:00+02:00
description: "What if you have to add an attribute to an HTML element in the DOM, using vanilla JavaScript?"
tags: browser
---

Say you have an element, which you selected using querySelector():

```js
const button = document.querySelector('#mybutton')
```

You can attach an attribute to it following those steps:

1. create the attribute
2. set its value
3. attach the attribute to the element

Example:

```js
const attribute = document.createAttribute('id')
attribute.value = `remove-${item.name}`
button.setAttributeNode(attribute)
```

If the element does not exist yet, you have to first create it, then create the attribute, then add the attribute to the element, and finally add the element to the DOM:

```js
const button = document.createElement('button')
const attribute = document.createAttribute('id')
attribute.value = `some-value`
button.setAttributeNode(attribute)
button.textContent = 'Click me'
document.querySelector('.container').appendChild(button)
```
