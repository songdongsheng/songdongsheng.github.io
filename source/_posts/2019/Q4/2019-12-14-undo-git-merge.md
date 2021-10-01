---
title: Undo a Git merge that hasn't been pushed yet
excerpt: Undo a Git merge that hasn't been pushed yet
date: 2019-12-14 22:07:34
tags:
  - Git
categories: [Programming, Git]
---

# Undo a Git merge that hasn't been pushed yet

## reflog
The first step would be to use reflog to find the commit right before the merge:

```bash
$ git reflog
```

## reset
Once you find the commit that you want to revert back to, use the reset command

```bash
$ git reset --hard <commit-hash>
```

## close behind the merge

If the commit you want to get back to is only one behind HEAD, then you can instead use ORIG_HEAD as a shortcut in the reset command:

1. With modern Git, you can:

```bash
git merge --abort
```

2. Older syntax:

```bash
git reset --merge
```

3. Old-school:

```bash
$ git reset --hard

or

$ git reset --hard ORIG_HEAD
```
