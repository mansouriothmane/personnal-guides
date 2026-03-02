# Saving Work When Switching Branches

## Git Stash

Temporarily shelve uncommitted changes:

```bash
git stash push -m "descriptive name for your work"
```

**Tips**:
- `git stash list`
- `git stash pop` # restore latest stash (and drop it)
- `git stash apply stash@{0}` # restore without dropping
- `git stash push -u` # stash **untracked** files as well

## Commit a WIP (Work In Progress)

Commit your changes even if incomplete, then amend later:

```bash
git add -A
git commit -m "WIP: implementing X feature"
# switch branches, do work, come back
git commit --amend # rewrite the last commit when ready
```

## Create a Throwaway Branch

```bash
git switch -c wip/feature-name   # save current state as a branch
git add -A && git commit -m "WIP"
git switch dev                  # now safely switch
# later...
git switch wip/feature-name
git reset HEAD~1                 # undo the WIP commit, keeping changes
```

## Worktrees (most powerful, underused)

Check out multiple branches simultaneously in separate directories:

```bash
git worktree add ../hermes-hotfix
# or create a new branch
git worktree add -b hotfix/urgent-bug ../hermes-hotfix main
cd ../hermes-hotfix   # work on hotfix here
cd ../hermes          # original branch untouched here
git worktree remove ../hermes-hotfix   # clean up when done
```

## My favourites

I prefer the WIP commit approach best when the current changes are meant to be commited on the same branch later.
The Throwaway branch is good when the changes are meant to be commited in another branch. In that case, it might be a better to give it a final name instead of `wip/feature-name`.

