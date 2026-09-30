# Git Workflows

> Git reference for software and technology work.

------------------------------------------------------------------------

## Table of Contents

1.  [The Git Mental Model](#1-the-git-mental-model)
2.  [Installing and Configuring Git](#2-installing-and-configuring-git)
3.  [Creating and Connecting
    Repositories](#3-creating-and-connecting-repositories)
4.  [The Everyday Git Workflow](#4-the-everyday-git-workflow)
5.  [Understanding Git Status and
    Outputs](#5-understanding-git-status-and-outputs)
6.  [Commits](#6-commits)
7.  [Branches](#7-branches)
8.  [Remote Repositories](#8-remote-repositories)
9.  [Fetch vs Pull vs Push](#9-fetch-vs-pull-vs-push)
10. [Merging Branches](#10-merging-branches)
11. [Merge Conflicts](#11-merge-conflicts)
12. [Rebase](#12-rebase)
13. [Stashing](#13-stashing)
14. [Viewing and Understanding
    History](#14-viewing-and-understanding-history)
15. [Undoing Changes Safely](#15-undoing-changes-safely)
16. [Reset: Soft, Mixed, and Hard](#16-reset-soft-mixed-and-hard)
17. [Revert: Safely Undoing Published
    Commits](#17-revert-safely-undoing-published-commits)
18. [Recovering Lost Work with
    Reflog](#18-recovering-lost-work-with-reflog)
19. [Cherry-Picking](#19-cherry-picking)
20. [Tags and Releases](#20-tags-and-releases)
21. [Ignoring Files](#21-ignoring-files)
22. [Useful Inspection and Debugging
    Commands](#22-useful-inspection-and-debugging-commands)
23. [Unrelated Histories](#23-unrelated-histories)
24. [Collaborative Error Handling](#24-collaborative-error-handling)
25. [Common Git Problems and What to
    Do](#25-common-git-problems-and-what-to-do)
26. [Professional Git Workflows](#26-professional-git-workflows)
27. [Professional Software-Team
    Examples](#27-professional-software-team-examples)
28. [Workflow Decision Guide](#28-workflow-decision-guide)
29. [Git Command Cheat Sheet](#29-git-command-cheat-sheet)
30. [Final Professional Checklist](#30-final-professional-checklist)
31. [References](#references)

------------------------------------------------------------------------

# 1. The Git Mental Model

Git becomes much easier once you stop thinking of it as "a system that
uploads files" and start thinking of it as a **history and snapshot
system**.

Git primarily tracks:

-   files
-   changes
-   snapshots (commits)
-   relationships between commits
-   branches (movable pointers to commits)
-   remote repositories

A simplified history might look like:

``` text
A---B---C---D   main
         \
          E---F  feature/login
```

Here:

-   `A` is an older commit.
-   `B`, `C`, and `D` are commits on `main`.
-   `E` and `F` are commits made on the feature branch.
-   `feature/login` points to `F`.
-   `main` points to `D`.

A branch is not a separate copy of your project in the way beginners
sometimes imagine. A branch is essentially a **named pointer to a
commit**.

------------------------------------------------------------------------

## 1.1 The Three Important Local States

Git commonly involves three areas:

``` text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Repository / Commit History
```

### Working directory

Your actual files.

### Staging area

The changes you have selected for the next commit.

### Repository

The committed history.

Example:

``` bash
git status
```

You modify:

``` text
app.py
```

Then:

``` bash
git add app.py
```

Now `app.py` is staged.

Then:

``` bash
git commit -m "Add login validation"
```

The staged changes become a commit.

------------------------------------------------------------------------

# 2. Installing and Configuring Git

Check Git:

``` bash
git --version
```

Configure your identity:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Check configuration:

``` bash
git config --global --list
```

Useful configuration:

``` bash
git config --global init.defaultBranch main
git config --global pull.rebase false
```

The exact settings used by teams vary. In professional environments,
project-level configuration may override your global configuration.

------------------------------------------------------------------------

# 3. Creating and Connecting Repositories

## 3.1 Start a New Local Repository

Inside a project:

``` bash
git init
```

Typical output:

``` text
Initialized empty Git repository in ...
```

Then:

``` bash
git add .
git commit -m "Initial commit"
```

------------------------------------------------------------------------

## 3.2 Clone an Existing Repository

For an existing remote repository:

``` bash
git clone https://github.com/company/project.git
cd project
```

Cloning normally creates:

-   the working directory
-   a local Git repository
-   a remote called `origin`

Check:

``` bash
git remote -v
```

Typical output:

``` text
origin  https://github.com/company/project.git (fetch)
origin  https://github.com/company/project.git (push)
```

------------------------------------------------------------------------

## 3.3 Connect an Existing Local Repository to GitHub

If you already have:

``` bash
git init
```

and need to connect it to a remote:

``` bash
git remote add origin https://github.com/company/project.git
```

Check:

``` bash
git remote -v
```

Then:

``` bash
git branch -M main
git push -u origin main
```

------------------------------------------------------------------------

# 4. The Everyday Git Workflow

The basic workflow is:

``` text
1. Get current changes
2. Create/use a feature branch
3. Edit files
4. Inspect changes
5. Stage changes
6. Commit
7. Push
8. Open a Pull Request / Merge Request
9. Review
10. Merge
```

Typical commands:

``` bash
git switch main
git pull
git switch -c feature/user-login

# edit files

git status
git diff
git add .
git status
git commit -m "Add user login"
git push -u origin feature/user-login
```

Then the team reviews the branch.

------------------------------------------------------------------------

# 5. Understanding Git Status and Outputs

Run:

``` bash
git status
```

This is one of the most important Git commands.

You may see:

``` text
On branch feature/login
Your branch is up to date with 'origin/feature/login'.

Changes not staged for commit:
  modified:   src/login.py

Untracked files:
  tests/test_login.py
```

This means:

-   You are on `feature/login`.
-   Git knows about the branch's remote tracking branch.
-   `src/login.py` changed but is not staged.
-   `tests/test_login.py` is new and untracked.

Stage:

``` bash
git add src/login.py tests/test_login.py
```

Then:

``` bash
git status
```

You may see:

``` text
Changes to be committed:
  modified:   src/login.py
  new file:   tests/test_login.py
```

These changes are now staged.

------------------------------------------------------------------------

# 6. Commits

## 6.1 Create a Commit

``` bash
git add .
git commit -m "Add user authentication"
```

A good commit should normally represent one coherent change.

Prefer:

``` text
Add password validation
```

over:

``` text
stuff
```

or:

``` text
changes
```

------------------------------------------------------------------------

## 6.2 View the Most Recent Commit

``` bash
git show
```

Or:

``` bash
git show HEAD
```

`HEAD` means "the commit currently checked out."

------------------------------------------------------------------------

## 6.3 Amend the Most Recent Commit

If you forgot a file:

``` bash
git add forgotten-file.py
git commit --amend
```

Or:

``` bash
git commit --amend -m "Add user authentication"
```

Be careful: amending rewrites the commit.

Avoid rewriting a commit that other people have already based work on
unless your team explicitly expects it.

------------------------------------------------------------------------

# 7. Branches

## 7.1 List Branches

``` bash
git branch
```

Remote branches:

``` bash
git branch -r
```

All branches:

``` bash
git branch -a
```

------------------------------------------------------------------------

## 7.2 Create a Branch

Modern syntax:

``` bash
git switch -c feature/login
```

Older/common syntax:

``` bash
git checkout -b feature/login
```

------------------------------------------------------------------------

## 7.3 Switch Branches

``` bash
git switch main
```

or:

``` bash
git checkout main
```

------------------------------------------------------------------------

## 7.4 Delete a Local Branch

After it has been merged:

``` bash
git branch -d feature/login
```

Force deletion:

``` bash
git branch -D feature/login
```

`-D` can delete a branch containing work that Git considers unmerged.
Use it carefully.

------------------------------------------------------------------------

## 7.5 Rename a Branch

``` bash
git branch -m old-name new-name
```

For the current branch:

``` bash
git branch -m new-name
```

------------------------------------------------------------------------

# 8. Remote Repositories

A remote is another Git repository, usually hosted on:

-   GitHub
-   GitLab
-   Bitbucket
-   Azure DevOps
-   another Git server

List remotes:

``` bash
git remote -v
```

Add one:

``` bash
git remote add origin URL
```

Change one:

``` bash
git remote set-url origin URL
```

Remove one:

``` bash
git remote remove origin
```

------------------------------------------------------------------------

# 9. Fetch vs Pull vs Push

These three commands are extremely important.

## 9.1 Fetch

``` bash
git fetch origin
```

Fetch downloads information from the remote without changing your
current working files.

Think:

``` text
Remote
   |
   | fetch
   v
Local knowledge of remote branches
```

It lets you inspect what changed before integrating it.

------------------------------------------------------------------------

## 9.2 Pull

``` bash
git pull
```

Conceptually:

``` text
git fetch
+
git merge
```

Depending on configuration, pull may instead perform a rebase.

For explicit behavior:

``` bash
git pull --rebase
```

or:

``` bash
git pull --no-rebase
```

------------------------------------------------------------------------

## 9.3 Push

``` bash
git push
```

Push sends local commits to the remote repository.

For a new branch:

``` bash
git push -u origin feature/login
```

`-u` establishes the upstream tracking relationship.

After that:

``` bash
git push
```

is usually enough.

------------------------------------------------------------------------

# 10. Merging Branches

Suppose:

``` text
main:     A---B---C
               \
feature:        D---E
```

You want to merge the feature into main.

``` bash
git switch main
git merge feature
```

Possible result:

``` text
A---B---C-------M
         \     /
          D---E
```

`M` is a merge commit.

------------------------------------------------------------------------

## 10.1 Fast-Forward Merge

If main has not moved:

``` text
A---B---C
         \
          D---E
```

Git can simply move the branch pointer:

``` text
A---B---C---D---E
```

This is a fast-forward merge.

------------------------------------------------------------------------

## 10.2 Force a Merge Commit

Some teams prefer explicit merge commits:

``` bash
git merge --no-ff feature/login
```

This preserves a visible branch integration point.

------------------------------------------------------------------------

# 11. Merge Conflicts

A conflict happens when Git cannot automatically decide how two sets of
changes should be combined.

Example:

``` text
<<<<<<< HEAD
const timeout = 30;
=======
const timeout = 60;
>>>>>>> feature/config
```

Meaning:

-   `HEAD` = your current branch version
-   `feature/config` = incoming branch version

You must decide what the final code should be.

For example:

``` javascript
const timeout = 60;
```

Remove the conflict markers.

Then:

``` bash
git add path/to/file
```

After all conflicts are resolved:

``` bash
git status
```

Then complete the merge:

``` bash
git commit
```

Depending on the Git operation, Git may also provide a command to
continue.

------------------------------------------------------------------------

## 11.1 Abort a Merge

If you realize the merge should not happen:

``` bash
git merge --abort
```

This is often the safest response when you are confused by a merge.

------------------------------------------------------------------------

## 11.2 Conflict-Resolution Workflow

A disciplined workflow:

``` bash
git status
git diff
```

Identify every conflicted file.

Open each file.

Resolve the conflict.

Run tests.

Then:

``` bash
git add resolved-file
git status
```

Once all conflicts are resolved:

``` bash
git commit
```

Never assume that "Git stopped reporting conflicts" means the
application is correct.

Always test after resolving conflicts.

------------------------------------------------------------------------

# 12. Rebase

Rebase moves your commits onto another base.

Suppose:

``` text
A---B---C---D main
     \
      E---F feature
```

Rebase:

``` bash
git switch feature
git rebase main
```

Conceptually:

``` text
A---B---C---D---E'---F'
```

Your commits are replayed on top of the latest `main`.

The commits receive new IDs because they are recreated.

------------------------------------------------------------------------

## 12.1 Why Rebase?

Rebase can create a cleaner linear history:

``` text
A---B---C---D---E---F
```

instead of:

``` text
A---B---C---M
     \       /
      D---E
```

However, rebase rewrites commit history.

### Core rule

Do not casually rebase commits that other developers have already based
work on.

A useful professional principle:

> Rebase your private/unshared work freely; coordinate carefully before
> rewriting shared history.

------------------------------------------------------------------------

## 12.2 Interactive Rebase

``` bash
git rebase -i HEAD~4
```

This lets you manipulate recent commits.

Common operations:

``` text
pick    keep commit
reword  change commit message
edit    stop and modify commit
squash  combine with previous commit
fixup   combine and discard message
drop    remove commit
```

Example:

``` text
pick abc123 Add login form
squash def456 Fix login typo
squash 789abc Fix another login issue
```

This can turn several development commits into one clean commit.

------------------------------------------------------------------------

# 13. Stashing

Stashing temporarily stores uncommitted work.

Suppose you are working on:

``` text
feature/payment
```

but urgently need to switch branches.

Check:

``` bash
git status
```

Then:

``` bash
git stash
```

Your working tree becomes clean.

Switch:

``` bash
git switch main
```

Do your urgent work.

Return:

``` bash
git switch feature/payment
```

Restore:

``` bash
git stash pop
```

------------------------------------------------------------------------

## 13.1 Name a Stash

``` bash
git stash push -m "Payment API work in progress"
```

List:

``` bash
git stash list
```

Example:

``` text
stash@{0}: On feature/payment: Payment API work in progress
stash@{1}: On feature/login: Login validation
```

------------------------------------------------------------------------

## 13.2 Apply Without Removing

``` bash
git stash apply stash@{0}
```

Unlike `pop`, `apply` keeps the stash.

------------------------------------------------------------------------

## 13.3 Remove a Stash

``` bash
git stash drop stash@{0}
```

Clear all stashes:

``` bash
git stash clear
```

Use this carefully.

------------------------------------------------------------------------

## 13.4 Stash Untracked Files

By default, untracked files may not be included.

Use:

``` bash
git stash -u
```

or:

``` bash
git stash push --include-untracked
```

------------------------------------------------------------------------

# 14. Viewing and Understanding History

Basic:

``` bash
git log
```

Compact:

``` bash
git log --oneline
```

Graph:

``` bash
git log --oneline --graph --decorate --all
```

A particularly useful alias-style command:

``` bash
git log --oneline --graph --decorate --all
```

Example:

``` text
* 81ab234 (HEAD -> feature/login) Add validation
* 7cd1234 Add login form
| * 44aa111 (main) Update dependencies
|/
* 123abcd Initial commit
```

This lets you see:

-   commits
-   branches
-   HEAD
-   merge relationships
-   divergent histories

------------------------------------------------------------------------

## 14.1 Show One File's History

``` bash
git log -- path/to/file
```

With patches:

``` bash
git log -p -- path/to/file
```

------------------------------------------------------------------------

## 14.2 Find Who Changed a Line

``` bash
git blame path/to/file
```

Use `blame` to understand history, not to assign personal blame.

It answers:

> Which commit last changed this line?

------------------------------------------------------------------------

## 14.3 Compare Commits

``` bash
git diff commit1 commit2
```

Compare branches:

``` bash
git diff main..feature/login
```

Compare your branch against remote:

``` bash
git diff origin/main..main
```

------------------------------------------------------------------------

# 15. Undoing Changes Safely

Different Git operations undo different things.

  -----------------------------------------------------------------------
  Situation                           Common command
  ----------------------------------- -----------------------------------
  Undo unstaged file changes          `git restore file`

  Unstage a file                      `git restore --staged file`

  Undo latest local commit but keep   `git reset --soft HEAD~1`
  changes                             

  Undo latest local commit and        `git reset HEAD~1`
  unstage changes                     

  Delete local changes                `git reset --hard HEAD`

  Undo a published commit             `git revert <commit>`

  Recover lost commits                `git reflog`
  -----------------------------------------------------------------------

The key question is:

> Has the commit been shared with other people?

If yes, prefer history-preserving operations such as `git revert`.

------------------------------------------------------------------------

# 16. Reset: Soft, Mixed, and Hard

Reset moves a branch pointer.

## 16.1 Soft Reset

``` bash
git reset --soft HEAD~1
```

The commit is removed from the branch history, but the changes remain
staged.

Useful when:

> "I committed too early and want to recreate the commit."

------------------------------------------------------------------------

## 16.2 Mixed Reset

``` bash
git reset HEAD~1
```

The commit is removed, and changes remain in the working directory but
are unstaged.

This is the usual default reset mode.

------------------------------------------------------------------------

## 16.3 Hard Reset

``` bash
git reset --hard HEAD~1
```

This moves the branch back and discards changes represented by the
removed commit from the working tree.

### WARNING

`--hard` can destroy uncommitted work.

Before using it, ask:

> Do I have anything here that I cannot recreate?

If yes, stash or commit it first.

------------------------------------------------------------------------

# 17. Revert: Safely Undoing Published Commits

If a commit has already been pushed and others may have pulled it, use:

``` bash
git revert <commit>
```

Git creates a new commit that reverses the previous commit.

Example:

``` text
A---B---C---D
        ^
      bad
```

Run:

``` bash
git revert C
```

Result:

``` text
A---B---C---D---R
```

`R` undoes the effect of `C` without rewriting shared history.

This is one of the most important professional Git practices.

------------------------------------------------------------------------

# 18. Recovering Lost Work with Reflog

`reflog` records movements of local references.

Run:

``` bash
git reflog
```

You might see:

``` text
81ab234 HEAD@{0}: commit: Add payment validation
7cd1234 HEAD@{1}: reset: moving to HEAD~1
81ab234 HEAD@{2}: commit: Add payment validation
```

If you accidentally reset a commit, you can often recover it.

For example:

``` bash
git reset --hard 81ab234
```

or create a recovery branch:

``` bash
git switch -c recovery 81ab234
```

### Important

Reflog is primarily local. Do not assume another developer's reflog
contains your history.

------------------------------------------------------------------------

# 19. Cherry-Picking

Cherry-pick applies a specific commit onto your current branch.

``` bash
git cherry-pick <commit>
```

Example:

``` text
main:
A---B---C

feature:
A---B---D---E
```

You want only `D`:

``` bash
git switch main
git cherry-pick D
```

Result:

``` text
A---B---C---D'
```

Useful for:

-   backporting a bug fix
-   moving a small isolated change
-   applying a hotfix to a release branch

Be careful with large chains of dependent commits.

------------------------------------------------------------------------

# 20. Tags and Releases

Tags identify important commits.

Create:

``` bash
git tag v1.0.0
```

Annotated tag:

``` bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

List:

``` bash
git tag
```

Push:

``` bash
git push origin v1.0.0
```

Push all tags:

``` bash
git push origin --tags
```

Tags are commonly used for:

``` text
v1.0.0
v1.1.0
v2.0.0
```

and can correspond to production releases.

------------------------------------------------------------------------

# 21. Ignoring Files

Create:

``` text
.gitignore
```

Example:

``` gitignore
# Python
__pycache__/
*.pyc
.venv/

# Node
node_modules/

# Environment variables
.env

# IDE
.vscode/

# Build output
dist/
build/
```

Never casually commit:

-   passwords
-   API keys
-   private certificates
-   production credentials
-   `.env` files containing secrets

If a secret was already committed, simply adding it to `.gitignore` does
not remove it from Git history.

Treat exposed secrets as compromised and rotate them.

------------------------------------------------------------------------

# 22. Useful Inspection and Debugging Commands

## Current branch

``` bash
git branch --show-current
```

## Repository state

``` bash
git status
```

## Remote information

``` bash
git remote -v
```

## Branch tracking

``` bash
git branch -vv
```

## Recent history

``` bash
git log --oneline -10
```

## Full graph

``` bash
git log --oneline --graph --decorate --all
```

## Exact commit

``` bash
git show <commit>
```

## Find a commit by message

``` bash
git log --all --grep="login"
```

## Search for a string in tracked files

``` bash
git grep "someFunction"
```

## Find when a line changed

``` bash
git blame file.py
```

------------------------------------------------------------------------

# 23. Unrelated Histories

This is the problem shown by:

``` text
fatal: refusing to merge unrelated histories
```

It happens when Git sees two histories with no common ancestor.

Common example:

``` text
Local repository:

A---B---C

Remote repository:

X---Y---Z
```

There is no shared starting commit.

This often happens when:

1.  You ran `git init` locally.
2.  You created commits locally.
3.  You created a GitHub repository separately.
4.  The GitHub repository already had an initial
    README/license/.gitignore commit.
5.  You try to pull them together.

------------------------------------------------------------------------

## 23.1 Merge the Histories

If you genuinely want both histories:

``` bash
git pull origin main --allow-unrelated-histories
```

Resolve conflicts if necessary.

Then:

``` bash
git add .
git commit
```

Push:

``` bash
git push origin main
```

------------------------------------------------------------------------

## 23.2 Before Doing This

Inspect both histories:

``` bash
git log --oneline --graph --all
```

Also:

``` bash
git remote -v
```

Ask:

> Are these actually two versions of the same project?

If not, merging them may be the wrong solution.

------------------------------------------------------------------------

## 23.3 Alternative: Replace the Remote

If the remote is disposable and your local repository is the intended
source of truth, a different strategy may be appropriate.

For example:

``` bash
git push -u origin main
```

If the remote contains commits you intentionally want to overwrite,
force pushing may be considered, but this is a **high-risk collaborative
operation**.

Prefer:

``` bash
git push --force-with-lease
```

over:

``` bash
git push --force
```

when rewriting remote history is genuinely necessary.

Never force-push a shared branch casually.

------------------------------------------------------------------------

# 24. Collaborative Error Handling

Professional Git work is less about memorizing commands and more about
managing risk.

A useful decision process is:

``` text
Something went wrong
        |
        v
Is my work committed?
        |
   +----+----+
   |         |
  No        Yes
   |         |
stash/      Has it been
commit      pushed?
             |
        +----+----+
        |         |
       No        Yes
        |         |
      reset     revert
      /rebase   or coordinated fix
```

------------------------------------------------------------------------

## 24.1 If You Have Uncommitted Work

First:

``` bash
git status
```

Then decide:

### Keep it

Commit:

``` bash
git add .
git commit -m "WIP: ..."
```

or stash:

``` bash
git stash push -m "Work in progress"
```

### Throw it away

For one file:

``` bash
git restore path/to/file
```

For all tracked working-tree changes:

``` bash
git restore .
```

Be careful: these operations can discard work.

------------------------------------------------------------------------

# 24.2 If You Made a Bad Local Commit

If it has not been pushed:

``` bash
git reset --soft HEAD~1
```

or:

``` bash
git reset HEAD~1
```

Then rebuild the commit.

------------------------------------------------------------------------

# 24.3 If You Made a Bad Published Commit

Prefer:

``` bash
git revert <bad-commit>
```

Then:

``` bash
git push
```

This preserves shared history.

------------------------------------------------------------------------

# 24.4 If You Accidentally Rebased Shared Work

Stop before pushing further.

Tell the team what happened.

Inspect:

``` bash
git reflog
```

Find the previous branch position.

Create a recovery branch:

``` bash
git switch -c recovery <old-commit>
```

Then coordinate the correction with the team.

------------------------------------------------------------------------

# 24.5 If Your Branch Is Behind Remote

Start with:

``` bash
git fetch origin
```

Inspect:

``` bash
git log --oneline --graph --decorate --all
```

Then choose:

``` bash
git merge origin/main
```

or:

``` bash
git rebase origin/main
```

based on the team's workflow.

------------------------------------------------------------------------

# 24.6 If You Have Merge Conflicts

Do not panic.

The workflow is:

``` bash
git status
git diff
```

Resolve one file at a time.

Then:

``` bash
git add file
```

Run tests.

Then complete the merge/rebase.

For a rebase:

``` bash
git rebase --continue
```

To abandon:

``` bash
git rebase --abort
```

For a merge:

``` bash
git merge --abort
```

------------------------------------------------------------------------

# 25. Common Git Problems and What to Do

## "Your branch is behind"

Usually:

``` bash
git pull
```

or the team's preferred fetch + merge/rebase workflow.

------------------------------------------------------------------------

## "Non-fast-forward"

You tried to push while the remote contains commits you don't have.

Do:

``` bash
git fetch origin
```

Inspect:

``` bash
git log --oneline --graph --all
```

Then integrate the remote work.

Avoid immediately doing:

``` bash
git push --force
```

------------------------------------------------------------------------

## "Please commit your changes or stash them before you switch branches"

Git is protecting your uncommitted work.

Options:

``` bash
git add .
git commit
```

or:

``` bash
git stash
```

Then switch branches.

------------------------------------------------------------------------

## "CONFLICT"

Git needs human input.

Use:

``` bash
git status
```

Resolve the files.

Then stage them.

------------------------------------------------------------------------

## "fatal: refusing to merge unrelated histories"

The histories do not share a common ancestor.

Investigate first.

If intentional:

``` bash
git pull --allow-unrelated-histories
```

------------------------------------------------------------------------

## "Detached HEAD"

You may have checked out a commit directly.

Example:

``` bash
git checkout abc123
```

You are not currently on a normal branch.

If you want to preserve work:

``` bash
git switch -c recovery-work
```

Then continue from that branch.

------------------------------------------------------------------------

# 26. Professional Git Workflows

There is no single Git workflow used by every company.

The right workflow depends on:

-   team size
-   deployment model
-   release frequency
-   regulatory requirements
-   product type
-   repository structure
-   CI/CD
-   whether releases are continuous or scheduled

Three important patterns are:

1.  Feature branch + Pull Request
2.  Trunk-based development
3.  GitFlow-style release branching

------------------------------------------------------------------------

# 27. Professional Software-Team Examples

## 27.1 Feature Branch + Pull Request Workflow

A common workflow:

``` text
main
 |
 +---- feature/login
 |          |
 |       commits
 |          |
 +----------+---- Pull Request
                    |
                 review
                    |
                  CI tests
                    |
                  merge
                    |
                   main
```

Typical developer workflow:

``` bash
git switch main
git pull

git switch -c feature/login

# work
git add .
git commit -m "Add login endpoint"

git push -u origin feature/login
```

Then create a Pull Request.

Reviewers examine:

-   correctness
-   tests
-   security
-   maintainability
-   architecture
-   performance
-   compatibility

CI may run:

``` text
lint
unit tests
integration tests
security scans
build
deployment checks
```

After approval, the branch is merged.

------------------------------------------------------------------------

## 27.2 Keeping a Feature Branch Current

Suppose:

``` text
main:
A---B---C---D

feature:
A---B---E---F
```

Main moved forward.

A developer can update the feature branch.

### Merge approach

``` bash
git switch feature/login
git fetch origin
git merge origin/main
```

History becomes:

``` text
A---B---C---D
     \       \
      E---F---M
```

### Rebase approach

``` bash
git switch feature/login
git fetch origin
git rebase origin/main
```

Conceptually:

``` text
A---B---C---D---E'---F'
```

Which approach is preferred is a team policy.

------------------------------------------------------------------------

# 27.3 Trunk-Based Development

In trunk-based development, developers integrate into a shared
main/trunk branch frequently.

Conceptually:

``` text
main
 |
 +--- small change
 |
 +--- small change
 |
 +--- small change
 |
 +--- release
```

Features may use short-lived branches or feature flags.

A developer might:

``` bash
git switch main
git pull

git switch -c small-change
```

Make a small change.

``` bash
git commit
git push
```

Open a PR.

Merge quickly.

This approach emphasizes:

-   small changes
-   frequent integration
-   automated testing
-   short-lived branches
-   continuous integration
-   feature flags when necessary

------------------------------------------------------------------------

# 27.4 Feature Flags

Feature flags allow code to be deployed without immediately exposing
functionality.

Conceptually:

``` python
if feature_flags.new_checkout:
    use_new_checkout()
else:
    use_old_checkout()
```

This separates:

``` text
Deploying code
```

from:

``` text
Releasing functionality
```

This can be valuable for large software teams.

------------------------------------------------------------------------

# 27.5 GitFlow-Style Workflow

A GitFlow-style model may contain:

``` text
main
develop
feature/*
release/*
hotfix/*
```

Example:

``` text
main
 |
 +----------------------+
                        |
develop                 |
 |                      |
 +-- feature/login      |
 |                      |
 +-- feature/payment    |
 |                      |
 +------ release/2.0 ---+
```

Typical concepts:

### `main`

Production/release history.

### `develop`

Integration branch for upcoming work.

### `feature/*`

Individual features.

### `release/*`

Release preparation.

### `hotfix/*`

Urgent production corrections.

This model can be useful in environments with scheduled releases and
multiple supported release lines, but it can also introduce additional
branch-management overhead.

------------------------------------------------------------------------

# 27.6 Hotfix Workflow

Suppose production has a critical bug.

A team may create:

``` bash
git switch main
git pull
git switch -c hotfix/payment-timeout
```

Fix it:

``` bash
git add .
git commit -m "Fix payment timeout"
git push -u origin hotfix/payment-timeout
```

Open a PR.

After review and CI:

``` text
hotfix
   |
   +---- main
```

If the organization maintains release branches, the fix may also need to
be backported.

Cherry-pick can be useful:

``` bash
git cherry-pick <fix-commit>
```

------------------------------------------------------------------------

# 27.7 Release Branch Workflow

For scheduled releases:

``` bash
git switch main
git pull

git switch -c release/2.4.0
```

Release branch may contain:

-   version changes
-   documentation
-   final bug fixes
-   release configuration

After release:

``` text
release/2.4.0
       |
       +---- production
```

Tag the release:

``` bash
git tag -a v2.4.0 -m "Release 2.4.0"
git push origin v2.4.0
```

------------------------------------------------------------------------

# 27.8 Pull Request Workflow

A professional PR normally answers:

### What changed?

Example:

> Added password reset endpoint.

### Why?

> Users previously had no self-service password recovery.

### How was it tested?

``` text
pytest
npm test
integration tests
manual verification
```

### Risks?

> Changes authentication flow; existing sessions were tested.

### Database changes?

> Added password reset token table.

A good PR makes it easy for another developer to understand the change
without reconstructing the entire development process.

------------------------------------------------------------------------

# 27.9 Code Review Workflow

Reviewer checks:

``` text
Correctness
   |
Security
   |
Tests
   |
Maintainability
   |
Performance
   |
Compatibility
```

Developers should respond to review comments with either:

-   code changes
-   explanation
-   evidence
-   discussion

Avoid using Git history as a substitute for communication.

------------------------------------------------------------------------

# 27.10 CI/CD Workflow

A professional repository may automatically perform:

``` text
Push
 |
 v
CI
 |
 +-- lint
 +-- unit tests
 +-- integration tests
 +-- build
 +-- security checks
 |
 v
Pull Request
 |
 v
Review
 |
 v
Merge
 |
 v
Deployment
```

Git is therefore part of a larger engineering system.

Git itself stores and manages history; CI/CD systems automate validation
and delivery around that history.

------------------------------------------------------------------------

# 28. Workflow Decision Guide

## I have uncommitted work and need to switch branches

Use:

``` bash
git stash
```

or make a temporary commit if appropriate.

------------------------------------------------------------------------

## I committed something locally by mistake

Use:

``` bash
git reset
```

or:

``` bash
git commit --amend
```

depending on the situation.

------------------------------------------------------------------------

## I already pushed the bad commit

Prefer:

``` bash
git revert
```

------------------------------------------------------------------------

## I lost a commit

Check:

``` bash
git reflog
```

------------------------------------------------------------------------

## I need one commit from another branch

Use:

``` bash
git cherry-pick
```

------------------------------------------------------------------------

## Two branches conflict

Resolve the files, test, stage, and complete the merge/rebase.

------------------------------------------------------------------------

## Git says histories are unrelated

Investigate first.

If the histories genuinely belong together:

``` bash
git merge --allow-unrelated-histories
```

------------------------------------------------------------------------

## I need to update my feature branch

Depending on team policy:

``` bash
git fetch origin
git merge origin/main
```

or:

``` bash
git fetch origin
git rebase origin/main
```

------------------------------------------------------------------------

## I need to undo a shared commit

Use:

``` bash
git revert <commit>
```

------------------------------------------------------------------------

## I need to rewrite shared history

Stop and coordinate with the team first.

If rewriting is explicitly approved, prefer:

``` bash
git push --force-with-lease
```

rather than:

``` bash
git push --force
```

------------------------------------------------------------------------

# 29. Git Command Cheat Sheet

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

------------------------------------------------------------------------

# 30. Final Professional Checklist

Before committing:

``` text
[ ] Is the change focused?
[ ] Did I inspect git diff?
[ ] Did I avoid committing secrets?
[ ] Did I add/update tests?
[ ] Is the commit message meaningful?
```

Before pushing:

``` text
[ ] Am I on the correct branch?
[ ] Is the branch name correct?
[ ] Have I checked the latest remote changes?
[ ] Do tests pass?
[ ] Am I about to rewrite shared history?
```

Before merging:

``` text
[ ] Conflicts resolved?
[ ] Tests pass?
[ ] Code reviewed?
[ ] CI passes?
[ ] Database/migration implications checked?
[ ] Deployment implications understood?
```

When something goes wrong:

``` text
1. Stop.
2. Run git status.
3. Inspect git log.
4. Protect uncommitted work.
5. Determine whether the work is shared.
6. Choose reset/revert/rebase/merge accordingly.
7. Test.
8. Communicate with the team if shared history is involved.
```

------------------------------------------------------------------------

# The Core Git Philosophy

The most useful way to think about Git professionally is not:

> "Which command fixes this?"

Instead ask:

> **What state is my repository in, what state do I want, and has this
> history been shared with anyone else?**

That question determines the correct operation.

A practical hierarchy is:

``` text
                    Git problem
                         |
                         v
                  Check git status
                         |
                         v
                 Protect your work
                 /               \
             stash              commit
                 \               /
                  v             v
                 Understand history
                         |
                         v
              Has the history been shared?
                    /           \
                  No             Yes
                  |               |
             reset/rebase      revert/merge
             amend/cherry      coordinated fix
                  \               /
                   \             /
                    v           v
                       Test
                        |
                        v
                    Communicate
                        |
                        v
                     Push
```

Git is fundamentally a tool for **controlled change and recoverable
history**.

The professional skill is not knowing every command from memory. It is
knowing:

-   what Git currently believes
-   what you want Git to believe
-   what work must be preserved
-   whether other people depend on the current history
-   how to make the smallest safe change
-   how to verify the result

Once those principles are understood, the commands become much easier to
learn.

# References
Github Docs :
https://git-scm.com/docs/git

Github cheat sheets :
https://education.github.com/git-cheat-sheet-education.pdf

git bootcamp summaries:
https://rcs.bu.edu/examples/Git/Bootcamp/Git_CheatSheet.pdf

git tutorial:
https://git-scm.com/docs/gittutorial

git learn:
https://git-scm.com/learn
