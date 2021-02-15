# Git Workshop
In this workshop we will focus on three sections:
1. How to write a commit message?
2. Rebasing your own work
3. Rebasing on the base branch

## How to write a commit message?
A good commit message is a way to communicate with your colleagues and future yourself.<br/>
Please follow [this article](https://wiki.bloopark.com/display/BPIT/How+to+Write+a+Git+Commit+Message) for more details.

skdjflsj
lskdjf


## Rebasing your own work
Rebase can be used to add/change/remove your commits directly from your own branch too! The “base” on which you rebase can be virtually any commit — even a direct ancestor.
- New point

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

## Rebasing on the base branch
In September 2016, GitHub introduced [a new way to merge pull requests:](https://github.blog/2016-09-26-rebase-and-merge-pull-requests/) the “Rebase and merge” button. Also available for other repository managers such as GitLab, it’s the “rebase front door”. It lets you perform a single rebase operation of your Pull Request commits on top of your base branch and then perform a merge. It is very important to observe that those two operations are performed in order, and that the rebase is not a substitution of the merge. Hence rebase is not used to replace the merge, but it completes it.

Consider the previous example. Before the final merge, we were in this situation:
![Local Rebase](https://blog-api.algolia.com/wp-content/uploads/2017/12/image2-720x239.png)

You can simulate what happens when you click on the “Rebase and merge” (when there’s no conflict) by performing the following commands:

```bash
git checkout new-feature
git rebase main
git checkout main
git merge new-feature --ff
```

By doing so, you finally end up with a “linear history”:

![Linear Hisotry](https://blog-api.algolia.com/wp-content/uploads/2017/12/image4-720x136.png)

As you see, rebasing is not a substitution for the merging step. As explained before, the two operations are not performed on the same branch: `rebase` is used on the feature branch whereas `merge` is performed of the base branch. For now, this operation just prevents having a single merge commit with all the changes in it, and it’s still a single operation that happens at the last step of your contribution (i.e., when you want to share your work).

Until now, we have only interacted with `master` as our base branch. To stay in sync with the changes of the base branch, it’s just a matter of performing the rebase step with the up-to-date base branch. The longer you wait to do this, the more out of sync you’ll be.

The up-to-date version of your base branch is hidden in plain sight. It’s a read-only version of the base branch, prefixed with the name of the remote to which you’re connected, or more simply put: it’s a read-only copy of the branch from your remote instance (such as GitHub or GitLab). The default prefix when you are cloning the repository for the first time is `origin`. More concretely, your `master` branch is the local version of master, whereas `origin/master` is the remote version of this branch, copied on your computer the last time you performed a `git fetch` operation.

lskdjflksj
lskdjlf
lkjldf

We’ve stepped through a lot of theoretical material, but as it turns out, the end result is relatively straightforward; here’s how to sync with changes happening on the remote:

```bash
git fetch
git checkout my-new-feat
git rebase origin/master
```

The first step retrieves the latest changes from the distant copy of `master` into your local `origin/master` branch. The second one checks out your feature branch. The last one  performs the `rebase` so that all your commits are now added on top of the latest changes that happened parallel to your own work. By applying those commands on our very first example, here is what would have happened:

![Remote Rebase](https://blog-api.algolia.com/wp-content/uploads/2017/12/image1.png)

As you can see, the feature branch now includes all the latest changes, so you’re able to work in sync with the rest of your team. By working with the above workflow, you’ll be able to deal with potential conflicts earlier and progressively instead of at the very last moment (when you want to merge your work within the base branch). People often disregard the “Rebase and merge” button because they expect too many conflicts at the very last step of the process (so they prefer to perform a regular merge commit instead). Ultimately, it takes a small active effort to stay in sync with the latest changes.

## Reference
- https://www.algolia.com/blog/engineering/master-git-rebase/