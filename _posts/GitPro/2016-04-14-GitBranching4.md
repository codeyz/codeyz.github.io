---
title: Git Branching(4) Perils of rebasing
layout: post
category : Git
tag : [branch, rebase, merge]
---
{% include JB/setup %}

<h3>The perils of rebasing</h3>
You should follow the guideline that

> Do not rebase commits that exist outside your repository. That is, do not rebase commits that you have pushed.

When you rebase commits, you're abandoning existing commits and creating new ones that are similar but different. If you push commits somewhere and other people pull them down and work on them, and now you rewrite those commits with `git rebase` and push them again, your coworkers have to re-merge their work.

Take an example as below.

![rebase commits outside local repository]({{ site.url }}/assets/images/GitPro/rebasePeril.png)

At this point, when you run `git log` you will find two commits(*C4*, *C4'*) have the same author, date and message. And if you push them to the remote, you'll reintroduce all the rebased commits, which can futher confuse people.

<h3>Resolution: rebase again</h3>

If you find this situation, instead of merge, you can do a rebase using either of the following two commands

<pre class="prettyprint lang-sh">
$ git rebase origin/master
$ git pull --rebase
</pre>

Git will:

1. Determine what work is unique to our branch. (C2, C3, C4, C6, C7)
2. Determine which are not merge commits. (C2, C3, C4)
3. Determine which have not been rewritten into the target branch. (just C2 and C3, since C4 is the same patch as C4')
4. Apply those commits to the top of origin/master.

The state will end up with the following.

![rebase the origin/master]({{ site.url }}/assets/images/GitPro/rebaseRemote.png)
