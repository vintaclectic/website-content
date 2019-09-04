---
title: "The definitive guide to XSS"
description: "A guide to cross-site scripting attacks. How do they work? How can you prevent them?"
date: 2019-09-03T07:00:00+02:00
tags: security
---

## What is XSS, aka Cross Site Scripting?

XSS is the term we use to define a particular kind of attack where a website (your website, if you don't pay attention) might be used as a vector to attack its users, because of an insecure handing of the user input.

Basically a bad actor (the attacker) can inject JavaScript, in some way on another, into our site, by taking advantage of a vulnerability we left in our code.

Using this vulnerability, they can steal user's information.

Based on how the XSS vulnerability is exploited, we have 3 major kinds of XSS attacks:

- persistent XSS
- reflected XSS
- DOM-based XSS

We'll see all about them very soon.

## Why is XSS dangerous?

Imagine you have a website. An attacker can somehow inject JavaScript code that is served by your website, and it's executed - without your will, and without you knowing about it - by your user's browsers.

This is very dangerous.

Due to your negligence in fixing a XSS vulnerability, your site can be used as an attack vector and your users information is at risk.

In particular, when anyone can inject JavaScript in a page, they can have access to the user's cookies associated with the website, and read any information contained in them. And send this to their own servers. They can listen for keyboard events, and get access to anyone the user types into the page and send it to the attacker's servers using fetch or XHR. Usernames and passwords, for example. They can also manipulate the DOM and with this power they can perform lots of bad things.

## Is XSS a frontend or backend problem?

It's both. It's a website architectural problem that involves both the frontend and the backend.

## An XSS attack example

XSS is basically enabled when you allow the user to enter information, which you store (in your backend) and then present back.

Say you have a blog, and you allow users to comment in the blog. If you blindly accept any content a users enter, a malicious user can try to enter a JavaScript snippet, in its most basic form enclosed within `<script></script>`. For example `<script>alert('test')</script>`.

You might store this comment in your database, and when the page is reloaded - again, if you don't have any prevention in place - all the users loading that page will run that JavaScript snippet.

I used a simple `alert()` call to make an example, but as listed above a user could enter any kind of script. At this point, the site is compromised.

## What is persistent XSS?

Persistent XSS is one of the 3 kinds of XSS we find in the wild. It's the one I described above in the blog post example.

In this case, the code for the vulnerability is stored in the database or into some other source, which is hosted by yourself.

Once someone can enter a JavaScript snippet, it's served automatically by your site without any other action required.

## What is reflected XSS?

Reflected XSS is a way to exploit a vulnerability in your site on-the-fly by providing the end user a link that has a script inside it.

In this way, the attacker provides a link similar to

```
yoursite.com/?example=<script>alert('test')</script>
```

If your site uses the `example` GET variable to perform something and display it on the page, and you don't check and sanitize its value, now that script will be executed by the user's browser.

A typical example is a search form. It might live in the `/search` URL and you might accept the search term using the GET `term` variable.

You might display the `You searched for <term>` string when someone searches for it. Now, if you didn't sanitize the value, you now have a problem.

Spam/phishing emails are a common medium for this XSS attack. Of course, the bigger and more important the site, the more frequently hackers will try to hack it.

## What is DOM-based XSS?

With persistent XSS, the attacking code must be sent to the server, where it can be (and hopefully it is) sanitized. With reflected XSS, the same is true.

DOM-based XSS is a kind of XSS where the malicious code is never sent to the server. It's common for this to happen by using the fragment part of a URL, or by referencing `document.URL`/`document.location.href`. Some examples you find online don't really work any more because modern browsers automatically escape JS in the address bar for us. They only work if you unescape it, which is kind of scary (don't do it!).

Here's a simple working example. Say you have a page listening on http://127.0.0.1:8081/testxss.html. Your client-side JavaScript looks at the `test` variable passed in the fragment part of the URL:

http://127.0.0.1:8081/testxss.html#test=something

The `#test=something` value is never send to the server. It's only local. Persistent/reflected XSS would not work. But say your script accesses that value using:

```js
const pos = document.URL.indexOf("test=") + 5;
const value = document.URL.substring(document.URL.indexOf("test=") + 5, document.URL.length)
```

and you write it directly into the DOM:

```js
document.write(value)
```

All is fine, until someone calls the URL like this:

http://127.0.0.1:8081/testxss.html#test=<script>alert('x')</script>

Now, thanks to the automatic escaping that happens by referencing `document.URL` nothing should happen in this specific case.

You'd get

```
%3Cscript%3Ealert('x')%3C/script%3E
```

printed to the page. The value is escaped, so it's not interpreted as HTML.

But if for some reason you unescape the document.URL value, you have a problem now, as the JavaScript is run. Any JS can be run on your users browsers.

> On older browser, this was a much bigger problem, since they didn't auto-escape JS put into the address bar.

## Are static sites vulnerable to XSS?

Yes! Any kind of site, actually. Because being static does not mean there is no information loaded from other sources. For example you might roll your own form or comments, even without a database.

Or, we might have a search functionality that accepts input from an HTTP GET or POST variable. You are not immune just by not having a database.

## How can we prevent XSS?

There are 3 techniques we can use:

- encoding
- validation
- CSP

Encoding is done to escape the data. Doing so, the browser will not interpret the JavaScript because, for example, `<script>` will be encoded to `%3Cscript%3E`.

Encoding, as a general rule, should be always done.

Server-side frameworks commonly provide helper functions to provide this functionality to you.

In client-side JavaScript we use a different encoding mechanism depending on the use case.

**If you need to add content to an HTML element**, the best way is to assign the user-generated input to that element using the `textContent` property. The browser will do all the escaping for you:

```js
document.querySelector('#myElement').textContent = theUserGeneratedInput
```

**If you need to create an element** use `document.createTextNode()`:

```js
const el = document.createTextNode(theUserGeneratedInput)
```

**If you need to add content to an HTML attribute**, use the `setAttribute()` method of the element:

```js
document.querySelector('#myElement').setAttribute('attributeName', theUserGeneratedInput)
```

If you need to add content to the URL, use the [`window.encodeURIComponent()`](/how-to-encode-url/) function:

```js
window.location.href = window.location.href + '?test=' + window.encodeURIComponent(theUserGeneratedInput)
```

Validation is usually done when you cannot use escaping to filter the input. A common example is a CMS that lets the user define the content of the page in HTML. You can't escape that.

You either use a blacklisting or whitelisting strategy for validation. The difference is that with blacklisting you decide which tags you want to disallow. With whitelisting you decide which tags you want to allow. Whitelisting is safer because blacklisting is prone to errors, complex and also not future-proof.

CSP means Content Security Policy. It's a new standard implemented by browsers to enforce only executing JavaScript code coming from secure and trusted sources, and you can disallow running inline JavaScript in your code. The kind of JavaScript that allowed the above XSS exploits, for example.

CSP is enabled by the Web Server, by adding the `Content‑Security‑Policy` HTTP Header when serving the page.