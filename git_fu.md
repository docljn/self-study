# Git Fu
## Sources
https://services.github.com/on-demand/downloads/github-git-cheat-sheet/

https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config

http://www.pauline-vos.nl/git-legit-cheatsheet/

https://brightonruby.com/2018/a-branch-in-time-tekin-suleyman/

https://www.raywenderlich.com/2288-git-tutorial-git-fu-with-the-command-line

https://railsware.com/blog/2014/08/11/git-housekeeping-tutorial-clean-up-outdated-branches-in-local-and-remote-repositories/

https://medium.com/@gabriellamedas/git-rebase-and-git-rebase-onto-a6a3f83f9cce

https://medium.com/@eclectusmedia/a-little-git-fu-for-the-budding-young-grasshopper-programmer-ecf2873fc85d

https://git.wtf/

## Setup
- user name and email
  - ```git config --global user.name “Your Name”```
  - ```git config --global user.email “your@email.address”```
- the full diff is visible when writing the commit message
  - ```git config --global commit.verbose true```
- use colours
  - ```git config --global color.ui auto```

## Writing (and ReWriting) Git History

### Why do it this way: someone will need to read it...
- Don't assume you'll always have access to the trello card and board
  - what if we move to another project management tool?
- when writing a commit message, make sure you include the why in the body.
  - consider not using ``` git commit -m ``` as the commandline isn't good for structuring text but rather ``` git commit ```

### Rewriting history:
- treat your commits as mutable until they get merged into master
  - unless you are collaborating on one branch!

#### How to do it
- Return all changes in a commit to the current working directory
  - ``` git reset HEAD~ ```
- Delete the last commit including all the code changes
  - ``` git reset --hard HEAD~ ```
- Add changes that should have been in the last commit without changing the commit message
  - ``` git add filename_of_changed_file```
  - ``` git commit --amend --no-edit```
- Fine-grained commit control
  - add changes you have made to a single file in several separate commits
    - ``` git add --patch filename```
- Incorporating changes from another branch
  - pull changes from one branch into your own, file by file
    - ``` git checkout source-branch-name file-to-checkout ```
  - pull changes from a specific commit into your own branch, file by file
    - ``` git checkout source-sha file-to-checkout ```
  - pull changes into your branch, commit by commit
    - ``` git cherry-pick sha-of-desired-commit ```
- Moving changes from the current live situation to anywhere else
  - ``` git stash```
  - ``` git checkout commit where changes belong```
  - ``` git stash apply``` (leaves changes in the stash for future use - use pop to remove)
- Merge a commit into the last one, interactively (see Interactive Rebase below)
  - ``` git rebase -i HEAD~``` ...
- Abort the current rebase and return to the original code status
  - ``` git rebase --abort```

#### Interactive Rebase
- *CARE*
  - if you rebase you will need to force push
  - only do this to you own branch, not a shared branch
    - ``` git push --force-with-lease``` will warn if there are any unexpected changes on the upstream branch
- selecting/changing/etc commits
  - drop: exclude this commit from the rebase (deleting the line also works)
  - pick: include this commit in the rebase
  - fixup: merge this commit into the previous commit and discard this commit message
  - squash: merge this commit into the previous commit, but keep both commit messages
  - reword: include this commit in the rebase, and edit the commit message
  - edit: stop after this commit has been applied, edit the files, commit, and continue
  - exec: execute the supplied shell script
- to continue an interactive rebase session
  - ``` git rebase --continue```
- you can stash, run tests, etc during an interactive rebase session...


## Reading Git History

### Overview of Changes
- one line log view
  - ``` git log --graph --pretty=oneline --abbrev-commit ```
- short format log (more details) restricted by date
  - ``` git log --graph --pretty=short --abbrev-commit --since=April ```
- more detailed log, restricted by author
  - ``` git log --graph --pretty=fuller --abbrev-commit --since=April --author="Patryk Małek" ```
- accepted pull requests restricted by date range
  - ``` git log --grep="pull request" --since=21june ```
- list of files changed in each commit
  - ``` git log --name-only --oneline ```
- list of files changed between the current commit and a specified commit (eg three previous)
  - ``` git diff --name-only HEAD~3 ```
- which branches was I working on recently
  - ``` git branch --sort=-committerdate | head ```
- which local branches have no remote
  - ``` git branch -vv | cut -c 3- | awk '$3 !~/\[/ { print $1 }' ```
- which files have unresolved merge conflicts
  - ``` git diff --name-only --diff-filter=U ```

### Housekeeping
- get rid of branches which are referenced locally but no longer exist on origin
  - ``` git remote prune origin ```
- fetch all remote branches, including new branches, and get rid of local references to branches that no longer exist on origin
  - ``` git fetch -p ```

### Specific Changes
#### Search Lite
- when was this line of code changed, and by whom
  - ``` git blame filename```
- what was the context of that change:
  - commit message and full diff for the commit show in git blame
    - ``` git log commit-sha --patch```
#### The Pickaxe
- ```git log -S "search_string_eg_method_name" --patch --reverse```
- commit message where the quoted bit of code changed
- full diff
- reverse chronological order so first change is at the top

### Which commit broke this
- ``` git bisect start ```
