# Anatta Git Flow

This Git Workflow is for Anatta's internal team, It's hybrid copy of Gitflow. Gitflow is ideally suited for projects that have a scheduled release cycle so it did fit in our development workflow so we made few changes in it and comeup with a hybrid version.


## How it works

---  diagram coming soon ----

`Staging and Master Branches`

Instead of a single `master` branch, this workflow uses two branches to record the history of the project. The `master` branch stores the production ready features, and the `staging` branch serves as an integration branch for features.


The first step is to complement the default `master` with a `staging` branch. A simple way to do this is for one developer to create an empty `staging` branch locally and push it to the server.

`git branch staging`
`git push -u origin staging`

This branch will contain the complete history of the project, whereas master will contain an abridged version.

## Feature Branches

Each new feature should reside in its own branch using `feature/` prefix, But instead of branching off of `staging`, feature branches use `master` as their parent branch. When a feature is complete, it gets merged into `staging` for client reviews and when same feature is approved by client and QA, it gets merged into `master` for production deployments.

---  diagram coming soon ----

#### Creating a feature branch

`git checkout master`
`git pull origin master`
`git checkout -b feature/my_feature`

#### Finishing a feature branch

When you’re done with the development work on the feature, the next step is to merge the `feature/my_feature` into `staging` to show it Client and QA for their approval.

`git checkout staging`
`git pull origin staging`
`git merge --no-ff feature/my_feature`
`git push origin staging`

#### feature branch ready for production

After Client's approval, the next step is to merge the `feature/my_feature` into `master` to deploy it production server

`git checkout master`
`git pull origin master`
`git merge --no-ff feature/my_feature`
`git push origin master`

#### Deleting feature branch
Once it get deployed to prodction, the next step is to delete local and remote branch.

`git branch -D feature/my_feature`
`git push origin :feature/my_feature`


## Hotfix Branches

`hotfix` branches are used to quickly patch for production. This branch that should fork directly off of `master`. As soon as the fix is complete, it should be merged into both `master` and `staging`.

`git checkout master`
`git pull origin master`
`git checkout -b hotfix/my_fix`

Similar to finishing a feature branch, a hotfix branch gets merged into both `master` and `staging`.

`git checkout master`
`git pull origin master`
`git merge --no-ff hotfix/my_fix`
`git push origin master`

`git checkout staging`
`git pull origin staging`
`git merge --no-ff hotfix/my_fix`
`git push origin staging`


## Summary

- A `staging` branch is created from `master`
- A `feature` branch is created from `master`
- When a feature is complete it is merged into the `staging` branch
- When the feature is approved it is merged into `master`
- If an issue in `master` is detected a hotfix branch is created from `master`
- Once the hotfix is complete it is merged to both `staging` and `master`









