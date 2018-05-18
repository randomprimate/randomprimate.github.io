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

You can also run the debugger with `node --inspect <script-name>`. This allows us to add the `debugger` in the script to add a breaking point into a running session. To actually activate this tho we need to open dev tools at:

    chrome-devtools://devtools/bundled/inspector.html?experiments=true&v8only=true&ws=127.0.0.1:9229/5e2abd65-4ce1-4fda-b9b7-c3cc7f04df67

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
```

### Web Server

This example is taken from the `nodejs` documentation. We are going to build a web server relying on Node's `htttp` module.

Create the following fila and run it with the node cli: `node web-server.js`

```node
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

The result should be the following console message:

```bash
Server running at http://127.0.0.1:3000/
```

## Node Package Manager

NPM for short is a JavaScript package manager. NPM is installed with `nodejs`.

Confirm by checking the version with `npm -v`.

There are a variety of modules available with which we are able to extend our node application functionality.

### Install an npm package

There are two ways to install packages: locally or globally. When you are using the package from another module or project, requiring it in your code then you will use a local installation. When using an npm package as a tool such as Angular CLI then you'll need to install this globally.

Local installation: `npm install <package-name>`

Global installation: `npm -g install <package-name>`

### Using package.json

To manage local packages we can use a `package.json` which allows us to list the packages our project depends on and their version.

The only two requirements are `name` and `version`. 

To generate this file we can use `npm init` or `npm init -y` for the default layout within the project's directory.

```node
{
  "name": "package_name",
  "description": "",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/username/package_name.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/username/package_name/issues"
  },
  "homepage": "https://github.com/username/package_name"
}
```
* name: the current directory name
* version: always 1.0.0
* description: info from the readme, or an empty string ""
* main: always index.js
* scripts: by default creates an empty test script
* keywords: empty
* author: empty
* license: ISC
* bugs: info from the current directory, if present
* homepage: info from the current directory, if present

We can also set more information through our CLI, such as `npm set init.license "MIT"`

### Dependencies

We can add two types of dependencies: 

* **dependencies**: Used in production.
* **devDependencies**: Used in development and test.

To install dependencies we can either add them directly to the `package.json` file with:

```node
"dependencies": {
    "prod_package": "^1.0.0"
  },
  "devDependencies" : {
    "dev_package": "^3.1.0"
  }
```

or we can use the CLI:

* **dependecies**: `npm install <package_name> --save`
* **devDependencies**: `npm install <package_name> --save`

Now to install the dependencies for a new project just run `npm install`. 

## Reference

\[1\] “Node.js v10.x Documentation.” Node.js, Node.js, nodejs.org/dist/latest-v10.x/docs/api.

\[2\] Npm, NPM, www.npmjs.com/.

\[3\] Weiss, Manuel. “The Absolute Beginner's Guide to Node.js.” Codeship, Codeship, 23 Jan. 2018, blog.codeship.com/node-js-tutorial/.