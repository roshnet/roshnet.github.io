---
layout: post
title:  "Un-messing up Git problems"
comments: true
date:   2019-08-12 20:43:11 +0530
---

Using version control is relatively easy, but it's easy to get entangled when you go off the general routine.

So, I surmise some knowledge about how things work in version control systems.

So, below with good intentions, I mention some git pitfalls I've ran into, and what measures I've taken to solve them.
Of course there can be a lot more to this list, but this might come in handy.

<br>

***

<br>


## Problem 1
Need to remove last 'n' commits in a PR / committed additional changes and
sent a PR already

### _Solution_
Pretty long commit history? Follow along.

1. Switch to the branch from which the PR has been sent -
   `git checkout <PR_BRANCH>`. This brings you to the latest commit on that branch.

2. Let's say I have 10 commits in the PR, and I need to remove commits
   after commit 6. That means commits (7, 8, 9, 10) are wrong, and need to be
   removed.

3. I repeat, make sure you're on commit 10 (_i.e._ last commit) of the PR'd branch.

4. Do `git rebase -i HEAD~4`, and this should present before me the last `4` commits.
   Here, `4` should be the number of unwanted commits after the last good commit.

5. In the list of commits that opens up, add the `drop` word before the hash of the commit
   you need to remove. So, your text editor may look like - 

   ```
    drop 27a3c82 Commit message of a commit to be removed
    drop 09e1bb7 Another commit to be removed
    drop 17fe062 Yet another extra commit to be removed
    drop fd69808 This commit gotta go

    # Rebase 11b91a6..fd69808 onto 11b91a6 (4 commands)
   ```

6. Now, doing `git push -f origin <PR_BRANCH>` should update your PR by removing those commits
   whose hash was preceeded by the `drop` keyword.


## Problem 2
Made a typo in the last commit message

### _Solution_
That's an easy one, and also the most likely to happen. Try typing slow the next
time.
 
1. Check out the pushed branch.
2. Run `git commit --amend`, and fix the typo and save the file which opens up
   with your commit message on it.
3. Now, run `git push origin <branch> -f`, where `<branch>` is the branch you
   pushed the wrong commit to.
4. Verify from GitHub that the commit message has been changed.


## Problem 3
Badly messed up the master branch, and need to start over / master branch
having issues I don't bother to know, but want it reset anyway

### _Solution_
Before you hit the fork button again, give it a read below.

1. Checkout your master.
2. Checkout the first commit on it (the very first one).
3. Create and switch to a new temporary branch - `git checkout -b tmp`
4. Now, `git pull upstream master` to mirror upstream/master to your
   local `tmp`.
5. You don't need the messed up version of the master. Delete it by
   `git branch -D master`
5. Now, `tmp` is just like your master, except for it's name.
   Rename it as `git branch -m tmp master`
6. Further, run `git push origin master` to sync your origin/master with
   the upstream/master.


## Problem 4
Cannot push a repository I cloned with `--depth=x`

### _Solution_
Git/GitHub doesn't allow that. But that don't mean undoable.

1. `rf -rf .git`
2. `git init`
3. Write your own history now.


## Problem 5
Need to reset changes to just a file / don't want to stash everything just
to reset one file

### _Solution_
Using editors like VS Code does make it more GUI, but you may need
to do it via the command line.

The answer - `git checkout <file>`, when you need to reset all changes to that particular file.


## Problem 6
Need to stage only "some" changes (not all) in a particular file / don't wanna "add" the whole
file, rather "add" only some changes in it

### _Solution_
So you went with the flow of making changes, and later realised that you had to 
follow atomic commits.
The concept of interactive staging can prove handy here.

Use `git add --patch <file>`, and follow what shows up next.
There are many choices, and `y` and `n` alone did my job most of 
the times. But, you may also want to use `e` and `s` for more control.

`y` stages the visible change (hunk), and goes on to ask for the next change
(top-down fashion). When done `add`ing changes for the commit, go for `n`.

This basically allows you to stage only that specific change out of
all changes in `<file>`. When that has been done, it's time to commit
that small change with a nice little message.


## Problem 7
Stuck in a rebase? Trust me, it's the last thing you need to do to get pinned down.

### _Solution_
The magic lies in `git rebase -abort`.

You might already know this, but this is like getting a new life at times.
Saved my life, might as well save yours :)


<br>

# Notes

This isn't a great list, but hope to add more such random problems as they
come up.
<br>
<br>

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "https://roshnet.github.io/2019/08/12/unmess-git.html";
this.page.identifier = "unmess-git";
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://roshnet.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}