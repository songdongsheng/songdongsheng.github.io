---
title: Performance of ZooKeeper Lock
description: Performance of ZooKeeper Lock
date: 2020-03-28 21:33:23
tags:
  - Programming
  - Java
categories: [Programming, Java]
permalink: performance-of-zookeeper-lock
---

# Performance of ZooKeeper Lock

## ZooKeeper Locks

[Fully distributed locks](http://zookeeper.apache.org/doc/current/recipes.html#sc_recipes_Locks) that are globally synchronous, meaning at any snapshot in time no two clients think they hold the same lock. These can be implemented using ZooKeeeper. As with priority queues, first define a lock node.

Note:
> There now exists a Lock implementation in ZooKeeper recipes directory. This is distributed with the release -- zookeeper-recipes/zookeeper-recipes-lock directory of the release artifact.

Clients wishing to obtain a lock do the following:

1. Call create( ) with a pathname of "locknode/guid-lock-" and the sequence and ephemeral flags set. The guid is needed in case the create() result is missed. See the note below.
2. Call getChildren( ) on the lock node without setting the watch flag (this is important to avoid the herd effect).
3. If the pathname created in step 1 has the lowest sequence number suffix, the client has the lock and the client exits the protocol.
4. The client calls exists( ) with the watch flag set on the path in the lock directory with the next lowest sequence number.
5. if exists( ) returns null, go to step 2. Otherwise, wait for a notification for the pathname from the previous step before going to step 2.

The unlock protocol is very simple: clients wishing to release a lock simply delete the node they created in step 1.

Here are a few things to notice:

- The removal of a node will only cause one client to wake up since each node is watched by exactly one client. In this way, you avoid the herd effect.
- There is no polling or timeouts.
- Because of the way you implement locking, it is easy to see the amount of lock contention, break locks, debug locking problems, etc.

Recoverable Errors and the GUID

- If a recoverable error occurs calling create() the client should call getChildren() and check for a node containing the guid used in the path name. This handles the case (noted above) of the create() succeeding on the server but the server crashing before returning the name of the new node.

## Java Classes

### DistributedLock

```java
public interface DistributedLock {
    void lock() throws LockingException;

    void unlock() throws LockingException;

    class LockingException extends RuntimeException {
        public LockingException(String msg, Exception e) {
            super(msg, e);
        }

        public LockingException(String msg) {
            super(msg);
        }
    }
}
```

### ZooKeeperLock

```java
public class ZooKeeperLock implements DistributedLock, Watcher {
    public ZooKeeperLock(ZooKeeper zooKeeper, List<ACL> acl, String lockerPath) throws KeeperException, InterruptedException {
    }

    static void ensurePath(ZooKeeper zooKeeper, String path) throws KeeperException, InterruptedException {
    }

    @Override
    public synchronized void lock() throws LockingException {
    }

    @Override
    public synchronized void unlock() throws LockingException {
    }

    @Override
    public void process(WatchedEvent event) {
    }
}

```

## Performance Tests

ZooKeeper locks is network and disk heavy program, network throughput is 33 Kpkt/s at 2300 TPS. Windows is much slower than Linux in such case.

|Disk type| Windows | Linux
----------|---------|---------
|HDD SATA |  30 TPS |   45 TPS
|SSD SATA | 110 TPS |  400 TPS
|SSD NVME |       - | 1300 TPS
|TMPFS    |       - | 2300 TPS
