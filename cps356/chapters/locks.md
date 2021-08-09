# Locks

In the introduction to concurrency we revealed how we need atomicity in certain
cases to avoid `indeterminate` consequences. Thus we come to locks, programmers
annotate their source code with locks putting them around `critical sections`
to ensure execution continues without any interrupts.

## Basic Idea

Using basic code, need lock around code like this:
```C
balance += 1;
```

We can create a lock variable as needed (mutex here):
```C
lock_t mutex;
// ...
lock(&mutex);
balance += 1;
unlock(&mutex);
```

Locks hold state of either being available (free), no threads hold the lock, or
a state of being acquired (held), where a single thread holds the lock. The
routine lock() acquires a lock, if no thread holds the lock then assign a 
random thread to this lock. If another thread tries to access do not return
a pointer, thus *locking* other threads from it. Calling unlock() allows the
lock to be free again. If a thread is waiting for the unlock then it will 
notice the change and hold the lock as needed.

Locks provide some control over scheduling; programmers view threads as as
entities created by themselves but in control (scheduled) by the os. A lock
can provide some of that control to the programmer as needed. 

### Pthread locks

POSIX library uses mutex (mutual exclusion); if a thread is in critical section
it excludes others from entering until it has completed the section. Example
POSIX code below:

```
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

Pthread_mutex_lock(&lock);	// exits on failure
balance += 1;
Pthread_mutex_unlock(&lock);
```

POSIX code above passes a variable to lock and unlock unlike what is shown
above. This is to allow for a more fine-grained approach where it can protect
different data and structures with different locks. This allows more threads
to hold the lock if needed. Coarse-grained approach is normal one so far where
a single thread holds that lock only.

### Building A Lock

*Crux: How do we build a efficient lock? What are the hardware required for a
lock with mutual exclusion + other properties?*

Hardware added to various computer architectures but we wont look at those.
Instead we look at how to use these features to build a mutual exclusion lock.
Will also look at how the os gets involved to build a locking library.

## Evaluating Locks

Basic criteria for locks:
1. Does it provide mutual exclusion
1. Fairness: does every thread get a fair shot at acquiring once free?
1. Performance: time overhead by lock. Look at single cpu, multiple cpu and
   when threads contend for the lock. 

### Controlling Interrupts
