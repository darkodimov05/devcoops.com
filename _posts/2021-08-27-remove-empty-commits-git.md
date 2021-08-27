---
layout: post
title:  "How to remove empty commits in Git"
categories: [ git ]
image: assets/images/git-remove-empty-commits.png
tags: [ git ]
---
In the previous tutorial, we've learned [How to create empty commits in Git]({% post_url 2021-08-15-create-empty-commits-git %}){:target="_blank"}. Let's see how can we remove them as well.

## Prerequisites
* Git

## Solution
**Step 1**. Once you've created an empty commit (if not, run `git commit --allow-empty -m "Test empty commit"`), try to list empty commits only:
```bash
git rev-list HEAD | while read commitHash; do
    if [ $(git diff-tree --name-status --no-commit-id $commitHash | wc -l) -eq 0 ]; then
        echo $commitHash
    fi;
done
```

Example hash value output from the empty commit:
```bash
af4c5c59e2cbd56385a0c7103b198e54df07464f
```

**Step 2**. Remove the empty commit:
```bash
git filter-branch --commit-filter 'git_commit_non_empty_tree "$@"' -f HEAD
```

*Parameters*:  
`-filter-branch` = allows you to rewrite branches.  
`-commit-filter` = filter for performing the commit.  
`git_commit_non_empty_tree "$@"` = including this parameter with *-commit-filter* will skip commits that leave the tree untouched. Tree hash stays the same if no files are changed, hence the empty commit.  
`-f` = short for `--force`.

**Step 3**. Confirm the empty commit removal, by executing the command in `Step 1` again.

## Conclusion
If something goes wrong, just `rm -rf` the repository and clone it again (**just kidding**).  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.