---
title: Remove blank element from json
description: Remove blank (null or empty) element from json via jq
date: 2019-05-25 23:54:57
tags:
  - JSON
categories: [Programming, JSON]
permalink: remove-blank-element-from-json
---

# Remove blank element from json

```
$ cat ~/.jq

def remove_empty:
  . | walk(
    if type == "object" then
      with_entries(
        select(
          .value != null and
          .value != "" and
          .value != [] and
          .value != {}
        )
      )
    else .
    end
  );
```
