---
title: How to flush output of Python print function
description: How to flush output of Python print function
date: 2020-01-12 23:27:26
tags:
  - Python
categories: [Programming, Python]
permalink: python-print-function-flush
---

# How to flush output of Python print function

## take an optional flush argument

```python
print("Hello world!", flush=True)
```

## overwrite print function with default set to flush = True

```python
def print(*objects, sep=' ', end='\n', file=sys.stdout, flush=True):
    __builtins__.print(*objects, sep=sep, end=end, file=file, flush=flush)
```

## lambda function

```python
message = lambda x: print(x, flush=True, end="")
message('I am flushing out now...')
```
