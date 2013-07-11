---
layout: post
title: "How To Use Git With SVN"
date: 2013-07-11 08:29
comments: true
categories: 
---
***My first three weeks at work have been a whirlwind of new information.*** And even though I am now fully able to contribute and build, I still have a running list of things to research and learn and fully comprehend. One of those things goes back to day one when I was cloning the repositories. "We use SVN repositories" I was told.

__So SVN and Git are the same thing, only different.__ SVN is an open source version control system, developed by Apache. Git is also an open source version control system, only developed by Linus Torvalds for the development of the Linux Kernel. While they have the same purpose, there are a few subtle differences in how the respositories work.

__A SVN repository doesn't have strict configuration requirements like a git repository.__ In fact, you can even have multiple projects in the same repository. Because git keeps track of a full history of all branches in a repository, multiple projects in one isn't the best idea.

__Another subtle difference is that the main branch in SVN is called `trunk`.__ You use the commands `svn copy` and `svn merge` to work on feature branches and merge them back to `trunk`. 

__At first I wondered if I needed to learn how SVN works.__ But then came the question, "Do you use SVN or git?" Once I replied that I use git, I was told I could still use it locally. Wonderful. I just needed to know the basic SVN commands to clone the repositories, and a few things about committing changes.

__So Git and SVN can work together. How?__ Git has a bridge to svn, using the command `git svn`. This command prefaces everything. 

__When I cloned my work repository, the command began with `git svn clone`,__ which clones my remote SVN repository into a local git repository. So from there I am in a local git environment. All my git commands for creating new branches, merging that branch to master, etc. all work! 

__So then the moment came when I finally contributed something to the code to commit. Now what?__<br>
1. I do my usual: `git commit -m "<my commit message>" <file_name_to_commit>` <br>
2. Then you have to commit with svn, so it's actually committed to the SVN repository: `git svn dcommit`

And really that's it. Easy as pie.


__Total side note,__ but I learned about `git stash` and `git stash apply` and wanted to share that tidbit. `git stash` allows you to stash away half-finished work, commit finished work, and then return to it later with `git stash apply`. You can read more about it <a href="http://git-scm.com/book/en/Git-Tools-Stashing">here</a>.

__Great Resources:__<br>
1. https://help.github.com/articles/what-are-the-differences-between-svn-and-git <br>
2. http://git-scm.com/book/ch8-1.html <br>
3. http://blogs.wandisco.com/2011/10/24/subversion-best-practices-repository-structure/