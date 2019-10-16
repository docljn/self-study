# Git reflog

## Whaaaa

- aka "reference log"
- records updates applied to the tips of branches

## Why

- you can go back to a commit even if it's not referenced by any branch or tag

### When

- you rebased, and instead of `f` used `d` when selecting commits
  - and you've force-pushed to the remote
- your push failed for some reason, and you didn't notice
  - you've deleted the local branch
- the work in that commit took ages to write, in a pair, and now it's gone forever

### Panic

- there's nothing in the git log that points to your lost commit
```bash
> git log
```

```git
2019-10-16 7c63af88a5e Commit Message
2019-10-16 dc019fdc146 EcSupply should now belong to a Company (#20227)
```

## Actually, don't panic

```bash
> git reflog
```

- this is shorthand for `git reflog show HEAD`
- which itself is an alias for `git log -g --abbrev-commit --pretty=oneline HEAD`

```git
c63af88a5e (HEAD -> my_branch, origin/my_branch) HEAD@{0}: commit (amend): Commit Message
d556849e0bc HEAD@{1}: rebase -i (finish): returning to refs/heads/my_branch
d556849e0bc HEAD@{2}: rebase -i (drop): Commit Message
ba2070b6961 HEAD@{3}: rebase -i (start): checkout fa5fbc438ad
acfb5062628 HEAD@{4}: commit (amend): Another Commit Message
deb3bfb15ae HEAD@{5}: commit: Another Commit Message
ba2070b6961 HEAD@{6}: commit: Commit Message
fa5fbc438ad (tag: jenkins-pass-2019-10-16T09-05-06Z, tag: deploy-production-2019-10-16T09-11-57Z, master) HEAD@{7}: reset: moving to fa5fbc438ad
ce8decdc476 HEAD@{8}: commit: Yet Another Commit Message
fa5fbc438ad (tag: jenkins-pass-2019-10-16T09-05-06Z, tag: deploy-production-2019-10-16T09-11-57Z, master) HEAD@{9}: checkout: moving from master to my_branch
```

### Reading a reflog

`deb3bfb15ae HEAD@{5}: commit (amend): Another Commit Message`

- SHA `deb3bfb15ae`
- pointer / reference `HEAD@{5}`
- action (sub action) `commit (amend)`
- commit message if it's a commit, explanation if it isn't `Another Commit Message`

### And breathe

- you can reset to any point in the reflog by passing in the SHA or the pointer/reference

- hard reset (and lose any work after that change)

```bash
> git reset --hard deb3bfb15ae
```

OR

- soft reset (and keep subsequent changes in your working tree)

```bash
> git reset HEAD@{5}
```

## But the commit still isn't there

- HEAD references your currently active branch, so if `git reflog` doesn't return what you need, it might be somewhere else...

- if you know which branch it should be on, try

```bash
> git reflog show <otherbranch>
```

- if you think you might have stashed it, try

```bash
> git reflog stash
```

- if you've no idea where the code that's gone missing might be, try

```bash
> git reflog show --all
```

## Other uses

### Timed diffs

```bash
>  git diff master@{0} master@{1.day.2.hours.ago}
```

### Pointer-based diffs

```bash
> git diff stash@{0} otherbranch@{0}
```

## When panicking might just be appropriate

```bash
> git reflog expire
```

- usually, you won't run this, but git does internally
- this cleans up old/unreachable reflog entries
- by default, git sets the expiry to 90 days

```bash
> git reflog delete <REFLOG_ENTRY>
```

- as above: just don't

### helpful sources

https://git-scm.com/docs/git-reflog

https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog

https://ohshitgit.com