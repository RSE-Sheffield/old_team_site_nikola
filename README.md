# RSE-Sheffield.github.io

[![Travis status](https://travis-ci.org/RSE-Sheffield/RSE-Sheffield.github.io.svg?branch=devel)](https://travis-ci.org/RSE-Sheffield/RSE-Sheffield.github.io)

Source code for the Research Software Engineering @ Sheffield website. Built using [Nikola (https://getnikola.com/)](https://getnikola.com/).
The source for this site is maintained on the `devel` branch and 
rendered content is deployed to the `master` branch.

Read on to learn how you can contribute to this site and about how the site is built, tested and deployed.

## Installation on macOS / Linux using conda

First clone this repo and `cd` into its directory.

[Install conda](https://conda.io/docs/user-guide/install/index.html)
(we recommending doing a regular install of Miniconda).
Then:

```bash
conda create --name rse-blog python=3.6
source activate rse-blog
pip install --upgrade -r "requirements.txt"
```

Note that the `python=3.6` above will work even if
you don't think you have Python 3 installed.
`conda` creates an environment and installs Python 3 into it
from its own packages.

On macOS you [may also need to](http://stackoverflow.com/questions/23172384/lxml-runtime-error-reason-incompatible-library-version-etree-so-requires-vers<Paste>) add the following to your `~/.bashrc`:

```bash
export DYLD_LIBRARY_PATH=${HOME}/anaconda/envs/nikola/lib/
```

## Installation on macOS / Linux using venv

An alternative approach is to forgo using conda and install into a [venv](https://docs.python.org/3/library/venv.html) (a different type of Python virtual environment).

First clone this repo and `cd` into its directory.

```bash
sudo apt-get install python3-venv  # or the macOS equivalent
# Create a directory to house your venvs
mkdir ~/.venvs
# Create the venv
python3.6 -m venv ~/.venvs/rse-blog
# Activate the venv
source ~/.venvs/rse-blog/bin/activate
# Install dependencies via pip, including wheel and nikola
pip install --upgrade -r "requirements.txt"
```

## Writing a blog post

Install Nikola
(the above installation will have installed Nikola into
either a conda or a virtualenv environment).

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

## Formatting tips

* When editing a Markdown page make sure you leave a blank line before any bulleted lists.

## Site testing and deployment

When you create a pull request against this repo
or push to a branch in this repo, travis-ci.org:

 1. Checks that the site can be built
 2. Then, if the build was triggered by a push to devel, deploys the rendered content to the master branch.  
    NB Travis CI deployment tasks [never run for builds triggered by pull requests](https://docs.travis-ci.com/user/deployment#pull-requests).

See `.travis.yml` for more info and see [this guide](https://medium.com/@bezgachev/6-simple-steps-to-automatically-test-and-deploy-your-javascript-app-to-github-pages-c4c32a34bcb1) 
for how to set up Travis-driven deployment.
