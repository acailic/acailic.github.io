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


---

## Threads

- _light processes_
    - each Thread has its own PC & Stack
    - they share the heap
    - can access shared objects in memory
- process: bunch of Threads
    - single-threaded process
    - multi-threaded process

---

## Concurrency vs Parallelism

- Concurrency: executing multiple Threads & Processes
    - can be done with a single CPU & Scheduling (Round-Robin)
- Parallelism: we have multiple CPUs to execute in parallel

---

## Thread scheduler

- used by the OS to execute more threads than CPUs at the same time
- Round-robin is not the only one
- This makes Thread behave like they behave

---

## Types of Threads

- System
- User
- Both can be marked as Daemon Threads
    - a program finishes when there's no non-daemon threads running

---

## Threads Subclassing Thread


```java
public class HelloThread extends Thread {
    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new HelloThread()).start();
    }
}

```

---

## Threads Implementing Runnable


```java
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}

```

---

## Pausing a Thread

```java 
Thread.sleep(4000);

throws InterruptedException
```
---

## Good idiom (pre Java 8)


```java
new Thread(new Runnable() {
    public void run() {
        yourNetworkStuff();
    }
}).start();
```

---
## Helpers

```java
// ...

public static void dp(String text) {    // debug print
    System.out.println(text + " - " + Thread.currentThread().getName());
}

public static void waitSeconds(long seconds) {
    try {
        Thread.sleep(seconds*1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}

```

---

## Waiting for results using Threads (polling)

```java
static int counter = 0;
static boolean finished;

// ...

new Thread(() -> {
    for (int i=0; i< 100; i++) {
        counter++;
    }
}).start();

System.out.println(counter);    // don't wait for thread to finish!

```

---

## Waiting for results using Threads (polling)

```java
public class Main {
    static int counter = 0;
    static boolean finished;

    public static void main(String[] args) throws InterruptedException {
        new Thread(() -> {
           for (int i=0; i< 100; i++) {
               counter++;
           }
           finished = true;
        }).start();

        while (!finished) {
            System.out.println("Still not finished");
            Thread.sleep(1000);
        }
        System.out.println(counter);
    }
}

```

---

## Try / Finally and Threads

```java
System.out.println("Starting main");

try {
    Thread t = new Thread(() -> {
        System.out.println("Starting thread");
        System.out.println("Hibernate thread");

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Ending Thread");
    });
    t.start();
    // t.join();
} catch (InterruptedException e) {
    e.printStackTrace();
} finally {
    System.out.println("Finally finished"); // will always get executed
                                            // maybe before the thread finishes
}

System.out.println("Finishing main");

```

---

## Exam watch

- no more focus on Threads
- instead, focus on __Concurrency__ API




---

## ExecutorService

- in package `java.util.concurrent`
- interface

---

## Single Thread Executor

- it's a serial queue of _tasks_
- we can add tasks to that queue
- all tasks run in order, but in a background thread
- there's only ONE background thead created
- all tasks are in a queue

```java
ExecutorService service = Executors.newSingleThreadExecutor();

System.out.println("Main: start " + Thread.currentThread().getName());

service.execute(() -> {
    dp("Hello");
});

service.execute(()->{
    dp("Bye");
});

dp("Main: stop");

service.shutdown();
```

---

## Shutting down Single Thread Executor

- need to finish it
- if not, creates a non-daemon thread that executes all Runnables in turn
- this non-daemon thread inhibits us from finishing
- shutdown does not cancel already running threads

---

## Shutting down Single Thread Executor

- methods:
    - `isShutdown`
    - `isTerminated`

|   | Active | Shutting Down | Shutdown |
| --- | --- | --- | --- |
| isShutdown  | false | true | true |
| isTerminated | false | false | true |

---

## We can create more than one ExecutorService

```java
ExecutorService service = Executors.newSingleThreadExecutor();
ExecutorService service2 = Executors.newSingleThreadExecutor();

service.execute(() -> {
    dp("Hello");
});

service2.execute(() -> {
    dp("Hello service 2");
});

service2.execute(() -> {
    dp("Bye Service 2");
});

service.execute(()->{
    dp("Bye");
});
```

---

## Submit tasks to an Executor

- execute: async
- submit: async
- invokeAll: sync
- invokeAny: sync

---

## Submit vs Execute

- Submit: takes a `Runnable` or `Callable`, returns a `Future`

```java
Future<?> submit(Runnable task);
<T> Future<T> submit(Callable<T> task);
```
- Execute: takes a `Runnable`, returns nothing,

```java
void execute(Runnable command);
```
---

## Future example

```java
static Optional<String> response = Optional.empty();
// ...

Future<?> result = service.submit(() -> {
    String sURL = "http://freegeoip.net/json/"; //just a string

    // Connect to the URL using java's native library

    try {
        URL url = new URL(sURL);
        HttpURLConnection request = (HttpURLConnection) url.openConnection();
        request.connect();
        response = Optional.ofNullable(convertStreamToString((InputStream) request.getContent()));

    } catch (IOException e) {
        e.printStackTrace();
    }
});

try {
    result.get(10, TimeUnit.HOURS); // waits for 10 hours
    System.out.println(response.orElse("Noooo"));
} catch (InterruptedException | ExecutionException | TimeoutException e) {
    e.printStackTrace();
}
```

---

## Future Methods

- isDone()
- isCancelled()
- cancel()
- get(): waits endlessly
- get (long timeout, TimeUnit unit): waits some time

```

TimeUnit.NANOSECONDS
TimeUnit.MICROSECONDS
...
TimeUnit.MINUTES
TimeUnit.HOURS
...

```

---

## Callable

- Callable can return a value
- often, `V`is a `Future`
- can throw cheked exceptions

```java
@FunctionalInterface public interface Callable<V> {
    V call() throws Exception;
}
```

---

## Callable vs Runnable


```java
@FunctionalInterface public interface Callable<V> {
    V call() throws Exception;
}
```

- runnable returns nothing
- no exceptions can be thrown

```java
@FunctionalInterface public interface Runnable {
    public abstract void run();
}
```

---

## Waiting for all tasks to finish

```java
ExecutorService service = null;
try {
    service = Executors.newSingleThreadExecutor();

    service.submit(() -> {
        for (int i = 0; i < 10; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            dp("Hello " + i);
        }
    });

    service.execute(() -> {
        dp("Bye");
    });

    System.out.println("All tasks added");
} finally {
    dp("Shutting down service");
    if (service != null) {
        service.shutdown();
    }
}

// can't add more here

```

---

## If we add...

```java

System.out.println("Can't add more tasks");

service.execute(() -> { // this crashes the main thread
    dp("Nope");
});

System.out.println("Terminated: " + service.isTerminated());    // don't get printed!

```

---

## Awaiting termination

```java
System.out.println("Terminated: " + service.isTerminated());

service.awaitTermination(15, TimeUnit.SECONDS);

System.out.println("Terminated: " + service.isTerminated());

```

---

## Scheduling tasks

- Schedule for start later

```java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();

dp("Let's start scheduler");

try {
    service.schedule(() -> {
        dp("Hello scheduled!");
    }, 3, TimeUnit.SECONDS);
} finally {
    dp("About to finish it!");
    service.shutdown();
}
```

---

## Scheduling & repeating tasks

- scheduleAtFixedRate: after delay, executes, passed some time, executes (although previous could be running)
- scheduleAtFixedDelay: after delay, executes until completion, then waits a fixed delay, then executes, ...

---

## Scheduling & repeating tasks

```java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();

dp("Let's start scheduler");

try {
    service.scheduleAtFixedRate(() -> {
        dp("Hello scheduled & repeating! " + counter++);    // counter is static / instance var

        if (counter == 10) {
            service.shutdown();
        }
    }, 2000, 300, TimeUnit.MILLISECONDS);
    dp("Try finished");
} 

```

---

## Thread-pool executor

- Thead pool: pre-instantiated reusable threads 
- Single Thread executor: if running one task and another comes in, waits for completing first before launching second. Result: ordered execution of tasks in one executor
- Pooled-thread executor: new Task gets in
    - if available Threads in the pool, run _immediately_ that task (_immediately_ depending of JVM Thread scheduler)
    - if not, waits in a queue until one the pool's thread finishes

---

## Fixed Thread Pool

- pass in number of Threads to be created
- threads are reused
- if tasks > threads in pool --> tasks wait to enter in a queue

```java
ExecutorService service = null;

try {
    service = Executors.newFixedThreadPool(3);

    service.submit(() -> { waitSeconds(5); dp("Task 1"); });

    service.submit(() -> { waitSeconds(2); dp("Task 2"); });

    service.submit(() -> { waitSeconds(1); dp("Task 3"); });

    for (int i = 0; i < 10 ; i++) {
        final int j = i;
        service.submit(() -> {
            dp("Task for: " + j);
        });
    }
} finally {
    service.shutdown();
}

```

---

## Fixed Thread Pool: result

```
Task 3 - pool-1-thread-3
Task for: 0 - pool-1-thread-3   <-- waiting to reuse
Task for: 1 - pool-1-thread-3   <-- reusing thread 3
Task for: 2 - pool-1-thread-3
Task for: 3 - pool-1-thread-3
Task for: 4 - pool-1-thread-3
Task for: 5 - pool-1-thread-3
Task for: 6 - pool-1-thread-3
Task for: 7 - pool-1-thread-3
Task for: 8 - pool-1-thread-3
Task for: 9 - pool-1-thread-3
Task 2 - pool-1-thread-2
Task 1 - pool-1-thread-1

```

---

## newCachedThreadPool

```java
service = Executors.newCachedThreadPool();
```

- creates as many threads as needed
- if possible, reuse them

```
Task for: 0 - pool-1-thread-4
Task for: 1 - pool-1-thread-5
Task for: 2 - pool-1-thread-6
Task for: 3 - pool-1-thread-7
Task for: 4 - pool-1-thread-5   <-- reusing
Task for: 5 - pool-1-thread-6
Task for: 7 - pool-1-thread-7
Task for: 8 - pool-1-thread-6
Task for: 6 - pool-1-thread-5
Task for: 9 - pool-1-thread-4
Task 3 - pool-1-thread-3
Task 2 - pool-1-thread-2
Task 1 - pool-1-thread-1

```

---

## Some Runtime goodies


```java
Runtime.getRuntime().addShutdownHook(new Thread(() -> System.out.println("Closing!")));

System.out.println(Runtime.getRuntime().availableProcessors());
```

---

## Synchronizing

> Shared mutable state is the root of all evil

- problem: race conditions and `++` operator not Thread Safe

---

```java
public static int n = 0;

public static void main(String[] args) {
    ExecutorService service = null;

    try {
        service = Executors.newCachedThreadPool();

        service.submit(() -> {
            waitSeconds(5); dp("Task 1 " + n++);
        });

        service.submit(() -> { waitSeconds(2); dp("Task 2 " + n++); });

        service.submit(() -> {
            waitSeconds(1);

            n++;
            dp("Task 3 " + n);
        });

        for (int i = 0; i < 10 ; i++) {
            final int j = i;
            service.submit(() -> {
                dp("Task for: " + j + " " + n++);
            });
        }
    } finally {
        service.shutdown();
    }
}
```

---

## Synchronize blocks

- `syncronized(object)` creates a mutual-exclusion region
- lock can be any instance object in Java

---

```java
public static Integer n = new Integer(0);

public static void main(String[] args) {
    ExecutorService service = null;

    try {
        service = Executors.newCachedThreadPool();

        service.submit(() -> {
            synchronized (n) {
                waitSeconds(5); dp("Task 1 " + n++);
            }
        });

        service.submit(() -> { waitSeconds(2);
            synchronized (n) {
                dp("Task 2 " + n++);
            }
        });

        service.submit(() -> {
            waitSeconds(1);

            synchronized (n) {
                n++;
                dp("Task 3 " + n);
            }
        });

        for (int i = 0; i < 10 ; i++) {
            final int j = i;
            service.submit(() -> {
                synchronized (n) {
                    dp("Task for: " + j + " " + n++);
                }
            });
        }
    } finally {
        service.shutdown();
    }
}

```

---

## Synchronized

- in methods: instance + static
- in vars: only instance

---

## Atomic types

- can`t use `synchronize` on static vars

```java
 public static AtomicInteger n = new AtomicInteger(0);

public static void main(String[] args) {
    ExecutorService service = null;

    try {
        service = Executors.newCachedThreadPool();

        service.submit(() -> {
            waitSeconds(5);
            dp("Task 1 " + n.incrementAndGet());
        });
    }
}

```

---

## Concurrent collections

- modify the List/KeySet while iterating: crash

```java
List<String> names = new ArrayList<>();
names.add("Hello");
names.add("World");
names.add("World");
names.add("World");
names.add("World");

for (String s: names) {
    names.remove(s);        // crash
}

Map<String, Integer> data = new HashMap<>();
data.put("hello", 1);
data.put("goodbye", 2);
for (String key: data.keySet()) {
    data.remove(key);       // crash
}

```

---

## Parallel Streams

```java
 Stream<String> names = Stream.of("Gollum", "Frodo", "Bilbo", "Aragorn", "Gandalf");
names.forEach(System.out::println);

Stream<String> parallelNames = Stream.of("Gollum", "Frodo", "Bilbo", "Aragorn", "Gandalf");
parallelNames.parallel().forEach(System.out::println);
```

# Questions

-  a CyclicBarrier allows multiple threads to run independently but wait at one point until all of the coordinating threads arrive at that point. Once all the threads arrive at that point, all the threads can then proceed. It is like multiple cyclists taking different routes to reach a particular junction. They may arrive at different times but they will wait there until everyone arrives. Once everyone is there, they can go on futher independent of each other.

-  extend Thread instead of implementing Runnable and add CyclicBarrier cb = new CyclicBarrier(2, m); 
  1. ItemProcessor needs to extend Thread otherwise ip.start() will not compile. 
  2. Since there are a total two threads that are calling cb.await ( one is the ItemProcessor thread and another one is the main thread), you need to create a CyclicBarrier with number of parties parameter as 2. If you specify the number of parties parameter as 1, Merger's run will be invoke as soon as the any thread invokes await but that is not what the problem statement wants.
  
  
-  From a ReadWriteLock, you can get one read lock (by calling lock.readLock() ) and one write lock (by calling lock.writeLock() ). Even if you call these methods multiple times, the same lock is returned. A read lock can be locked by multiple threads simultaneously (by calling lock.readLock().lock() ), if the write lock is free. If the write lock is not free, a read lock cannot be locked. The write lock can be locked (by calling lock.writeLock().lock() ) only by only one thread and only when no thread already has a read lock or the write lock. In other words, if one thread is reading, other threads can read, but no thread can write. If one thread is writing, no other thread can read or write.  Methods that do not modify the collection (i.e. the threads that just "read" a collection) should acquire a read lock and threads that modify a collection should acquire a write lock.  The benefit of this approach is that multiple reader threads can run without blocking if the write lock is free. This increases performance for read only operations. 
-  java.util.concurrent package: 
  1. ExecutorService interface extends Executor interface. While Executor allows you to execute a Runnable, ExecutorService allows you to execute a Callable. 
  2. Executors is a utility class that provides several static methods to create instances of ExecutorService. All such methods start with new e.g. newSingleThreadExecutor(). You should at least remember the following methods: newFixedThreadPool(int noOfThreads), newSingleThreadExecutor(), newCachedThreadPool(), newSingleThreadScheduledExecutor(), newScheduledThreadPool(int corePoolSize).
  
- java.util.concurrent.Executor is an interface that has only one method: void execute(Runnable command)  Java concurrent library has several implementations for this interface such as ForkJoinPool, ScheduledThreadPoolExecutor, and ThreadPoolExecutor.  As instance of an Executor is created using various static factory methods of java.util.concurrent.Executors such as newFixedThreadPool, newSingleThreadExecutor, and newScheduledThreadPool.  Thus, the number of threads used by an Executor instance depends on the class of that instance and how that instance was created. For example, if an instance was created using Executors.newSingleThreadExecutor(), only one thread will be created, but if it was created using Executors.newFixedThreadPool(5), five threads will be created.   
  
-  CopyOnWriteArrayList guarantees that the Iterator acquired from its instance will never get this exception. This is made possible by creating a copy of the underlying array of the data. The Iterator is backed by this duplicate array. An implication of this is that any modifications done to the list are not reflected in the Iterator and no modifications can be done on the list using that Iterator (such as by calling iterator.remove() ). Calls that try to modify the iterator will get UnsupportedOperationException.

-  Future.get() will block until there is a value to return or there is an exception.
   Therefore, the program will block at line marked //1 and will print the return value of the given Callable i.e. "DONE" once it is done.  If you don't want to block the code, you may use Future.isDone(), which returns a boolean without blocking.  List<Runnable> shutdownNow() This method attempts to stop all actively executing tasks, halts the processing of waiting tasks, and returns a list of the tasks that were awaiting execution. This method does not wait for actively executing tasks to terminate. Use awaitTermination to do that.  There are no guarantees beyond best-effort attempts to stop processing actively executing tasks. For example, typical implementations will cancel via Thread.interrupt(), so any task that fails to respond to interrupts may never terminate.  


- Fork/Join framework: 
    1. The worker threads in the ForkJoinPool extend java.lang.Thread and are created by a factory.
    2. One worker thread may steal work from another worker thread. A ForkJoinPool differs from other kinds of ExecutorService mainly by virtue of employing work-stealing: all threads in the pool attempt to find and execute subtasks created by other active tasks (eventually blocking waiting for work if none exist). This enables efficient processing when most tasks spawn other subtasks (as do most ForkJoinTasks).
    3. The tasks submitted to a ForkJoinPool doesn't have to sit on the leaves of the task tree. it is implementation dependent.

- AtomicInteger allows you to manipulate an integer value atomically. It provides several methods for this purpose. `ai.incrementAndGet();` , `ai.incrementAndGet();` 


-  `new CopyOnWriteArrayList<>(` - Multiple threads can safely add and remove objects from sList simultaneously. A thread-safe variant of ArrayList in which all mutative operations (add, set, and so on) are implemented by making a fresh copy of the underlying array.  This is ordinarily too costly, but may be more efficient than alternatives when traversal operations vastly outnumber mutations, and is useful when you cannot or don't want to synchronize traversals, yet need to preclude interference among concurrent threads. The "snapshot" style iterator method uses a reference to the state of the array at the point that the iterator was created. This array never changes during the lifetime of the iterator, so interference is impossible and the iterator is guaranteed not to throw ConcurrentModificationException. The iterator will not reflect additions, removals, or changes to the list since the iterator was created. Element-changing operations on iterators themselves (remove, set, and add) are not supported. These methods throw UnsupportedOperationException.

- HashMap supports adding null key as well as null values but ConcurrentHashMap does not. Inserting null key or null in a ConcurrentHashMap will throw a NullPointerException.



- An important thing to know about the Iterators retrieved from a ConcurrentHashMap is that they are backed by that ConcurrentHashMap, which means any operations done on the ConcurrentHashMap instance may be reflected in the Iterator. 
Retrieval operations (including get) generally do not block, so may overlap with update operations (including put and remove). Retrievals reflect the results of the most recently completed update operations holding upon their onset.
For aggregate operations such as putAll and clear, concurrent retrievals may reflect insertion or removal of only some entries. Similarly, Iterators and Enumerations return elements reflecting the state of the hash table at some point at or since the creation of the iterator/enumeration. They do not throw ConcurrentModificationException.
However, iterators are designed to be used by only one thread at a time.  and the following is the behaviour description of the EntrySet retrieved from a ConcurrentHashMap instance using the entrySet() method:  entrySet() returns a Set view of the mappings contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa. The set supports element removal, which removes the corresponding mapping from the map, via the Iterator.remove, Set.remove, removeAll, retainAll, and clear operations. It does not support the add or addAll operations. iterator that will never throw ConcurrentModificationException, and guarantees to traverse elements as they existed upon construction of the iterator, and may (but is not guaranteed to) reflect any modifications subsequent to construction.


- a synchronized method ends up throwing an exception to the caller, the locks acquired by the method due to the usage of synchronized keyword is released automatically. The Java exception mechanism is integrated with the Java synchronization model. this does not apply to java.util.concurrent.locks.Lock. These locks must be released by the programmer explicitly.

- It has two different Thread instances (thus two distinct threads) and both are started. The fact that the same Runnable instance is used by both the threads is immaterial. Both the threads will run independently and thus the the run method of the Runnable will be invoked twice.

- THREAD: The Thread class implements the Runnable interface and is not abstract. 
          A Thread is created by doing new ClassThatExtendsThread()  OR by doing  new Thread( classImplementingRunnable); The newly created Thread is started by calling start().
          All the code that does the work in a separate thread goes in the run() method. 
          Method signature :  public void run()
          Note that this method is not Abstract in the Thread Class. So you need not necessarily override it. But the Thread class version doesn't do anything. It is just empty implementation.
           A call to start() returns immediately but before returning, it internally causes a call to the run method of either the Thread class( if the thread was created by doing new ClassThatExtendsThread() ) or of the  Runnable class ( if the thread was created by doing  new Thread( classImplementingRunnable);
           General Information:
          suspend(), resume() and stop() are deprecated methods because they don't give the Thread a chance to release shared resources, which may cause deadlocks. We do not expect any questions on these methods in the exam but it is good to know about them for interview purpose.


- Consider this situation:
  1. OS schedules thread 1. It executes m1() and then enters m2(). Now here, it acquires the lock for obj2 (first sync. stmt of m2()) and before it could get a lock for obj1, the OS preempts it.
  2. Now, OS schedules thread 2. It enters m1() and gets the lock for obj1 (first sync. stmt of m1()) . But when it tries to get the lock for obj2, it will be stuck as it is already acquired by thread 1.
  Now both the threads are waiting for locks acquired by each other. So nobody can run. So the program gets stuck. Note that the threads are not dead. They just keep waiting. So the program never ends in such a case.
  
-  deadlock might occur when multiple threads try to acquire locks on multiple objects in different sequence. Consider the following situation - 
  Thread1 tries to execute the first transfer and acquires the lock for account a1. It updates the balance of a1 but before it could acquire the lock for a2, thread2 executes and acquires the lock for account a2. Thread2 updates the balance and tries to acquire the lock of a1. It will now be stuck because a1's lock is already acquired by thread1. It will not proceed until thread1 releases a1's lock. At the same time, it will keep its own lock for a2 because it has not released it.
  Now, thread1 executes and tries to acquire the lock for a2, which is with thread2. So it cannot proceed further either.
  
- two new threads are created but none of them is started.(Remember run() does not start a thread. start() does.)
  Here, run is called but NOT of MyThread class but of Thread class. Thread class's run() is an interesting method. If the thread object was constructed using a separate Runnable object, then that Runnable object's run method is called otherwise, this method does nothing and returns. Here, Thread's run calls MyThread's run() which prints the string and returns. Everything is done in one thread (the main thread) and so the order is known.
  
  
- straight forward implementation on an thread unsafe class. Observe that count is a shared resource that is accessed by multiple threads and there are multiple issues in this code:
  
  1. Since access to count is not synchronized, there is no guarantee that changes made by thread 1 will even be visible to thread 2. Thus, both the threads may print 0 and increment it to 1 even if they run one after the other. To understand this point, you need to read about topic of visibility guarantee provided by the Java memory model.
  
  2. count++ is not an atomic operation, which means that it can be broken up into three separate steps - 1. get the current value of count in cache 2. increment the cache value and 3. update the count with updated cache value. Now, for example, while thread 1 is about to execute step 3, thread 2 may come in and execute steps 1, 2, and 3. Thread 1 then executes the remaining step 3. The result is that value of count will be 1 after both the threads are done, while it should have been 2, and thus one increment operation is lost.

- static variable and there are 2 threads that are accessing it. None of the threads has synchronized access to the variable. So there is no guarantee which thread will access it when. Further, one thread might increment i while another is executing i<5. So it is possible that more than 5 words may be printed. For example, lets say i is 0 and thread1 starts first loop. It will print Hello. Now, before thread1 could do i++, thread2 might run and it will print "World" while i is still 0. 

- threads, current thread, runnable:
 1. The statement : Thread.currentThread().setName("MainThread"); in the main() method sets the name of the current thread. This is the thread that is running the main method. 
 2. The statement: MyRunnable mr = new MyRunnable("MyRunnable"); creates a new MyRunnable object. In its constructor, it creates a new threads with the name "MyRunnable" and also starts this thread. 
 3. Now there are two threads running: The main thread (having the name "MainThread") and the MyRunnable thread (having the name "MyRunnable").
 4. The myrunnable thread executes the run method and prints "MyRunnable" because that is the name of the thread that called the run() method. On the other hand when the main method calls mr.run(), it prints "MainThread" because that is the name of the thread that has called the run method(). Therefore, both, Main and MyRunnable will be printed. Note that the order cannot be determined because the OS decides which thread to schedule when.

- synchronized keyword can be applied only to non-abstract methods that are defined in a class or a block of code appearing in a method or static or instance initialization blocks. It cannot be applied to methods in an interface.

- both the threads are trying to use the same fields of the same object. The order of execution of thread can never be told so we cannot tell which variable be used by which thread at what time. So the output cannot be determined. 
Further, since the variables x and y are not declared as volatile, the updates made by one thread may not be visible to another thread at all. It is, therefore, possible that both the threads will find x and y to be 0, 0 at the beginning and update them by 1 in each iteration.

-  there are two threads running. (The main() thread and the thread that you just started!). Both the threads are trying to use the same variable. Now, which thread will run first cannot be determined so whether the main() thread reads 'x' first or the new thread changes 'x' first is not known. So the output of the program cannot be determined.

- fork/join framework : suited for computation intensive tasks that can be broken into smaller pieces recursively.

- A ForkJoinPool differs from other kinds of ExecutorService mainly by virtue of employing work-stealing.

- public final boolean compareAndSet(int expect, int update) Atomically sets the value to the given updated value if the current value == the expected value. Parameters: expect - the expected value update - the new value Returns: true if successful. False return indicates that the actual value was not equal to the expected value.

-  ReadWriteLock, you can get one read lock (by calling lock.readLock() ) and one write lock (by calling lock.writeLock() ). Even if you call these methods multiple times, the same lock is returned. A read lock can be locked by multiple threads simultaneously (by calling lock.readLock().lock() ), if the write lock is free. If the write lock is not free, a read lock cannot be locked. The write lock can be locked (by calling lock.writeLock().lock() ) only by only one thread and only when no thread already has a read lock or the write lock. In other words, if one thread is reading, other threads can read, but no thread can write. If one thread is writing, no other thread can read or write.  Methods that do not modify the collection (i.e. the threads that just "read" a collection) should acquire a read lock and threads that modify a collection should acquire a write lock.  The benefit of this approach is that multiple reader threads can run without blocking if the write lock is free. This increases performance for read only operations. The following is the complete code that you should try to run:

- java.util.Random instance across threads may encounter contention and consequent poor performance. Consider instead using ThreadLocalRandom in multithreaded designs,
  public class ThreadLocalRandom extends Random A random number generator isolated to the current thread. Like the global Random generator used by the Math class, a ThreadLocalRandom is initialized with an internally generated seed that may not otherwise be modified. When applicable, use of ThreadLocalRandom rather than shared Random objects in concurrent programs will typically encounter much less overhead and contention. Use of ThreadLocalRandom is particularly appropriate when multiple tasks
  
- Future as the return type of the call method, it would be technically a valid implementation. But it would not be appropriate. A Callable should return actual data object instead of wrapping the data into a Future. It is the job of ExecutorService.submit() method to return a Future that wraps the data returned by Callable.call(). For example: Future result = Executors.newSingleThreadExecutor().submit(new MyTask()); Further, since Future is an interface, new Future ("Data from callable") will not compile either.

- ReentrantLock implements Lock. Lock.lock() returns void. Lock.tryLock() returns boolean.

- public final boolean compareAndSet(int expect, int update) Atomically sets the value to the given updated value if the current value == the expected value. Parameters: expect - the expected value update - the new value Returns: true if successful. False return indicates that the actual value was not equal to the expected value.

- synchronized keyword can be applied only to non-abstract methods that are defined in a class or a block of code appearing in a method or static or instance initialization blocks. It cannot be applied to methods in an interface.

-  A Thread is created by doing new ClassThatExtendsThread() OR by doing  new Thread(classImplementingRunnable); The newly created Thread is started by calling start().  All the code that does the work in a separate thread goes in the run() method. Method signature: public void run() Note that this method is not abstract in the Thread Class. So you do not have to necessarily override it. However, if you want to do anything useful, you should override it. The Thread class's version of run() method doesn't do anything other than to call the run method of the Runnable instance (if it is passed while instantiating the class).  A call to start() returns immediately but before returning, it internally causes a call to the run method of either the Thread instance (if the thread was created by doing new ClassThatExtendsThread()) or of the Runnable instance (if the thread was created by doing  new Thread( classImplementingRunnable);)

- NO:
 1. The forEachOrdered method processes the elements of the stream in the order they are present in the underlying source. In this case, the underlying source of the first stream is the "source" ArrayList. In this ArrayList, the elements are in the required order already and that is the order in which they will be printed even if the stream is a parallel stream because of forEachOrdered.
 2. Parallel streams allow operations such as peek and map to execute on the elements of the stream from multiple threads. This means they can be executed in any order. Therefore, in this case, the code that adds the elements to the destination (that is, the call to peek at //2) can add elements to destination list in any order. This means that, effectively, the order of elements in destination is unknown. This is the problem that really needs to be fixed here. Changing parallelStream to stream on the source will rectify this problem because then the elements will be added to destination in the same order.


- static variable and there are 2 threads that are accessing it. None of the threads has synchronized access to the variable. So there is no guarantee which thread will access it when. Further, one thread might increment i while another is executing i<5. So it is possible that more than 5 words may be printed. 

- two new threads are created but none of them is started.(Remember run() does not start a thread. start() does.)
  Here, run is called but NOT of MyThread class but of Thread class. Thread class's run() is an interesting method. If the thread object was constructed using a separate Runnable object, then that Runnable object's run method is called otherwise, this method does nothing and returns. Here, Thread's run calls MyThread's run() which prints the string and returns. Everything is done in one thread (the main thread) and so the order is known.


- First thread acquires the lock of sb1. It appends X to sb1. Just after that, the OS stops this thread and starts the second thread. (Note that first thread still has the lock of sb1). Second thread acquires the lock of sb2 and appends Y to sb2. Now it tries to acquire the lock for sb1. But sb1's lock is already acquired by the first thread so the second thread has to wait. Now, the OS starts the first thread. It tries to acquire the lock of sb2 but cannot get it as the lock is already acquired by the second thread.
  Here, you can see that both the threads are waiting for each other to release the lock. So in effect both are stuck! This is a classic case of a deadlock.
  So the output cannot be determined.

- 1. The statement : Thread.currentThread().setName("First"); in the main() method sets the name of the current thread. This is the thread that is running the main method. 2. The statement: MyRunnable mr = new MyRunnable("MyRunnable"); creates a new MyRunnable object. In its constructor, it creates a new threads with the name "MyRunnable" and also starts this thread. 3. Now there are two threads running (or ready to run state): The main thread (having the name "First") and the MyRunnable thread (having the name "MyRunnable"). Any of these two threads may be allowed to run by the OS. 4. Calling mr.run() does not create a new Thread. Instead, the run() method is executed by the thread that has called the run() method, in this case the main thread. Since the main threads calls run() twices, the name of the main thread is printed twice in sequence. However, it changes the name before calling the run() second time. Therefore, "Second" will always be printed after "First" (may not be immediately after, though). Since it is printed by the same thread. 


- `AtomicInteger ai = new AtomicInteger(); 
    Stream<String> stream = Stream.of("old", "king", "cole", "was", "a", "merry", "old", "soul").parallel();
    stream.filter( e->{   
     ai.incrementAndGet();     return e.contains("o"); 
    }).allMatch(x->x.indexOf("o")>0);   
    System.out.println("AI = "+ai);`
    
1. In the given code, stream refers to a parallel stream. This implies that the JVM is free to break up the original stream into multiple smaller streams, perform the operations on these pieces in parallel, and finally combine the results.
2. Here, the stream consists of 8 elements. It is, therefore, possible for a JVM running on an eight core machine to split this stream into 8 streams (with one element each) and invoke the filter operation on each of them. If this happens, ai will be incremented 8 times. 
3. It is also possible that the JVM decides not to split the stream at all. In this case, it will invoke the filter predicate on the first element (which will return true) and then invoke the allMatch predicate (which will return false because "old".indexOf("o") is 0). Since allMatch is a short circuiting terminal operation, it knows that there is no point in checking other elements because the result will be false anyway. Hence, in this scenario, ai will be incremented only once.
4. The number of pieces that the original stream will be split into could be anything between 1 and 8 and by applying the same logic as above, we can say that ai will be incremented any number of times between 1 and 8.

- Performance : 
 The order of join() and compute() is critical. 
 Remember that fork() causes the sub-task to be submitted to the pool and another thread can execute that task in parallel to the current thread. 
 Therefore, if you call join() on the newly created sub task, you are basically waiting until that task finishes. This means you are using up both the threads (current thread and another thread from the pool that executes the subtask) for that sub task. Instead of waiting, you should use the current thread to compute another subtask and when done, wait for another thread to finish. This means, both the threads will execute their respective tasks in parallel instead of in sequence. 
 Therefore, even though the final answer will be the same, the performance will not be the same.
 
 - code: `AtomicInteger count = new AtomicInteger(0); at //1 count.incrementAndGet(); at //2`
  AtomicInteger allows you to manipulate an integer value atomically. It provides several methods for this purpose. Please go through the JavaDoc API description for this class as it is important for the exam.
  
-  `public class CopyOnWriteArrayList<E> implements List<E>, RandomAccess, Cloneable, Serializable`
  A thread-safe variant of ArrayList in which all mutative operations (add, set, and so on) are implemented by making a fresh copy of the underlying array.  This is ordinarily too costly, but may be more efficient than alternatives when traversal operations vastly outnumber mutations, and is useful when you cannot or don't want to synchronize traversals, yet need to preclude interference among concurrent threads. The "snapshot" style iterator method uses a reference to the state of the array at the point that the iterator was created. This array never changes during the lifetime of the iterator, so interference is impossible and the iterator is guaranteed not to throw ConcurrentModificationException. The iterator will not reflect additions, removals, or changes to the list since the iterator was created. Element-changing operations on iterators themselves (remove, set, and add) are not supported. 
These methods throw UnsupportedOperationException.


- Although two new threads are created but none of them is started.(Remember run() does not start a thread. start() does.)
  Here, run is called but NOT of MyThread class but of Thread class. Thread class's run() is an interesting method. If the thread object was constructed using a separate Runnable object, then that Runnable object's run method is called otherwise, this method does nothing and returns. Here, Thread's run calls MyThread's run() which prints the string and returns. Everything is done in one thread (the main thread) and so the order is known.