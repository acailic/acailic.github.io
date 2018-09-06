---
title: OCP Review 7 - Concurrency
layout: post
tags: [java, ocp, threads]
date: 2018-09-06
---

# Concurrency

## Locks:

- `Lock` and `ReadWriteLock` are interface.
- `ReentrantLock` and `ReentrantReadWriteLock` are concrete classes.
- Lock objects offer multiple advantages over the implicit locking of an object’s monitor. Unlike an implicit lock, a thread can use explicit lock objects to wait to acquire a lock until a time duration elapses.
- Method `lock()` acquires a lock on a `Lock` object. If the lock is not available, it waits until the lock can be acquired.
- Call `unlock` on a Lock object to release its lock when you no longer need it.
-  The `ReadWriteLock` interface doesn’t extend Lock or any other interface.

## Parallel fork/join framework:

- Method `compute()` determines whether the task is small enough to be executed or if it needs to be divided into multiple tasks.
- If the task needs to be split, a new `RecursiveAction` or `RecursiveTask` object is created, calling fork on it.
- Calling `join` on these tasks returns their result.




# Links 

- [Creating Threads with the ExecutorService](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/CreatingThreadsWithTheExecutorService.java) <br />
- [Identifying Threading Problems](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/IdentifyingThreadingProblems.java) <br />
- [Introducing Threads](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/IntroducingThreads.java) <br />
- [Managing Concurrent Processes](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/ManagingConcurrentProcesses.java) <br />
- [Synchronizing Data Access](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/SynchronizingDataAccess.java) <br />
- [Using Concurrent Collections](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/UsingConcurrentCollections.java) <br />
- [Working with Parallel Streams](https://github.com/acailic/java8-learning/blob/master/Java-8/src/concurrency/WorkingWithParallelStreams.java) <br />