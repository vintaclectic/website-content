---
title: How to create a comment in HTML
date: 2019-09-12T07:00:00+02:00
description: "Find out how to add comments to an HTML page"
tags: html
---

A comment in an HTML page is a bit of HTML that is not interpreted by the browser.

A comment is included between the `<!--` and the `-->` tags:

```html
<!-- a comment -->
```

Comments can also span over multiple lines:

```html
<!--
a
comment -->
```

You can use comments in HTML for yourself, to indicate for example why some HTML is present.

You can also use them to remove portion of HTML you wrote but now you don't want to display. For testing purposes, perhaps.

In both cases, the person visiting the site will not see the text you commented in the rendered page, but comments are still visible if they try to view the source of the page.

So, don't keep anything secret or anything you don't want users to see in there.

Plus, remember that page size matters for speed, so you should not have lots of comments in your HTML in production. Little comments are fine, I am more talking about hundreds lines of commented HTML.