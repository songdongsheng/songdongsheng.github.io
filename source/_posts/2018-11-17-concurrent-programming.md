---
title: ������̸���
description: �������������Գ���������������⣺�ֹ���ͬ���ͻ���
date: 2018-11-17 18:46:49
tags:
  - concurrent
  - programming
categories: [���, ����]
permalink: concurrent-programming
---

# �������
�������������Գ���������������⣺�ֹ���Э���ͻ��⡣

![�������ȫ��ͼ](concurrent-programming/concurrent-programming.png)

## �ֹ�
+ ThreadPool/Executor
+ ForkJoinPool/ForkJoinTask 
+ ScheduledThreadPoolExecutor
+ Guarded Suspension (wait/notify)
+ Balking pattern (reject)
+ Messaging design pattern

## Э��

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

## ����
### ����
+ local variables
+ immutable data structures
+ TLS
+ Copy-on-Write
+ CAS
+ atomic package

### ������
+ synchronized
+ Lock
+ ReadWriteLock
