[TOC]

# Generality
## Repository
### Bare vs Non-Bare Repository

> **References**
> 1. [What is a bare Git repository?](http://www.saintsjd.com/2011/01/what-is-a-bare-git-repository/ "What is a bare Git repository?")


# Initialize a Repository
## Initialize from an Existing Directory
```Shell
cd <localdir>
git init
git add .
git commit -m 'message'
git remote add origin <url> (e.g. https://github.com/sonhuytran/...)
git push -u origin master
```