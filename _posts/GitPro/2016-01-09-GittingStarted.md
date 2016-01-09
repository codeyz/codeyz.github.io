---
title: Getting Started
layout: post
category : life
tag : life,诗
---
{% include JB/setup %}

###Git is *distributed*

Git is *distributed* version control system, which avoids the single of  failure. Every working directory fully mirrors the repository including every operation, while centralized version control system such SVN, only the latest version is copied when checking out.

###Every version is *snapshot*
Git is more like a miniature filesystem. Every time you commit, or save the state of your project, it takes a picture of all your files and stores a reference to the snapshot.

![Git is distributed]({{ site.url }}/assets/images/GitPro/snapshot.png)


###The three states
Git has three main states that your files can reside in:

1. Committed: data is safely stored in your local database.
2. Modified: data is changed but has not been committed to your local database.
3. Staged: you have marked the modified files in its current version to go into your next commit snapshot.

These three states lead to three main sections in Git:

1. Repository: it's where Git stores metadata and object database for your pproject.
2. Working directory: a single checkout of the project for you to use or modify.
3. Staging Area: it's where Git stores data which will go into your next commit.

The basic Git workload goes something like this:

1. You modify files in the working directory.
2. You stage the files, adding snapshot of them to the staging area.
3. You do a commit, which takes a snapshot of all your files in staging area and stores them to Git repository.

![Git is distributed]({{ site.url }}/assets/images/GitPro/3States.png)

###First-Time Git setup
####Configuration files
There are three location where Git stores its configuration:

1. `/etc/gitconfig`: Configuration for all users on the system. If you pass `--system` to `git config`, Git will read or write this file.
2. `~/.gitconfig` or `~/.config/git/config`: Configuration for the current on the system. You can pass `--global` to `git config` to make Git read or write this file.
3. `.git/config`: Specific to the current repository. Using `git config` without `--system` or `--global` will read or write this file.

The priority is `.git/config` > `~/.gitconfig` or `~/.config/git/config` > `/etc/gitconfig`. The higher overrides the lower.

###Your identify
Once after you install Git, you should set your user name and email address, which will be baked into your commits.

	git config [--global, --system] user.name "codeyz"
	git config [--global, --system] user.name "xubiao.codeyz@gmail.com"
			
You can use `git config --list` to check all the configuration or use `git config [user.name, user.email, core.editor,...]` to check specfic parameter. 
				
See more using `git help config`
