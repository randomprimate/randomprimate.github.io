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
source: [https://blog.codeship.com/node-js-tutorial/](https://blog.codeship.com/node-js-tutorial/ "https://blog.codeship.com/node-js-tutorial/")  
  
> Node.js is just another way to execute code on your computer. It is simply a JavaScript runtime.  
  
Once installed we can open a  REPL (read-eval-print-loop) where you can execute raw JavaScript code by running \`node\`.   
  
\`\`\`  
\$ node  
> const anarr = \[1, 3, 5, 7\]  
undefined  
> anarr  
\[ 1, 3, 5, 7 \]  
> anarr.push(2, 4, 6)  
7  
> anarr  
\[ 1, 3, 5, 7, 2, 4, 6 \]  
>   
\`\`\`  
  
Now to run a file with node.  
  
\`\`\`  
\# hello.js  
console.log("Hello World")  
\`\`\`  
  
\`\`\`  
\$ node hello.js  
\$ Hello World  
\`\`\`  
  
\###  Asynchronous Callbacks in node.js  
As you saw in the previous example, the typical pattern in Node.js is to use asynchronous callbacks. Basically you’re telling it to do something and when it’s done it will call your function (callback). This is because Node is single-threaded. While you’re waiting on the callback to fire, Node can go off and do other things instead of blocking until the request is finished.  
  
This is especially important for web servers. It’s pretty common in modern web applications to access databases. While you’re waiting for the database to return results Node can process more requests. This allows you to handle thousands of concurrent connections with very little overhead, compared to creating a separate thread for each connection.  
  
\### Web Server  
  
\`\`\`  
\#!javascript  
var http = require('http');  
  
http.createServer(function (req, res) {  
  res.writeHead(200, {'Content-Type': 'text/plain'});  
  res.end('Hello World\\n');  
}).listen(8080);  
  
console.log('Server running on port 8080.');  
\`\`\` 