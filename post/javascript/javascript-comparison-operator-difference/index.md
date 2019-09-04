---
title: "Which equal operator should be used in JavaScript comparisons? == vs ==="
description: "You can use two different operators to check for object equality. They are `==` and `===`. Which one to use?"
date: 2019-09-02T07:00:00+02:00
tags: js
---

In JavaScript you can use two different operators to check for object equality. They are `==` and `===`.

They basically do the same thing, but there is a big difference between the two.

`===` will check for equality of two values. If they are objects, the objects must be of the same type. JavaScript is not typed, as you know, but you have some fundamental types which you must know about.

In particular we have value types (Boolean, null, undefined, String and Number) and reference types (Array, Object, Function).

If two values are not of the same type, `===` will return false.

If they are of the same type, JavaScript will check for equality.

With **reference types**, this means the values need to reference the *same* object / array / function. Not one with the same values: the same one.

`==` is different because it will attempt to convert types to match.

This is why you get results like

```js
false == '0'  //true
null == undefined //true
```

In my experience, in 97% of the cases you'll want to use `===`, unless `==` provides exactly what you want. It has less drawbacks and edge cases.

The same goes for `!=` and `!==`, which perform the same thing, but negated.

Always default to `!==`.