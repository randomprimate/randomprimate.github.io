---
layout: post
comments: false
title: Node.js Intro
date: 2017-06-21 19:52:57 +0000
description: 'Getting started with Nodejs.  '
categories:
- nodejs
tags:
- nodejs
- introduction
image: ''
type: ''
photos: []
headline: ''
modified: ''
mathjax: false
---
# Node.js

> Node.js is just another way to execute code on your computer. It is simply a JavaScript runtime.


## Install 

**TBD**



### Asynchronous Callbacks in node.js

As you saw in the previous example, the typical pattern in Node.js is to use asynchronous callbacks. Basically you’re telling it to do something and when it’s done it will call your function (callback). This is because Node is single-threaded. While you’re waiting on the callback to fire, Node can go off and do other things instead of blocking until the request is finished.

This is especially important for web servers. It’s pretty common in modern web applications to access databases. While you’re waiting for the database to return results Node can process more requests. This allows you to handle thousands of concurrent connections with very little overhead, compared to creating a separate thread for each connection.

## Node CLI

Usage: `node [options] [v8 options] [script.js | -e "script"] [arguments]`

To execute a js script use: `node <script-name>`

You can also run the debuger with `node --inspect <script-name>`. This allows us to add the `debugger` in the script to add a breaking point into a running session. To actually activate this tho we need to open dev tools at:

```
chrome-devtools://devtools/bundled/inspector.html?experiments=true&v8only=true&ws=127.0.0.1:9229/5e2abd65-4ce1-4fda-b9b7-c3cc7f04df67
```

Where `5e2abd65-4ce1-4fda-b9b7-c3cc7f04df67` is generated when launching the session. 

Finally we can also open a  REPL (read-eval-print-loop) where you can execute raw JavaScript code by running `node`.

```node
$ node
> const anarr = [1, 3, 5, 7]
undefined
> anarr
[ 1, 3, 5, 7 ]
> anarr.push(2, 4, 6)
7
> anarr
[ 1, 3, 5, 7, 2, 4, 6 ]
> 
``` 

### Web Server

```node
#!javascript
var http = require('http');
    
http.createServer(function (req, res) {
	res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
}).listen(8080);
    
console.log('Server running on port 8080.');
```

## Reference

\[1\] “Node.js v10.x Documentation.” Node.js, Node.js, nodejs.org/dist/latest-v10.x/docs/api/debugger.html.

\[2\] Weiss, Manuel. “The Absolute Beginner's Guide to Node.js.” Codeship, Codeship, 23 Jan. 2018, blog.codeship.com/node-js-tutorial/.