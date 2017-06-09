# RSE-Sheffield.github.io

[![Travis status](https://travis-ci.org/RSE-Sheffield/RSE-Sheffield.github.io.svg?branch=devel)](https://travis-ci.org/RSE-Sheffield/RSE-Sheffield.github.io)

Source code for the Research Software Engineering @ Sheffield website. Built using [Nikola (https://getnikola.com/)](https://getnikola.com/).

Contributors to this site need to make Pull Requests to this branch (`devel`). 

The site is deployed from the master branch using `nikola github_deploy`,  an operation performed by the owners of the site.

## Installation on macOS / Linux using conda

First install conda; then:

```bash
conda create -n nikola python=3.6
source activate nikola
pip install --upgrade 'Nikola[extras]'
```

On macOS you [may also need to](http://stackoverflow.com/questions/23172384/lxml-runtime-error-reason-incompatible-library-version-etree-so-requires-vers<Paste>) add the following to your `~/.bashrc`:

```bash
export DYLD_LIBRARY_PATH=${HOME}/anaconda/envs/nikola/lib/
```

## Installation on macOS / Linux using venv

An alternative approach is to forgo using conda and install into a [venv](https://docs.python.org/3/library/venv.html) (a different type of Python virtual environment).

```bash
sudo apt-get install python3-venv  # or the macOS equivalent
# Create a directory to house your venvs
mkdir ~/.venvs
# Create the venv
python3.6 -m venv ~/.venvs/rse-blog
# Activate the venv
source ~/.venvs/rse-blog/bin/activate
# Install the wheel package, needed to install the (binary) wheel versions of
# various other packages (often far quicker than installing from source)
pip install wheel
# Install nikola static site generator package plus extras
pip install 'Nikola[extras]'
```

## Writing a blog post

Install Nikola

Fork this repo, clone it to your machine and enter the directory.

Create a file in the **posts** directory. It will need to start with a header like this: 

```
<!--
.. title: Accelerated versions of R for Iceberg
.. author: Mike Croucher
.. slug: intel-R-iceberg
.. date: 2016-09-12 00:31:35 UTC
.. tags:
.. category:
.. link:
.. description:
.. type: text
-->
```

The `slug` refers to the end of the URL and it will need to be unique.

You can build and test your post on your own machine using:

```bash
cd site
nikola build
```

If the build is OK, you can look at it on your browser with 

```bash
nikola serve --browser
```

As an alternative to repeatedly running `nikola build` every time you make a change you can run:

```bash
nikola auto
```

to detect changes, automatically rebuild your site and refresh your browser.

When you are happy, submit a Pull Request.

## For site admins

To publish a post, accept the PR then from the `site` directory you can run a single command to:

* commit a rendered version of the site to the `master` branch then 
* push this to the `origin` (_not_ `upstream`) branch on GitHub

this command being:

```bash
nikola github_deploy
```
