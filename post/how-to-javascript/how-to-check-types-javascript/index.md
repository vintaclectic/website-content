---
title: "How to check types in JavaScript without using TypeScript"
date: 2019-08-26T07:00:00+02:00
description: "Find out how to add types to JavaScript without using TypeScript"
tags: js
---

If you haven't been living under a rock, you know something about TypeScript. It's a new language introduced by Microsoft, and it's basically JavaScript with types (and compiles to JavaScript to run in the browser).

Now, I used it in some test projects but I tend to avoid writing my tutorials in TypeScript for various reasons.

The first is that I mostly write beginners tutorials and TypeScript is not usually what people start with.

Also, I think that if I start writing things in TypeScript, I'd introduce confusion - what am I talking about?

TypeScript fans can still make use of JavaScript tutorials, since JavaScript can fit very well into their TypeScript files, while the opposite is not true.

So, I stick to the fundamentals of the Web Platform rather than on technologies that build on top of it.

That said...

There are times where I'd benefit from having types in JavaScript. They are helpful.

Thanks to [this video](https://www.youtube.com/watch?v=YHvqbeh_n9U) by the awesome Paul Lewis, I found that **we can actually have types in JavaScript**, using [Visual Studio Code](/vscode/)!

First, you need TypeScript installed, if you haven't already:

```sh
npm install -g typescript
```

Then you add a `tsconfig.json` file to the root of your project. Assuming you have the JavaScript files in the `src` folder, this is the minimum amount of configuration you need in that file:

```json
{
  "compilerOptions": {
    "outFile": "../../built/local/tsc.js",
    "checkJs": true,
    "allowJs": true
  },
  "include": [
    "src/*"
  ]
}
```

You can decide to exclude folders, for example it's a good idea to exclude `node_modules`:

```json
{
  "compilerOptions": {
    "outFile": "../../built/local/tsc.js",
    "checkJs": true,
    "allowJs": true
  },
  "include": [
    "src/*"
  ],
  "exclude": [
    "node_modules",
  ]
}
```

Now, VS Code can point out type errors in our JavaScript code.

And it can do it automatically, without us having to do anything.

In particular, it can infer the types of function parameters using the default value.

Say we have this function, where `times` is assigned the default value of 2:

```js
const multiply = (aNumber, times = 2) => {
  return aNumber * times
}
```

Now since the second parameter has a default value, we can call this function with

```js
multiply(20)
```

to multiply 20 by 2, or like this to multiply it by 10:

```js
multiply(20, 10)
```

But if you pass, for example, a string as the second parameter like `multiply(20, 'hey')`, VS Code will now tell you there's a problem:

> Argument of type '"hey"' is not assignable to parameter of type 'number'

Awesome!

We can perform this kind of type checking also for arguments that don't have a default value. You can do so using [JSDoc](https://github.com/jsdoc/jsdoc), which is normally used as an API generator, and adding type hints:

```js
/**
 * @param {number} aNumber
 */
const multiply = (aNumber, times = 2) => {
  return aNumber * times
}
```

⚠️ Don't forget the double `**` in the beginning of the comment, otherwise things will not work as expected.

Now if you try to call `multiply('ho!')` you'll get an error too:

> Argument of type '"ho!"' is not assignable to parameter of type 'number'

Other than `number`, you can set the following types:

* `null`
* `undefined`
* `boolean`
* `string`
* `Array`
* `Object`

Example:

```js
/**
 * @param {null} aNull
 * @param {undefined} anUndefined
 * @param {boolean} aBoolean
 * @param {string} aString
 * @param {Array} anArray
 * @param {Object} anObject
 */
const multiply = (aNull, anUndefined, aBoolean, aString, anArray, anObject) => {
  console.log(aNull, anUndefined, aBoolean, aString, anArray, anObject)
}
```

Now, of course not having to add annotations in comments and having the code itself tell you the *truth* would be better. If you can live with this way of doing things, great! Otherwise, there's TypeScript.