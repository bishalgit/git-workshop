# Best Practices
Some of the best practices are disccussed below. These are not to enforce but to provide guidelines for your workflow.<br/>

Table of Contents:
- [Good Naming Convention for branches](#good-naming-convention-for-branches)
- [Atomic Commits](#atomic-commits)
- [References](#references)

## Good workflow for branches
Use fix, hotfix, feature, testing, improvement as prefix to your branch. It helps PMs and developers to easily spot in testing environments.
Examples:
```bash
git checkout -b feature/RBA-999-allow-public-user-to-add-comments
git push -u origin feature/RBA-999-allow-public-user-to-add-comments
```
Here in the example we also use `git push -u origin <branch_name>` so that upstream is updated. Next time you can only use `git push` to push your changes.

## Atomic Commits
It is a habit not a job.<br/>

Bigger commits can:
- Obfuscate the source of bugs or regressions in the future.
- Make it difficult to revert undesired changes without also reverting desired ones.
- Make larger tickets more overwhelming and difficult to manage.

### Atomic Approach
- Commit each fix or task as a separate change
- Only commit when a block of work is complete
- Commit each layout change separately
- Joint commit for layout file, code behind file, and additional resources

### Atomic Commits and the Single Responsibility Principle
You should be able to describe your changes with a single, meaningful message without trying to tack on additional information about some unrelated work that you did.

### There's No Such Thing as Too Many Commits
Many enterprises have huge number of commits. It is not about number: it is about you able to clearly differentiate between commits x-1, x, and x+1.

### The Benefits of Writing Atomic Commits in Git
1. Easy to track down regressions
2. Easy to revert, drop, and amend
3. Easy to complete larger tasks
4. Easy to merge features to other branches

## Habit of rebase from remote
Rebase at least once a day from the base branch. This habit has following benefits:
- Avoid conflicts
- Test with latest features
- Might solve some of your bugs in the current branch
- Avoid extra tickets/issues which might arise if not rebased with the base branch

## Only squash if needed
If you follow atomic commits havit then chances are you will never have to squash. But rebase and squash commits if it makes sense to organize your local works: order, amend, or drop commits.

## Resolve conflicts
You will have less conflicts if you have the habit of remote rebasing. However, conflicts are part of the git workflow in big teams as no team can create perfectly atomic tickets/issues. So, here are some tips for resolving conflicts:
- When you are rebasing, current change means a commit from base branch and incoming change means one of your commit.
- Check if you need both changes or one of them.
- Always run automated tests after resolving conflicts. Also test the workflow that you just resolved the conflicts of.
- In doubt, you can always do `git rebase --abort` and start over again or take help from experienced ones. 

## Pull Request
- Before tagging someone for reviews make sure you have actually created a PR with your branch upto date with base branch.
- Always welcome feedbacks from reviewers: they are there to make the project better. On the contrary object the feedbacks if necessary with your logical answers.
- Communicate in the PR itself.
- When you review PRs follow best practices. Take it seriously. Don't hold back from giving correct reviews as your goal is to make best project not to please others.

## References
- https://www.aleksandrhovhannisyan.com/blog/atomic-git-commits/
