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
