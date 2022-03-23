# Rebasing your own work
Rebase can be used to add/change/remove your commits directly from your own branch too! The “base” on which you rebase can be virtually any commit — even a direct ancestor.

In fact, if you wanted to see what was happening during the rebase we did, you could have used the “interactive mode” of rebase by adding the `-i` or `–interactive` argument. By doing so, Git will open your editor of choice (the one defined in your `EDITOR` environment variable) and list all of the commits that will be affected by the rebase operation, and what should be done with every single one of them. This is where the true power of `rebase` lies.

From your editor, Git lets you reorder, rename, or remove commits, but you can also split single commits into multiples, merge two or more commits together, or change their commit messages at the same time! Pretty much everything you’d like to do with your history is possible with `rebase`. And the awesome thing is that it’s relatively straightforward to tell Git what to do. Every commit is presented on its own line, in a sequential order, prefixed by the command that will get applied to it. Reordering commits is as easy as reordering lines, with the most recent commits at the bottom of the list. Removing commits is just a matter of removing the corresponding line, or specifying the `d` or `drop` command as a prefix. One of your messages contained a typo? Just use the `r` or `reword` command to keep the commit, but change the associated commit message.

To sum up, `rebase` is just a Git command that lets you:
- Select one or multiple sequential commits
- Base them on any commit of your repository
- Apply changes to this commit sequence as they are added on top of the new base commit

To better illustrate this, consider the following series of commits:

```bash
$ git --no-pager log --oneline
57f15b4 (HEAD -> master) add D and E files
61681da add B file
7d4a28d add C file
f92bb1d add A
78b3f67 root commit
```

Note that, except for our base commit, all commit hashes have changed. This is due to the way Git generates those commit hashes, which are not only based on the changes themselves, but also on the parent commit hash and other metadata.

Anyway, let’s rebase!

Let’s start with a `git rebase -i HEAD~4`. This tells Git to interactively rebase the last 4 commits from HEAD inclusive. `HEAD~4` points to the “root commit” which is the commit upon which we will rebase. After hitting ENTER, your editor of choice will open (or `vi` by default on Unix-style systems). Here, Git is simply asking you what you want to do with the commit you performed.

```bash
pick f92bb1d add A
pick 7d4a28d add C file
pick 61681da add B file
pick 57f15b4 add D and E files

# Rebase 78b3f67..57f15b4 onto 78b3f67 (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

As explained earlier, every line represents a single commit, prefixed by the corresponding rebase command that will get applied. All the commented lines are ignored during the rebase and are here to remind you of what to do now. In our case, we will go with the following commands:

```bash
reword f92bb1d add A
pick 61681da add B file
pick 7d4a28d add C file
edit 57f15b4 add D and E files
```

Here, we told Git to perform three tasks for us during the rebase:

- Stop at the first commit to let us change the commit message
- Reorder the second and third commit to have them in the correct order
- Stop at the last commit to let us do some manual amending
