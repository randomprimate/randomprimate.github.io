---
layout: post
comments: false
title: Node.js Introduction
date: 2017-06-21 19:52:57 +0000
description: 'Understand what Node.js is and the basic concepts behind it. Set up and dependency management.'
categories:
- nodejs
tags:
- nodejs
- introduction
headline: Getting started with Nodejs
modified: ''
---
## Intro

Nodejs is simply a JavaScript runtime which means that it's a way to run JavaScript on whatever platform nodejs has been installed.  

An important concept with node is the `non-blocking I/O` part of the official description. This basically means that one function call will not block or be blocked by another function call. Given that JavaScript uses a single thread in its event loop we can't count on using multiple threads. Now the way it handles this is by using asynchronous callbacks, you can think of this as a callback queue and you can think of callbacks as a function that runs after another function has completed execution.

Now the rest of this article will walk you through installing node, trying it out with a simple web server and lastly learn how to manage dependencies and installing packages. By the end, you should have the foundation to learn any specific framework or tool based on nodejs.

## Install

Node Version Manager ([NVM](https://github.com/creationix/nvm)) is the best way of having node installed in your system due to different projects using different node versions.

Install NVM:

`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`

Install node with `nvm install 8.11.2` which installs version `8.11.2`, you can later check installed versions with `nvm ls`.

You can also download the latest package from the [official site](https://nodejs.org/en/download/) which has a package for Linux, Mac, and Windows.

For any hiccup during installation think about dependencies first, look up your platform's requirements. For Linux, for example, you might find [this](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-16-04) article useful.


## Node CLI

Usage: `node [options] [v8 options] [script.js | -e "script"] [arguments]`

To execute a js script use: `node <script-name>`

You can also run the debugger with `node --inspect <script-name>`. This allows us to add `debugger` in our script which will add a breaking point into a running session. To actually activate this tho we need to open dev tools at:

    chrome-devtools://devtools/bundled/inspector.html?experiments=true&v8only=true&ws=127.0.0.1:9229/5e2abd65-4ce1-4fda-b9b7-c3cc7f04df67

Where `5e2abd65-4ce1-4fda-b9b7-c3cc7f04df67` is generated when launching the session.

Finally, we can also open a  REPL (read-eval-print-loop) where you can execute raw JavaScript code by running `node`.

{% highlight bash linenos %}
$ node
> const anarr = [1, 3, 5, 7]
undefined
> anarr
[ 1, 3, 5, 7 ]
> anarr.push(2, 4, 6)
7
> anarr
[ 1, 3, 5, 7, 2, 4, 6 ]
{% endhighlight %}

### Web Server

This example is taken from the `nodejs` documentation. We are going to build a web server relying on Node's `HTTP` module.

Create the following file and run it with the node cli: `node web-server.js`

{% highlight javascript linenos %}
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
{% endhighlight %}

The result should be the following console message:

{% highlight bash linenos %}
Server running at http://127.0.0.1:3000/
{% endhighlight %}

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

{% highlight json linenos %}
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
{% endhighlight %}

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

{% highlight json linenos %}
"dependencies": {
    "prod_package": "^1.0.0"
  },
  "devDependencies" : {
    "dev_package": "^3.1.0"
  }
{% endhighlight %}

or we can use the CLI:

* **dependecies**: `npm install <package_name> --save`
* **devDependencies**: `npm install <package_name> --save-dev`

Now to install the dependencies for a new project just run `npm install`.

## Final Thoughts

With this info you should be better equipped to either build with Node.js or
use some of the awesome tools which are available to you. Here are a few links
to some projects I recommend looking into:
- For web development try [Sails.js](https://sailsjs.com/)
- Another popular framework is [Mean.js](http://mean.io/)
- Hardware, robotics and IoT are available through [Johnny5](http://johnny-five.io/)
- Web sockets for real time data is accessible through [Socket.io](https://socket.io/)


## Reference

[1] Bearnes, Brennen. “How To Install Node.js on Ubuntu 16.04 DigitalOcean.” 5 Common Server Setups For Your Web Application DigitalOcean, DigitalOcean, 7 Mar. 2018, www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-16-04.

[2] Creationix. “Creationix/Nvm.” GitHub, github.com/creationix/nvm.

[3] Morelli, Brandon. “JavaScript: What the Heck Is a Callback? – Codeburst.” Codeburst, Codeburst, 12 June 2017, codeburst.io/javascript-what-the-heck-is-a-callback-aba4da2deced.

[4] “Node.js v10.x Documentation.” Node.js, Node.js, nodejs.org/dist/latest-v10.x/docs/api.

[5] Npm, NPM, www.npmjs.com/.

[6] Patel, Priyesh. “What Exactly Is Node.js? – FreeCodeCamp.” FreeCodeCamp, FreeCodeCamp, 18 Apr. 2018, medium.freecodecamp.org/what-exactly-is-node-js-ae36e97449f5.

[7] Weiss, Manuel. “The Absolute Beginner's Guide to Node.js.” Codeship, Codeship, 23 Jan. 2018, blog.codeship.com/node-js-tutorial/.
