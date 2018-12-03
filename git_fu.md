# Git Fundi

## Setup
### User name and email
- ```git config --global user.name “Your Name”```
- ```git config --global user.email “your@email.address”```
- if you do not want to have your private email address visible on github
  - log into github
    - go to Personal Settings -> Emails
    - tick / enable: keep my email address private  
    - take a note of the generated email address
  - set your global git config user.email to the generated email address
- if you want a different name and/or email in a specific git repo
  - ```git config user.name "Local Name"```  
  - ```git config user.email "local@email.address"```  
- if you need two different github accounts on the same machine
  - e.g. personal & work
  - https://medium.freecodecamp.org/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca

### Editor preferences
- by default, git will use a terminal-based editor like vim when opening files
- to change this to, e.g. Atom
  - ```git config --global core.editor "atom --wait"```

### Display preferences
- make the full diff visible when writing a commit message
  - ```git config --global commit.verbose true```
- use colours
  - ```git config --global color.ui auto```

### Aliasing
- you can add keyboard shortcuts by editing your global ~/.gitconfig file
  ```
  [alias]
  name = long version of git command
  ```
- you can also add them directly from the commandline
  ```
  git config --global alias.c commit
  ```
- you can specify repo-specific aliases as well by editing .git/config in the repo, or from the commandline as before
  ```
  git config alias.f 'fetch origin'
  ```
- prefix the name of the shortcut with ```git``` to call it from the commandline

#### Some of my favourite aliases
- checkout the last branch I was on
  - ```last = checkout @{-1}```
- show me the diff between staged files and the last commit
  - ```dc = diff --cached```
- show me a short version of the logs with colour highlighting and without any merges
  - ```sl = log --no-merges --format=\"%Cred%ad%Creset %Cblue%an%Creset %C(yellow)%h%Creset %Cgreen%s%Creset%n%b\"```
- show me a short version of the logs including merges
  - ```sl-wm =  log --format=\"%Cred%ad%Creset %Cblue%an%Creset %C(yellow)%h%Creset %Cgreen%s%Creset%n%b\"```
- push my changes to the remote unless someone else has changed the files
  - ```force = push --force-with-lease```
- add the changes I've staged to the last commit without any other changes
  - ```fix = commit --amend --no-edit```  


## Writing (and ReWriting) Git History

### Why? someone will need to read it...
- Don't assume you'll always have access to the trello card and board
  - what if we move to another project management tool?
- when writing a commit message, make sure you include the why in the body.
  - consider not using ``` git commit -m ``` as the commandline isn't good for structuring text
  - use ``` git commit ``` and then use the default editor
- suggested commit structure
  -
  ```
  "AccountName : Purpose/Result of Change (50 characters)
  <blank line> (essential)
  Explanation in as many or as few words as you need
  Wrap at 72 characters
  Put things you would normally add in code comments here
  Consider adding any tracking reference for context"
  ```
- using vim with the correct highlighting and plugins makes this easy
  - your vim will probably come with a defaults.vim file in /usr/local/share/vim/vim81 (mac) with all of this setup already
  - ```syntax on``` means the text will change colour when your commit message title goes over 50 characters
  - ```filetype indent plugin on``` means vim will autowrap at whatever textwidth is set to (72-80 is commonplace)
  - check that wrapping and indents is enabled in vim by typing ```:filetype``` inside your vim editor
    - you want
    ```
    filetype detection:ON plugin:ON indent:ON
    ```
  - if this isn't happening, you may need to edit your ~/.vimrc (but usually it isn't necessary!)  

- it is now possible to write a commit with multiple authors on github
  - useful for sharing credit/blame when pairing or mobbing
  -
  ```
  "Title
  <blank line>
  Body
  <blank line>
  <blank line>
  Co-authored-by: name <github@registered.email.address>
  Co-authored-by: name <github@registered.email.address>"
  ```
  - https://help.github.com/articles/creating-a-commit-with-multiple-authors/

### Rewriting history
- treat your commits as mutable until they get merged into master
  - unless you are collaborating on one branch!

#### How to do it
- return all changes in a commit to the current working directory
  - ``` git reset HEAD~ ```
- remove untracked files from the working directory
  - test first
  - ``` git clean -n ```
  - then do it for real
  - ``` git clean -f ```
  - there are also ways to remove directories
- delete the last commit including all the code changes
  - ``` git reset --hard HEAD~ ```
- add changes that should have been in the last commit without changing the commit message
  - ``` git add filename_of_changed_file```
  - ``` git commit --amend --no-edit```
- fine-grained commit control
  - add changes you have made to a single file in several separate commits
    - ``` git add --patch filename```
- incorporating changes from another branch
  - pull changes from one branch into your own, file by file
    - ``` git checkout source-branch-name file-to-checkout ```
  - pull changes from a specific commit into your own branch, file by file
    - ``` git checkout source-sha file-to-checkout ```
  - pull changes into your branch, commit by commit
    - ``` git cherry-pick sha-of-desired-commit ```
- moving changes from the current live situation to anywhere else
  - ``` git stash```
  - ``` git checkout commit where changes belong```
  - ``` git stash apply``` (leaves changes in the stash for future use - use pop instead of apply to remove)
- merge a commit into the last one, interactively (see Interactive Rebase below)
  - ``` git rebase -i HEAD~```
- abort the current rebase and return to the original code status
  - ``` git rebase --abort```

#### Interactive Rebase
- *CARE*
  - if you rebase you will need to force push
  - only do this to your own branch, not a shared branch
    - ``` git push --force-with-lease```
    - warns if there are any unexpected changes on the upstream branch
- selecting/changing/etc commits
  - drop: exclude this commit from the rebase (deleting the line also works)
  - pick: include this commit in the rebase
  - fixup: merge this commit into the previous commit and discard this commit message
  - squash: merge this commit into the previous commit, but keep both commit messages
  - reword: include this commit in the rebase, and edit the commit message
  - edit: stop after this commit has been applied, edit the files, commit, and continue
  - exec: execute the supplied shell script
- if there is a conflict, the rebase will stop to allow you to resolve the conflict
  - because of the way rebase replays your commits you may end up having to resolve the same conflict many times
  - don't panic  
- to continue a rebase
  - ``` git rebase --continue```
- you can stash, run tests, etc during an interactive rebase session...

### Merging <!-- TODO: add more here-->
- abort a current merge
  - ``` git merge --abort```
- useful merge commit message
  - ``` git merge -m 'message here'```  

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

### Your Local Changes
- unstaged changes since the last commit
  - ``` git diff ```
- staged changes since the last commit
  - ``` git diff --cached```
- to see unicode characters inline (like emojis)
  - ```git --no-pager diff```  

#### second-order-diff
  -- Tom Moertel
- use case  
  - when you're doing a global find/replace with a regex
  - you need to keep rerunning the script until you have found all the edge cases
  -
  ```bash
  git status
  run script
  git diff
  git stash # your reviewed diff
    # current branch is now back to original state before any changes
  run altered script
  git diff stash{0}
    # compare the current branch to the reviewed diff
  git stash # your new reviewed diff
    # current branch is now back to original state before any changes
  rerun script
  git diff stash{0}
  git stash
  rerun
  ```  

### Finding Specific Changes

#### Search Lite
- when was this line of code changed, and by whom
  - most modern IDEs will include this functionality
  - ``` git blame filename```
- what was the context of that change:
  - commit message and full diff for the commit show in git blame
    - ``` git log commit-sha --patch```

#### The Pickaxe
- ```git log -S "search_string_eg_method_name" --patch --reverse```
- commit message where the quoted bit of code first changed
- full diff
- reverse chronological order so first change is at the top
- tends to be less successful on a large codebase with many duplicated names

### Which commit broke this
- ``` git bisect start ```

## Git Housekeeping
- get rid of branches which are referenced locally but no longer exist on origin
  - ``` git remote prune origin ```
- fetch all remote branches, including new branches, and get rid of local references to branches that no longer exist on origin
  - ``` git fetch -p ```
- check what's being ignored by git
  - useful if you're sending a full copy of your local files somewhere
  - you really want to be sure there are no secrets about to be shared
  - ``` git status --ignored ```
- temporarily git ignore a file
  - ``` git update-index --assume-unchanged path_to_file```
  - and undo that
  - ``` git update-index --assume-no-unchanged path_to_file```


## Safe Git Practice
- https://github.com/Gazler/githug

## Github shortcuts
- I'm sure gitlab and bitbucket have similar
### canonical url
- when you are viewing a file/tree/whatever on github, press 'y' and the link will change to reference the precise SHA rather than the link via master
- even if the branch changes, your link will still take you back to the precise page/commit

### dealing with pull requests
- github stores all pull requests in your repo
- even if the fork is deleted you can still track the code
- ``` git fetch origin pull/id/head:name```
  - fetches the pull request with id 12 into a local branch named 'pr'
  - much simpler than setting up the remote etc etc etc
  - consider aliasing this for a ci process

### quote into replies
- highlight text, hit 'r'

### find a file in a repo
- hit 't' then type for a fuzzy search for a filename
- really useful for any language or frameworkd with many nested directories

## Sources
- https://services.github.com/on-demand/downloads/github-git-cheat-sheet/
- https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config
- http://www.pauline-vos.nl/git-legit-cheatsheet/
- https://brightonruby.com/2018/a-branch-in-time-tekin-suleyman/
- https://www.raywenderlich.com/2288-git-tutorial-git-fu-with-the-command-line
- https://railsware.com/blog/2014/08/11/git-housekeeping-tutorial-clean-up-outdated-branches-in-local-and-remote-repositories/
- https://medium.com/@gabriellamedas/git-rebase-and-git-rebase-onto-a6a3f83f9cce
- https://medium.com/@eclectusmedia/a-little-git-fu-for-the-budding-young-grasshopper-programmer-ecf2873fc85d
- https://git.wtf/
- https://medium.freecodecamp.org/github-privacy-101-how-to-remove-personal-emails-from-your-public-repos-58347b06a508
- https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config
- https://medium.freecodecamp.org/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca
- https://help.github.com/articles/creating-a-commit-with-multiple-authors/
- https://vimeo.com/72955426



### Future Study Material
- https://medium.freecodecamp.org/github-extensions-to-boost-your-productivity-4692ad2b1796
- https://medium.freecodecamp.org/4-steps-to-build-an-automated-testing-pipeline-with-gitlab-ci-24ccab95535e
- https://medium.freecodecamp.org/improve-development-workflow-of-your-team-with-githooks-9cda15377c3b
- https://medium.freecodecamp.org/useful-tricks-you-might-not-know-about-git-stash-e8a9490f0a1a
- https://github.com/github/hub
