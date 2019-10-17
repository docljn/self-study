# Git rerere

## Whaaaa

- aka "reuse recorded resolution"

## Why

- git will remember how you’ve resolved a conflict
- the next time it sees the same conflict, git can resolve it for you automatically

## When

- You've got a long-lived feature branch
  - you don't want a whole lot of intermediate merge commits from master
  - you do want an easy final merge with master

- You prefer to rebase your long-lived branch onto master
  - you don't want to deal with the same rebasing conflicts each time you rebase

- You have a whole set of evolving feature branch which need to be occasionally merged into a testable head
  - if the tests fail, you can rewind the merges and re-do them without the branch that made the tests fail
  - you really don't want to have to re-resolve the conflicts each time you do this

## How

### Enable rerere

```bash
> git config --local rerere.enabled true
```

### Resolve your conflicts

merge a branch into your branch that causes conflicts

- fix one conflict, and save the file
- `git rerere diff` shows what git will remember as the solution for that conflict
- `git add <fixed file>`
- `git rerere remaining` will show any remaining conflicted files
- repeat the `fix, save, diff, add, remaining` cycle until all conflicts are fixed
- `git commit -m 'merge commit message`

### Roll back the commit

- you don't want to pollute your branch history! `git reset --hard HEAD^`

### Merge in again

Git remembers the resolution you used for this specific conflict

## Gotchas

### Automatic merge failed

You will still see `Automatic merge failed` in the logs when you merge in a conflicting branch

- This doesn't mean the merge has failed!
- Run `git rerere remaining` to see if there are any unresolved conflicts, and continue as above

### Staging isn't automatic

You will still need to stage any auto-fixed conflicts, so you have the option to confirm that git has done what you wanted
If you want git to autostage solved conflicts, use `> git config --local rerere.autoupdate true`

### Your rerere history doesn't last forever

Unresolved conflicts older than 15 days and resolved conflicts older than 60 days are pruned

## Downsides

- does add overhead in terms of storage etc
- generally advised not to enable globally, but YMMV

## Useful links

https://git-scm.com/docs/git-rerere

https://git-scm.com/book/en/v2/Git-Tools-Rerere

https://medium.com/@porteneuve/fix-conflicts-only-once-with-git-rerere-7d116b2cec67

https://www.youtube.com/watch?v=duqBHik7nRo



