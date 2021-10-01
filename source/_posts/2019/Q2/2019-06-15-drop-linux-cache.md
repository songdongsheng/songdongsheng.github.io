---
title: Drop Linux Cache
excerpt: Drop Linux Cache
date: 2019-06-15 22:50:08
tags:
  - Linux
categories: [Operating system, Linux]
---

# Drop Linux Cache

```bash
#!/bin/bash

/bin/sync

# Free pagecache
# echo 1 > /proc/sys/vm/drop_caches
# sysctl vm.drop_caches=1

# Free reclaimable slab objects (includes dentries and inodes)
# echo 2 > /proc/sys/vm/drop_caches
# sysctl vm.drop_caches=2

# Free both page cache and slab objects
echo 3 > /proc/sys/vm/drop_caches
sysctl vm.drop_caches=3

# Disable drop cache logs, you will not be able to enable the logs again unless the system is restarted.
# https://github.com/torvalds/linux/blob/master/fs/drop_caches.c
# stfu |= sysctl_drop_caches & 4;

# echo 4 > /proc/sys/vm/drop_caches
# sysctl vm.drop_caches=4

/bin/sync
```
