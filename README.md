# RSE-Sheffield.github.io
Source code for the Research Software Engineering @ Sheffield website. Built using Nikola https://getnikola.com/

Contributors to this site need to make Pull Requests to this branch (devel). 

The site is deployed from the master branch using `nikola github_deploy` -- an operation performed by the owners of the site.

## Installation on Mac OS X
I installed Nikola on OS X as follows

```
conda create -n nikola python=3.5
source activate nikola
pip install --upgrade "Nikola[extras]"
export DYLD_LIBRARY_PATH=/Users/walkingrandomly/anaconda/envs/nikola/lib/
```
The last line will need to be added to `.bash_profile`

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

```
nikola build
```

If the build is OK, you can look at it on your browser with 

```
nikola serve --browser
```
When you are happy, submit a Pull Request.

## For site admins

To publish a post, accept the PR and do

```
nikola githb_deploy
```

