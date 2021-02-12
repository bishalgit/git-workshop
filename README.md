# Git Workshop
In this workshop we will focus on three sections:
1. How to write a commit message?
2. Rebasing your own work
3. Rebasing on the base branch

## How to write a commit message?
A good commit message is a way to communicate with your colleagues and future yourself.<br/>
Please follow [this article](https://wiki.bloopark.com/display/BPIT/How+to+Write+a+Git+Commit+Message) for more details.

## Rebasing your own work
 Rebase can be used to add/change/remove your commits directly from your own branch too! The “base” on which you rebase can be virtually any commit — even a direct ancestor.

In fact, if you wanted to see what was happening during the rebase we did, you could have used the “interactive mode” of rebase by adding the `-i` or `–interactive` argument. By doing so, Git will open your editor of choice (the one defined in your `EDITOR` environment variable) and list all of the commits that will be affected by the rebase operation, and what should be done with every single one of them. This is where the true power of `rebase` lies.

From your editor, Git lets you reorder, rename, or remove commits, but you can also split single commits into multiples, merge two or more commits together, or change their commit messages at the same time! Pretty much everything you’d like to do with your history is possible with `rebase`. And the awesome thing is that it’s relatively straightforward to tell Git what to do. Every commit is presented on its own line, in a sequential order, prefixed by the command that will get applied to it. Reordering commits is as easy as reordering lines, with the most recent commits at the bottom of the list. Removing commits is just a matter of removing the corresponding line, or specifying the `d` or `drop` command as a prefix. One of your messages contained a typo? Just use the `r` or `reword` command to keep the commit, but change the associated commit message.

To sum up, `rebase` is just a Git command that lets you:
- Select one or multiple sequential commits
- Base them on any commit of your repository
- Apply changes to this commit sequence as they are added on top of the new base commit

## Rebasing on the base branch
In September 2016, GitHub introduced [a new way to merge pull requests:](https://github.blog/2016-09-26-rebase-and-merge-pull-requests/) the “Rebase and merge” button. Also available for other repository managers such as GitLab, it’s the “rebase front door”. It lets you perform a single rebase operation of your Pull Request commits on top of your base branch and then perform a merge. It is very important to observe that those two operations are performed in order, and that the rebase is not a substitution of the merge. Hence rebase is not used to replace the merge, but it completes it.

You can simulate what happens when you click on the “Rebase and merge” (when there’s no conflict) by performing the following commands:

```bash
git checkout new-feature
git rebase main
git checkout main
git merge new-feature --ff
```

By doing so, you finally end up with a “linear history”:

![Linear Hisotry](https://blog-api.algolia.com/wp-content/uploads/2017/12/image4-720x136.png)

## Reference
- https://www.algolia.com/blog/engineering/master-git-rebase/