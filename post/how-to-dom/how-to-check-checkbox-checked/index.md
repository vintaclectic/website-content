---
title: "How to check if a checkbox is checked using JavaScript?"
date: 2019-09-08T07:00:00+02:00
description: "Find out how to check the state of a checkbox, looking if it is checked or not, using JavaScript"
tags: browser
---

Inspect the `checked` property of the element.

Say you have this checkbox:

```html
<input type="checkbox" class="checkbox" />
```

You can see if it's checked using

```js
document.querySelector('.checkbox').checked
```

You can also check if looking for `.checkbox:checked` does not return `null`:

```js
document.querySelector('.checkbox:checked') !== null
```

but I think looking for `.checked` is cleaner.

Do NOT use `getAttribute()` looking for the `checked` attribute value, because that's always true if the checkbox is checked by default in this way:

```html
<input type="checkbox" checked />
```

Also don't check for the `value` of a checkbox element. It's always `on`, regardless whether the checkbox is checked or not.