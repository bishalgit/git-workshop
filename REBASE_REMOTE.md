# Rebasing on the base branch
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

We’ve stepped through a lot of theoretical material, but as it turns out, the end result is relatively straightforward; here’s how to sync with changes happening on the remote:

```bash
git fetch
git checkout my-new-feat
git rebase origin/master
```

The first step retrieves the latest changes from the distant copy of `master` into your local `origin/master` branch. The second one checks out your feature branch. The last one  performs the `rebase` so that all your commits are now added on top of the latest changes that happened parallel to your own work. By applying those commands on our very first example, here is what would have happened:

![Remote Rebase](https://blog-api.algolia.com/wp-content/uploads/2017/12/image1.png)

As you can see, the feature branch now includes all the latest changes, so you’re able to work in sync with the rest of your team. By working with the above workflow, you’ll be able to deal with potential conflicts earlier and progressively instead of at the very last moment (when you want to merge your work within the base branch). People often disregard the “Rebase and merge” button because they expect too many conflicts at the very last step of the process (so they prefer to perform a regular merge commit instead). Ultimately, it takes a small active effort to stay in sync with the latest changes.
