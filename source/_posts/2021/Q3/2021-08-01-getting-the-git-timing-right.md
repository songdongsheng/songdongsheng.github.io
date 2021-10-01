---
title: Getting the Git Timing Right
excerpt: Getting the Git Timing Right
date: 2021-08-01 14:13:30
tags:
    - Programming
    - Git
categories: [Programming, Git]
---

# Getting the Git Timing Right

## Introduction

Do you work on a development team that is distributed across several time zones? Got confused by the dates that Git shows in the commit logs?

## Commit timestamps

Let’s assume that two developers work in the same Git repository. The first developer Prasad is located in Bangalore, India. His colleague Joe is located in San Diego, US. The Git log they created looks as follows:

```console
$ git log
commit 42eae2e49371be294911ad094d9644d2f58059fc (HEAD -> main)
Author: Joe Smith <jsmith@sandiego.us>
Date:   2016-12-06 11:41:44 -0800

    Commit 3

commit 6d8ee6115eaa5ce17b4217d4fedb7a98eb999a1c
Author: Prasad Gupta <pgupta@bangalore.in>
Date:   2016-12-06 21:45:51 +0530

    Commit 2

commit 7c9ae27bdb381e73010a0744fd10e979094810ef
Author: Prasad Gupta <pgupta@bangalore.in>
Date:   2016-12-06 21:45:00 +0530

    Commit 1
```

What might look a little bit odd is the order of the commits in the logs, since they are in the different time zones.

## Use iso-local date format

### git log with parameter date

```shell
$ git log --date=iso-local
commit 42eae2e49371be294911ad094d9644d2f58059fc (HEAD -> main)
Author: Joe Smith <jsmith@sandiego.us>
Date:   2016-12-07 03:41:44 +0800

    Commit 3

commit 6d8ee6115eaa5ce17b4217d4fedb7a98eb999a1c
Author: Prasad Gupta <pgupta@bangalore.in>
Date:   2016-12-07 00:15:51 +0800

    Commit 2

commit 7c9ae27bdb381e73010a0744fd10e979094810ef
Author: Prasad Gupta <pgupta@bangalore.in>
Date:   2016-12-07 00:15:00 +0800

    Commit 1
```

### Set log parameter date with git config

```shell
$ git config --global --get log.date
$ git config --global --add log.date iso-local
$ git config --global --get log.date
iso-local
```

## Commit with a UTC Timestamp

If you find it rather unnecessary to include your local timezone in your commits, and would like to commit in UTC time for example, you have two options.

### Changing your timezone before doing a commit

```shell
$ date; TZ=Tunis date
Tue Aug 1 12:17:30 CST 2021
Tue Aug 1 04:17:30 Tunis 2021

$ date >> 1
$ TZ=Tunis git commit -avm "demo commit"
[main 931d268] demo commit

$ date >> 1
$ git commit -avm "demo commit 2"
[main 616fa14] demo commit 2
 1 file changed, 1 insertion(+)

$ git log --date=iso
commit 616fa1499ebf757831e7476bf8af8d9d7bb282f5 (HEAD -> main)
Author: Joe Smith <jsmith@sandiego.us>
Date:   2021-08-01 12:19:41 +0800

    demo commit 2

commit 931d268aba2f8fb190ddff01586b189d178f2bd3
Author: Joe Smith <jsmith@sandiego.us>
Date:   2021-08-01 04:19:18 +0000

    demo commit
```

### Using the parameter date to override the date in the commit

```shell

$ date >> 1
$ git commit --date="$(date --utc +%Y-%m-%dT%H:%M:%S%z)" -avm "demo commit 3"
[main f5a45b0] demo commit 3
 Date: Tue Aug 1 04:24:23 2021 +0000
 1 file changed, 1 insertion(+)

$ git log -1 --date=iso
commit f5a45b0f74bb2ab29057a7ab62e2f3cff66c46dd (HEAD -> main)
Author: Joe Smith <jsmith@sandiego.us>
Date:   2021-08-01 04:24:23 +0000

    demo commit 3
```
