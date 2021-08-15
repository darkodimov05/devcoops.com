---
layout: post
title:  "How to create empty commits in Git"
categories: [ git ]
image: assets/images/git-empty-commits.jpg
tags: [ git ]
---
As a DevOps engineer, you must have heard at least something about GitOps. GitOps is a set of practices that allows to manage infrastructure and application configurations using Git as a single source of truth. Building CI/CD pipelines is one major DevOps responsibility, which usually is triggered by a git commit. While developing, we might need to test a deployment pipeline, by pushing a "test" git commit. You really don't wanna commit empty files, hence you can create an empty commit by following the steps below.

## Prerequisites
* Git

## Solution
**Step 1**. Create an empty git commit:
```bash
git commit --allow-empty -m "Trigger test deployment"
```

**Step 2**. Push the commit to the remote repository:
```bash
git push
```

## Conclusion
Empty git commits will show up in your history, so you could either squash or remove them, which will be a topic about in a future post.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.