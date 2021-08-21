---
layout: post
title:  "How to list and count commits in Git"
categories: [ git ]
image: assets/images/git-list-count-commits.png
tags: [ git ]
---
As part of the [Git](https://devcoops.com/categories/#git){:target="_blank"} series, today we are going to see how can we list and count empty and non empty commits.

## Prerequisites
* Git

## List all commits
There are a few ways to do it:
```bash
git log --reflog
git log --oneline
git log --name-only
```

## List empty commits only
```bash
git rev-list HEAD | while read commitHash; do
    if [ $(git diff-tree --name-status --no-commit-id $commitHash | wc -l) -eq 0 ]; then
        echo $commitHash
    fi;
done
```

## List non empty commits only
```bash
git rev-list HEAD | while read commitHash; do
    git diff-tree --name-status $commitHash
done
```

## Commit count across all branches
```bash
git rev-list --all --count
```

## Commit count for a revision (`HEAD`, `master`, a commit hash)
```bash
git rev-list --count <revision>
```

## Count empty commits
```bash
git rev-list HEAD | while read commitHash; do
    if [ $(git diff-tree --name-status --no-commit-id $commitHash | wc -l) -eq 0 ]; then
        echo '1'
    fi;
done | wc -l
```

## Count non empty commits
```bash
git rev-list HEAD | while read commitHash; do
    if [ $(git diff-tree --name-status --no-commit-id $commitHash | wc -l) -gt 0 ]; then
        echo '1'
    fi;
done | wc -l
```

## Conclusion
You could do the same via the GitHub API as well.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.