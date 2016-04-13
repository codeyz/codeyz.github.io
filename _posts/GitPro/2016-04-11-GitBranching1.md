---
title: Git Branching(1) Branch Introduction
layout: post
category : Git
tag : [git, merge, branch, conflicts]
---
{% include JB/setup %}

<h3>What is a branch</h3>

After making a commit, Git stores a `commit object` which contains a pointer to the snapshot of the contents you staged. The `commit object` also contains user's name, email, pointers to its parent or parents(what this commit came). Following figure dipicts this.

![commits and their parents]({{ site.url }}/assets/images/GitPro/commitAndparents.png)

A branch is actually a pointer to a commit which is shown in following figure. `master` is a default branch in Git. A special `HEAD` pointer records which branch you are currently on.

![branch and commits]({{ site.url }}/assets/images/GitPro/branchAndcommits.png)


<h3>Creating a New Branch</h3>

This command can be used to create a new branch while `checkout` will switch to another branch(The `HEAD` pointer will also point to the new one). When creating a new branch, a new pointer to <span style = "color: #f22430">the same commit</span> you're currently on will be created.

<pre class="prettyprint lang-sh">
$ git branch [branch-name]
$ git checkout [branch-name]
$ git checkout -b [branch-name] #create a new branch and switch to it
</pre>

![new branch]({{ site.url }}/assets/images/GitPro/newBranch.png)

<h3>Merging Branches Workflow</h3>

<pre class="prettyprint lang-sh">
$ git merge [branch-name]
</pre>

<h4>Condition 0: fast-forward</h4>

If the branch you merged in is directly upstream of commits you're on, Git simply moves pointer forward, called fast-forward.

![Condition 0: directly upstream]({{ site.url }}/assets/images/GitPro/mergeCondition0.png)

<h4>Condition 1: three-way</h4>

If the branch you merged in isn't directly upstream of commits you're on, Git does a simple three-way merge, using two snapshots pointed to by the branch tips and the best common ancestor which is used for merge base. Instead of fast-forward, at this point, Git creates a new snapshot, called merge commit.

![Condition 0: not directly upstream]({{ site.url }}/assets/images/GitPro/mergeCondition1.png)

<h3>Branch Management</h3>

<pre class="prettyprint lang-sh">
$ git branch # show branches
$ git branch -v # show last commit on each branch
$ git branch --merged # list branches that you have merged into the branch you're currently on
$ git branch --no-merged # list branches that you have not yet merged into the branch you're currently on
$ git branch -d [branch-name] # delete a branch
</pre>
