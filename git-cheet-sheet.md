<h1>GIT CHEET SHEET</h1>

To reset last local commit

```sh
  git reset --hard HEAD~1
```

To remove remote branch - (In most cases, [remote_name] will be origin)

```sh
  git push -d [remote_name] [branch_name]
```

To remove local branch - (first, checkout another branch)

```sh
  git branch -D [branch_name]
```

Show modified files in working directory, staged for your next commit

```sh
  git status
```

Add a file as it looks now to your next commit (stage)

```sh
  git add [file]
```

Add all files as it looks now to your next commit (stage)

```sh
  git add -A
```

Unstage a file while retaining the changes in working directory

```sh
git reset [file]
```

Diff of what is changed but not staged

```sh
  git diff
```

Diff of what is staged but not yet commited

```sh
  git diff --staged
```

Commit your staged content as a new commit snapshot

```sh
  git commit -m “[descriptive message]”
```

List your branches. a \* will appear next to the currently active branch

```sh
  git branch
```

Create a new branch at the current commit

```sh
  git branch [branch-name]
```

Create and check out a new branch

```sh
  git checkout -b [branch-name]
```

Switch to another branch and check it out into your working directory

```sh
  git checkout [branch-name]
```

Merge the specified branch’s history into the current one

```sh
  git merge [branch]
```

Show all commits in the current branch’s history

```sh
  git log
```

Save modified and staged changes

```sh
  git stash
```

List stack-order of stashed file changes

```sh
  git stash list
```

Write working from top of stash stack

```sh
  git stash pop
```

Discard the changes from top of stash stack

```sh
  git stash drop
```
