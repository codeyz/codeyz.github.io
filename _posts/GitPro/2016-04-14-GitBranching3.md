---
title: Git Branching(3) Rebasing
layout: post
category : Git
tag : [branch, rebase, merge]
---
{% include JB/setup %}

<h3>The basic rebase</h3>

There are two main ways to integrate changes from one branch to another, the `merge` and the `rebase`.

Please get more detail about merge in:[git merge workload]({% post_url 2016-04-11-GitBranching1 %})

> In Git, rebasing is to take all the changes that were committed on a branch and replay then on another branch.

You'll running the following:
<pre class="prettyprint lang-sh">
$ git checkout test
$ git rebase master # rebase branch test onto master
</pre>

Git will do following work:

1. going to the common ancestor of two branches.
2. gitting the diff introcuded by each commit of the branch you're on.
3. saving those diffs to temporary files.
4. resetting the current branch you're on to the same branch you are rebasing onto.
5. applying each change in turn.

The following graph dipicts the result. After rebasing, the commit *C4* and *C5* are deleted on branch test, and another two commits *C4'* and *C5'* are created on the top of master branch. And you can fast-forward master to branch test.

![rebase]({{ site.url }}/assets/images/GitPro/rebase.png)

Supposing you have checked out a new branch named test2 from test and made some commits on it. The state is like below. Commit *C4* is the common commit of branch test and test2. Now you want to rebase test2 onto master, but hold off changes on branch test. You can take changes only on branch test2(*C6* and *C7*) and replay them on master using option `--onto`.

![rebase common commit]({{ site.url }}/assets/images/GitPro/rebaseCommonCommit.png)

<pre class="prettyprint lang-sh">
$ git rebase --onto master test test2
</pre>

The command says, "check out branch test2 and ignore patches from common ancestors of test and test2 and replay them on master. Then the result will become the following.

![result of rebase common commit]({{ site.url }}/assets/images/GitPro/rebaseCommonCommitResult.png)
