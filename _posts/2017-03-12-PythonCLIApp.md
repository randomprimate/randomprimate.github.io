---
layout: post
title: "Python CLI Application"       
description: "Develop a Python CLI application and leverage the functionality of
the Click Python package. After getting a working script we'll be bundling it
with setuptools so we can deploy it to a package repository which will be
covered in Distributing Python Packages."
headline: Develop a Python CLI App     
modified: 2017-03-12                 
category: Python
tags: [cli, python]
---

## Intro

A CLI application is a command line utility to be used through a text interface
such as [grep](https://linux.die.net/man/1/grep) or
[cURL](https://curl.haxx.se/docs/manpage.html). In this article we'll be
building our own CLI application with Python and the [Click](http://click.pocoo.org/6/)
package. We'll also dig into multiple commands and options. You can then learn
how to deploy it to a package repository by reading _Distributing Python Packages_.

You can find the source for this post [here](https://gitlab.com/balameb/fibocli).
To get the relevant code in GitLab go into `repository` and on the branch
dropdown choose the tag corresponding to this post's name.

### Content
1. [Create a simple Python script](#creating-the-script)
2. [Add Click](#adding-the-click-package)
3. [Bundle with Setuptools](#bundle-with-setuptools)
4. [Multiple Commands](#multiple-commands)
5. [Add Arguments](#add-arguments)

## Creating the Script

Let's start with a simple script that displays the Fibonacci sequence up to the
'n' amount. Here's the equation that we'll be using for our script:

**Fibonacci Sequence**

$$\begin{aligned}
F_n = F_{n-1} + F_{n-2}
\end{aligned}$$

What we need next is a directory for our project and within this directory
create a `fibocli.py` file.

{% highlight python linenos %}
mkdir fibocli
cd fibocli
touch fibocli.py
{% endhighlight %}  

I like using [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
to maintain dependencies so let's also configure an environment.

{% highlight python linenos %}
mkvirtualenv fibocli
touch requirements.txt
{% endhighlight %}  

In the `fibocli.py` paste the following code and try it out. At the function call
under the `__name__` block the integer parameter is the number of iterations
that the sequence should run.

{% highlight python linenos %}
def fibo(num):
    """Fibonacci sequence"""
    n = int(num)
    l = [0, 1]
    if n == 0:
        result = l[0]
    elif n == 1:
        result = l
    a = 0
    b = 1
    for i in range(0, n-1):
        b += a
        a = b - a
        l.append(b)
    result = l

    print('result')

if __name__ == '__main__':
    fibo(10)

# Credits for the fibo function: http://stackoverflow.com/questions/494594/how-to-write-the-fibonacci-sequence-in-python
{% endhighlight %}

## Adding the Click package

To add command line behavior to our script we'll use the [Click](http://click.pocoo.org/6/) package.
Here's a quick description from the official site:

>Click is a Python package for creating beautiful command line interfaces in a composable way with as little code as necessary. It’s the “Command Line Interface Creation Kit”.

Install `Click` with `pip` and freeze update the requirements file.

{% highlight python linenos %}
pip install click
pip freeze > requirements.txt
{% endhighlight %}

Now in our script we need to import `click` and add some decorators. Click is
based on declaring commands through decorators.

{% highlight python linenos %}
import click

@click.command()
@click.option('--num', default=10, prompt='Sequence iterations.')
...
  # Substitute print('result') with:
  click.echo('result')
...
{% endhighlight %}

Just by adding `@click.command()` we are converting a function into a Click
command. The `@click.option` decorator will enable parameters which are going to
be used as flags such as `--verbose`. The option our script has uses a default
and also a prompt which will ask you for the number of iterations and fallback to
10 if there is no input; no need to keep the parameter on our main function call
by the way.

Another useful change we added to our script is the use of `click.echo` instead
of `print` which acts as the former but is compatible with both Python 2 and 3.

Now run the script with `python fibocli.py` and you'll get a prompt to add the
number of iterations for the Fibonacci sequence. You can also use the `--num`
flag to skip the prompt as well as the `--help` flag to view some documentation.

## Bundle with Setuptools

Using setuptools allows us to make our application distributable. Here's a
few reasons from the official docs:

* Setuptools automatically generates executable wrappers for Windows so your
command line utilities work on Windows too.

* Setuptools scripts work with virtualenv on Unix without the virtualenv having
to be activated. This is a very useful concept which allows you to bundle your
scripts with all requirements into a virtualenv.

Packaging also allows us to call on the application through the application's
name, no need of calling python or using the script's path.

To get started add a `setup.py` file in the same directory and include the
following code:

{% highlight python linenos %}
from setuptools import setup

setup(
    name='fibocli',
    version='0.1',
    py_modules=['fibocli'],
    install_requires=[
        'Click',
    ],
    entry_points='''
        [console_scripts]
        fibocli=fibocli:fibo
    ''',
)
{% endhighlight %}

You can change the name, version and module name for your current release but
the really important section is under `entry_points`. On the left side of the
equals sign (=) we have the name of the script that should be generated, the
right side is the import path followed by a colon (:) with the Click command.

### Testing our script

Let's make sure this is working as expected. All we need is to install our package.

{% highlight python linenos %}
 pip install --editable .
{% endhighlight %}

Now let's try our application with no flags to get the prompt, later with a
prompt and lastly with a `help` flag to view documentation.

{% highlight python linenos %}
fibocli
fibocli --num=32
fibocli --help
{% endhighlight %}

## Multiple Commands

To use multiple commands we need to attach them to a `group`. This is a simple
task which requires a central function (group) and explicitly adding them to
that group through a `add_command` method.

To make this happen we will create a new function called cli which will be the
group and we'll also add a hello world function to test this out.

{% highlight python linenos %}
@click.group()
def cli():
    pass

...
@click.command()
def hello():
    """Say Hi"""
    click.echo('hello world')


cli.add_command(fibo)
cli.add_command(hello)
...
{% endhighlight %}

Let's try this out. First we'll call the `fibocli` command to view the suggested
format. Then we will call it again with each command we have in our application

{% highlight python linenos %}
# Get usage suggestion through docs
fibocli
# Run the fibo command
fibocli fibo --num=25
# Run the hello command
fibocli hello
{% endhighlight %}

## Add Arguments

The other feature that we'll test out is adding arguments to our commands. An
important thing to note is that Click will not add documentation so you are
expected to do so manually. We'll change the hello world command slightly so
that we can add an argument to it and additionally we will also change the
directory structure to include commands from another script which is much closer
to what you'll be building.

Let's start by adding a `__init__.py` file to the root of our project which will
let Python treat the directory as containing packages. Also include a
`scripts` directory with another `__init__.py` file inside of it. We'll also
create a `hello_world.py` script under the `scripts` directory to test importing
this script.

{% highlight bash%}
├── __init__.py
├── fibocli.py
├── requirements.txt
├── scripts
│   ├── __init__.py
│   └── hello_world.py
└── setup.py
{% endhighlight %}

Now let's remove the `hello()` function with it's decorator from the `fibocli`
script and in `hello_world` we can add something like this:

{% highlight python linenos %}
import click


@click.command()
@click.option('--count', default=1, help='number of greetings')
@click.argument('name', nargs=-1)
def hello(count, name):
    """
    Name: Hello\n
    Description: Say Hi\n
    Arguments:\n
    - NAME:\n
      - Everyone you want to greet.\n
      - Takes multiple strings, i.e. "Jane" "John"
    """
    for n in name:
        for x in range(count):
            click.echo('Hello %s!' % n)
{% endhighlight %}

Notice the new `@click.argument` decorator. This will take a `name` argument for
the `hello` command, it will actually take unlimited arguments thanks to the
`nargs` parameter.

Before testing the script we need to import it and modify how we are adding it
in the `fibocli` file.

{% highlight python linenos %}
import scripts.hello_world as hel
...
# Substitute cli.add_command(hello) with:
cli.add_command(hel.hello)
{% endhighlight %}

You are now ready to test the final script, don't forget to try out the multiple
argument option for our `hello` command.

{% highlight python linenos %}
fibocli hello "Jane" "Joe" "John" --count=3
{% endhighlight %}

## Final Thoughts
We've gone from a simple script to a CLI application. We are now using multiple
commands and parameters. We can also work with documentation and bundle our
script with Setuptools, our next step is _distribution_.
I started digging into this topic due to some heavy use of Python scripts in
security and monitoring which was getting out of control, one of the biggest
advantages was centralizing and scoping functionality and development in one
tool alone but there are plenty other benefits from using CLI applications such
as [nested and scoped commands](http://click.pocoo.org/5/commands/), better
[error handling](http://click.pocoo.org/5/exceptions/) and some really cool
[utilities](http://click.pocoo.org/5/utils/). Make sure to visit the docs to
get the most out of this package.
