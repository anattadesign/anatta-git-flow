# Anatta Git Flow

This Git Workflow is for Anatta's internal team, It's hybrid copy of Gitflow. Gitflow is ideally suited for projects that have a scheduled release cycle so it did fit in our development workflow so we made few changes in it and comeup with a hybrid version.


## How it works

---  diagram coming soon ----

`Staging and Master Branches`

Instead of a single `master` branch, this workflow uses two branches to record the history of the project. The `master` branch stores the production ready features, and the `staging` branch serves as an integration branch for features.


The first step is to complement the default `master` with a `staging` branch. A simple way to do this is for one developer to create an empty `staging` branch locally and push it to the server.

```shell
git branch staging
git push -u origin staging
```

This branch will contain the complete history of the project, whereas master will contain an abridged version.

## Feature Branches

Each new feature should reside in its own branch using `feature/` prefix, But instead of branching off of `staging`, feature branches use `master` as their parent branch. When a feature is complete, it gets merged into `staging` for client reviews and when same feature is approved by client and QA, it gets merged into `master` for production deployments.

---  diagram coming soon ----

#### Creating a feature branch

```shell
git checkout master
git pull --ff-only origin master
git checkout -b feature/my_feature
```

#### Finishing a feature branch

When you’re done with the development work on the feature, the next step is to merge the `feature/my_feature` into `staging` to show it Client and QA for their approval.

```shell
git checkout staging
git pull --ff-only origin staging
git merge --no-ff feature/my_feature
git push origin staging
```

#### feature branch ready for production

After Client's approval, the next step is to merge the `feature/my_feature` into `master` to deploy it production server

```shell
git checkout master
git pull --ff-only origin master
git merge --no-ff feature/my_feature
git push origin master
```

#### Deleting feature branch
No need for this. Once you merge your branch to master. Delete it from the UI and in your local have the setting `git config --global fetch.prune true` So post deleting whenever you do git fetch it will sync remote and local.

## Hotfix Branches

`hotfix` branches are used to quickly patch for production. This branch that should fork directly off of `master`. As soon as the fix is complete, it should be merged into both `master` and `staging`.

```shell
git checkout master
git pull --ff-only origin master
git checkout -b hotfix/my_fix
```

Similar to finishing a feature branch, a hotfix branch gets merged into both `master` and `staging`.

```shell
git checkout master
git pull --ff-only origin master
git merge --no-ff hotfix/my_fix
git push origin master

git checkout staging
git pull --ff-only origin staging
git merge --no-ff hotfix/my_fix
git push origin staging
```

## Things to note
* Always use `--ff-only` when pulling changes from remote, otherwise it creates a merge commit, which pollutes history. Then you can rebase your local branch over remote and push accordingly.
* If a change is going to take 3 or more commits, it should not be in a hotfix branch
* When doing a merge you should always use `--no-ff` to create an additional commit. This helps to group related commits together.

## Useful git configuration
Git configuration can help you a lot with the projects. Here are a few useful options-
* `push.default=current` - When typing `git push` without any branch name, it will push the current branch automatically.
* `core.excludesfile=~/.gitignore` - Useful to create a global .gitignore file.
* `core.autocrlf=input` - Helps to set the line-endings to the unix standard. Useful for anyone who might have windows-style line-endings for some reason.
* `pull.rebase=true` - Automatically rebase if needed, when doing a fast-forward pull from remote.
* `help.autocorrect=50` - If you make a spelling mistake in the git command, git will automatically guess which command you wanted to run, and run it for you correctly.
* `git config --global fetch.prune true` - Sync local branch with remote

## Summary

- A `staging` branch is created from `master`
- A `feature` branch is created from `master`
- When a feature is complete it is merged into the `staging` branch
- When the feature is approved it is merged into `master`
- If an issue in `master` is detected a hotfix branch is created from `master`
- Once the hotfix is complete it is merged to both `staging` and `master`
