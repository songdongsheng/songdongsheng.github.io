---
title: 并发编程概述
description: 并发编程领域可以抽象成三个核心问题：分工、同步和互斥
date: 2018-11-17 18:46:49
tags:
  - Programming
  - Java
  - Concurrent
categories: [Programming, Java]
permalink: concurrent-programming
---

# 并发编程
并发编程领域可以抽象成三个核心问题：分工、协作和互斥。

![并发编程全景图](concurrent-programming.png)

## 分工
+ ThreadPool/Executor
+ ForkJoinPool/ForkJoinTask
+ ScheduledThreadPoolExecutor
+ Guarded Suspension (wait/notify)
+ Balking pattern (reject)
+ Messaging design pattern

## 协作

+ Semaphore
+ monitor
    - Lock
        + ReadWriteLock
    - Condition
    - synchronized
+ CountDownLatch
+ CyclicBarrier
+ Phaser
+ Exchanger

## 互斥
### 无锁
+ local variables
+ immutable data structures
+ TLS
+ Copy-on-Write
+ CAS
+ atomic package

### 互斥锁
+ synchronized
+ Lock
+ ReadWriteLock
