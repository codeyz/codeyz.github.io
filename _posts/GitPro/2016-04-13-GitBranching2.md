---
title: Git Branching(2) Remote Branches
layout: post
category : Git
tag : [remote, branch]
---
{% include JB/setup %}


<h3>Remote references</h3>

>Remote references are references(pointers) in your remote repositories, including branches, tags, and so on.

<pre class="prettyprint lang-sh">
$ git ls-remote [remote] # list remote references
$ git ls-remote origin
From https://github.com/codeyz/GitPractice.git
31b32f7ab45ecad20602b495ad8f83e9d63fefaf	HEAD
31b32f7ab45ecad20602b495ad8f83e9d63fefaf	refs/heads/master
f1c84fba9029937874427033113485078b9a5f0e	refs/tags/v0.1
</pre>

<h3>Remote-tracking branches</h3>

> Remote-tracking branches are references(pointers) to the state of remote repositories. They act as bookmarks to remind you where the branches in your remote repositories were the last time you connected them. They are local references you can't move.Remote-tracking branches take the form (remote)/(branch). 

When you clone repository to local, Git automatically creates a pointer to where its master branch is and names it `origin/master`. And Git also creates a master branch as your own local branch. This process is figured below.

> <span style = "color: #f22430">origin</span> is the default name for a remote like master is the default branch name.

![clone from server]({{ site.url }}/assets/images/GitPro/serverLocal.png)

If your coworkers update remote repositories and you also update local commit, the state may like below.

![diverge of server and local]({{ site.url }}/assets/images/GitPro/divergeOfServerLocal.png)

To sync work, you can use `fetch` command to fetches data from a server. and it also will move the `origin/master` reference to its new position.

<pre class="prettyprint lang-sh">
$ git fetch origin
</pre>

![fetch]({{ site.url }}/assets/images/GitPro/fetch.png)

Suppose now there an another serve named codeyz as a remote, and when fetch from codeyz, `codeyz/master` will be created.

![fetch another remote]({{ site.url }}/assets/images/GitPro/fetchAnother.png)

> When do a fetch to bring down new remote-tracking branches, you don't automatically have local, editable copies of them. In other words, for example, if in the remote there is a branch named test, after fetched to local, you have not a new branch test, you have only an origin/test pointer that you can't modify.

You can merge the test branch into your current working branch using 

<pre class="prettyprint lang-sh">
$ git merge origin/test
</pre>

Or if you want to have a copy, you can `checkout` it

<pre class="prettyprint lang-sh">
$ git checkout -b test origin/test
</pre>


<h3>Tracking branches</h3>

> When checking out a local branch from a remote-tracking branch, this local branch is called tracking branch. Tracking branches have their specific relationship to remote-tracking branches. If you `git pull` on a tracking branch, The Git automatically knows which server to fetch from and branch to merge into.

<pre class="prettyprint lang-sh">
# to checkout a local branch from a remote-tracking branch
$ git checkout -b [branch] [remote/branch]
$ git --track [branch] [remote/branch]
</pre>

If you have a local branch already and want to set it to a remote branch, or want to change the upstream branch you're tracking, next command will be useful.

<pre class="prettyprint lang-sh">
# set or change the upstream branch
$ git branch -u [remote/branch]
</pre>

To list local branches with more information and what each branch is tracking, using option `-vv`

<pre>
# list local branches with more information and what each branch is tracking
$ git branch -vv
</pre>

Note that these information are only since the last time you fetched from servers.

<h4>Delete remote branches</h4>

Run the following to delete a remote branch

<pre>
$ #git push [remote] --delete [branch]
$ git push origin --delete test
</pre>
