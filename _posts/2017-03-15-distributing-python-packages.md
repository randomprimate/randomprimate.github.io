---
layout: post
title: Distributing Python Packages
description: "Distribute your Python application as a package to be used publicly
or just by your team. Leverage Setuptools enhancements over `distutils` to
improve package management and maintenance."
headline: Distribute your Python package
modified: 2017-03-15
category: Python
tags: [package, distribution, python]
---

## Intro

Your application can now be distributed as a Python package. If you're working
on Open Source use PyPI to distribute it but if this is an internal or just
generally private package you'll need to setup a private package server or use
something like the service discussed here which is called [Gemfury](https://manage.fury.io).

While there are simpler methods of getting access to an application
such as cloning a repository they usually fall short when dealing with
dependencies, including required files and environment management.

You can find the source for this post [here](https://gitlab.com/balameb/fibocli).

This is a followup post from _Python CLI Application_.
If you haven't read it and you'd like to follow along then please clone the
project.

## Sections

1.[General Setup](#general-setup)  
2.[Private Package Repository](#private-package-repository)  
3.[Public Package Repository](#public-package-repository)  

## General Setup
Always version control your projects. Let's initialize this as a git project if
you haven't done so already, create a `.gitignore` file and commit our current
state.

{% highlight python linenos %}
git init
touch .gitignore
git add .
git commit -m "Initial state for fibocli app"
{% endhighlight %}


Because we're going to publish this package we'll need to have better
information in our setup script. Here's how it should look:

{% highlight python linenos %}
from setuptools import setup, find_packages

setup(
    name='fibocli',
    version='0.1.1',
    packages=find_packages(),
    include_package_data=True,
    py_modules=['fibocli'],
    install_requires=[
        'Click',
    ],
    entry_points='''
        [console_scripts]
        fibocli=fibocli:cli
    ''',
    author="<your-name>",
    author_email="<your-email>",
    description="<package-description>",
    license="<license>",
    keywords="<keyword-for-search>",
    url="<package-site>",
)

{% endhighlight %}

## Private Package Repository

If you haven't done so initialize your project as a git repository with `git init`.
Add and commit your code at least locally. We'll need git for the following steps.

- Create an account at [Gemfury](https://manage.fury.io)
- Add fury as remote
{% highlight python linenos %}
git remote add fury https://<username>@git.fury.io/<username>/<package-name>.git
{% endhighlight %}
- Push to Fury `git push fury master`. You'll need to authenticate.

You can now install packages with `pip` using something like:
{% highlight python linenos %}
pip install <package-name> --extra-index-url https://pypi.fury.io/<token>/<username>/
{% endhighlight %}

**Optional:** You can also enable Gemfury as a source by adding the following line to your
`requirements.txt`:

{% highlight python linenos %}
--extra-index-url https://pypi.fury.io/<token>/<username>/
{% endhighlight %}

_This instructions are from the official Getting Started Guide which
should be at or near your dashboard. Make sure you get the relevant link with
your account's token there._

## Public Package Repository
_The following steps have been taken from the official PyPI [guide](https://packaging.python.org/distributing/#uploading-your-project-to-pypi)._

- Install `twine` with `pip install twine`
- Create a Source Distribution with `python setup.py sdist`
- Create Wheels universally if compatible `python setup.py bdist_wheel --universal`
- Create an account at [PyPI](https://pypi.python.org/pypi?%3Aaction=register_form)
- Create a `.pypirc` file with `vi ~/.pypirc`
- Add the following information to your `.pypirc` file:

{% highlight python linenos %}
[distutils]
index-servers=pypi

[pypi]
repository = https://upload.pypi.org/legacy/
username = <username>
{% endhighlight %}

- Finally upload your package with `twine upload dist/*`
- There is a bug that will keep you from uploading a package you have removed
discussed [here](https://github.com/pypa/packaging-problems/issues/74)

You can now install this package through any other session with `pip install fibocli`

## Final Thoughts

We have managed to publish our package to both a public and private package
repository. This is very powerful as both methods are compatible with version
control and collaboration. If you are interested in better control over the
private package repository you can host your own with [mypypi](https://pypi.python.org/pypi/mypypi)
and there is also [s3pypi](https://github.com/novemberfiveco/s3pypi) to host
packages on S3 with an excellent article about setting it up [here](https://novemberfive.co/blog/opensource-pypi-package-repository-tutorial/).

## Helpful links
* [Packaging and Distributing Projects](https://packaging.python.org/distributing/)
* [Building and Distributing Packages with Setuptools¶](https://setuptools.readthedocs.io/en/latest/setuptools.html)
* [Choosing a license](http://docs.python-guide.org/en/latest/writing/license/)
