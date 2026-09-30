#  Git Command Cheat Sheet

## Setup

``` bash
git init
git clone URL
git config --global user.name "Name"
git config --global user.email "Email"
```

## Status

``` bash
git status
git branch
git branch -a
git remote -v
```

## Changes

``` bash
git diff
git add file
git add .
git restore file
git restore --staged file
```

## Commits

``` bash
git commit -m "Message"
git commit --amend
git show
```

## Branches

``` bash
git switch main
git switch -c feature/name
git branch -d feature/name
git branch -m new-name
```

## Remote

``` bash
git fetch
git pull
git pull --rebase
git push
git push -u origin branch
```

## Merge

``` bash
git merge branch
git merge --abort
```

## Rebase

``` bash
git rebase main
git rebase -i HEAD~5
git rebase --continue
git rebase --abort
```

## Stash

``` bash
git stash
git stash list
git stash pop
git stash apply
git stash drop
git stash -u
```

## History

``` bash
git log
git log --oneline
git log --oneline --graph --decorate --all
git reflog
git blame file
```

## Undo

``` bash
git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~1
git revert <commit>
```

## Cherry-pick

``` bash
git cherry-pick <commit>
```

## Tags

``` bash
git tag
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
```
