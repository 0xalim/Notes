# Common Concurrency Problems

Much of earlier work was focused around deadlock problems in concurrency. We
will delve into that a bit deeper, maybe some non-deadlock bugs as well. We will
also look into real code bases to have a look into some real problems.

*Crux: Many variety of concurrency bugs have patterns. Knowing which ones to
look out for is the first step to writing more robust concurrent code.*

## What Types Of Bugs Exist

You might first ask, what type of bugs exist for concurrent programs? Thats hard
to answer, but people have gone ahead and looked at popular concurrent
applications. So we will use these analyzations.

Open source applications we look at:
1. MySQL
1. Apache
1. Mozilla
1. OpenOffice

In total these 4 applications had 74 total concurrency related bugs. Listed
under non-deadlock and deadlock related bugs first we look at non-deadlock
bugs via the same guys who researched these 4 applications. Then look at
deadlock related bugs via long history of work in preventing, avoiding or
handling deadlock.

## Non-Deadlock Bugs

These fall under 2 major types: `Atomicity violation` and `Order violation`.

### Atomicity Violation Bugs

Simple example in MySQL found

```C
Thread1::
if (thd->proc_info) {
	fputs(thd->proc_info, ...);
}

Thread2::
thd->proc_info = NULL;
```

Here thread 1 runs checks proc\_info has a value so it goes into the if
statement. An interrupt occurs, thread 2 runs and sets proc\_info to NULL;
obviously we see the issue here, a crash is the consequence as a result.

*Atomicity Violation: code region intended to be atomic, but the atomicity is
not enforced during execution.*

Simple fix for this is to require a lock when handling anything with proc\_info
even any other code that accesses the structure.

### Order-Violation Bugs

Non-deadlock bug found by Lu; order violation, simple example below.

```C
Thread1::
void init() {
	mThread = PR_CreateThread(mMain, ...);
}

Thread2::
void mMain(...) {
	mState = mThread->state;
}
```

Issue here is that thread 2 can run before thread 1, resulting in mMain being
not initialized. Thus a crash can be the consequence.

*Order Violation: The desired order between two memory accesses is flipped.
Example is A should always be executed before B, but the order is not enforced.*

One way to solve this is to enforce ordering, condition variables can be used.
Example to solve this code is in p439 in the book. When ordering is needed
between threads, semaphores or condition variables can be used.

## Deadlock Bugs

Deadlock occurs when lock1 is waiting for another lock2, but thread2 that
holds lock2 is waiting for lock1 to be released.

*Crux: how do we build systems to prevent, avoid or detect and recover from
deadlocks? Is this even a problem in systems today?*

### Why Do Deadlocks Occur

Just thinking about it, wouldn't simple deadlocks be easily avoidable?
Couldn't you just do stuff in order? Issue is usually in much more complex
systems than the ones we've discussed and seen. Circular dependancies where
lots of things depend on others (like an operating system!) could easily
be a system that deadlocks appear in.

Another reason is because of encapsulation, in software hidding the
implementation is common practice for many reasons. Due to this, locks don't
mesh well. Example with Java Vector class and method AddAll().

```Java
Vector v1, v2;
v1.AddAll(v2);
v2.AddAll(v1);
```

The implementation locks v1 and v2 to be multi-threaded safe. But if line 2 and
3 are called at nearly the same time, a potential for deadlock is possible.

### Conditions for Deadlock

Conditions:
1. Mutual exclusion; like grabbing a lock
1. Hold-and-wait; thread holds resources while waiting for more resources
1. No preemption; locks cannot be forcibly removed from threads
1. Circular wait; circular chain of threads that hold resource being requested
   by another thread in the chain.

### Prevention

**Circular wait:**

Prevent circular wait by writing code in `total ordering` on lock acquisition.
Need to acquire lock1 before lock2 and such. In larger systems where it is more
difficult to do so, `partial ordering` can be used. 

At the top of Linux source code for memory mapping lock acquisition ordering
can be seen written:
1. simple ones: `i_mutex` before `i_mmap_rwsem`
1. complex ones: `i_mmap_rwsem` before `private_lock` before `swap_lock` ...

* Both partial and total ordering requires careful understanding + design of
code. Great care is required, do not be sloppy with your code!

**Hold-and-wait:**

Way to solve this is to surround lock acquisition within locks. Exmaple:

```C
pthread_mutex_lock(prevention);
pthread_mutex_lock(l1);
pthread_mutex_lock(l2);
...
pthread_mutex_unlock(prevention);
```

We guarantee that no thread switch can occur when acquiring locks. Also fine
to acquire locks in different order, due to it being in a lock so ordering
doesn't matter. Drawbacks are:
1. Encapsulation is against us again, need to know which locks need to be held
   and acquire them ahead of time.
1. Decrease concurrency, need locks acquired early instead of when they are
   needed.

**No Preemption:**

We view locks as held until unlocked, we get in trouble. Thread libraries can
sometimes offer some support to help such as with `pthread_mutex_trylock()`
either grabs if it can or returns error code. Can even try later if fail. An
interface like the one below can be used to build deadlock-free lock acquisition

```C
top:
	pthread_mutex_lock(l1);
	if (pthread_mutex_trylock(l2) != 0) {
		pthread_mutex_unlock(l1);
		goto top;
	}
```

A potential problem `livelock` where 2 or more threads try to acquire a lock
and keep failing. It isn't a deadlock but no progress is being made, so name
livelock. Multiple ways to go about this, random delay and such.

This approach doesn't really add preemption, we're just letting lock owners
back out of lock ownership gracefully.

**Mutual Exclusion:**

Possible to build data structures without locks via use of powerful hardware!

```C
void AtomicIncrement (int *value, int amount) {
	do {
		int old = *value;
	} while (CompareAndSwap (value, old, old + amount) == 0);
}
```

Hardware support to compare and swap as discussed before. Instead of holding
a lock, updating, and releasing it we can build an approach that repeats
the increment and uses compare and swap. No deadlock possibility, only livelock
still. Below is a more complex implementation of list insertion:

```C
void insert(int value) {
	node_t *n = malloc(sizeof(node_t));
	assert(n != NULL);
	n->value = value;
	do {
		n->next = head
	} while (CompareAndSwap(&head, n->next, n) == 0);
}
```

Implementation tries to swap new node into position, as head of list. This
will however fail if another thread swapped in a new head, causing this
thread to retry again with the new head. This is but one implementation for list
insertion, even more complex stuff goes into it for delete, lookup, etc.

### Deadlock Avoidance via Scheduling

Instead of preventing deadlocks, why not avoid them altogether? This approach
needs some understanding of which locks are required at execution to prevent
deadlock.

It is possible to schedule jobs is a manner which prevents deadlock, but
stacking jobs on CPUs costs performance. A famous approach like this is
`Dijkstra's Bankers Algorithm`. This type of approach is not very widely
used, due to the fact that you need to know all tasks and locks that would
be run. So, this approach isn't widely-used general-purpose solution.

### Detect and Recover

Final strategy is just to allow the deadlock to happen ever so often. And then
we take necessary actions moving forward; if the os froze for example due to
deadlock we would need to reboot. In dbms a auto deadlock detector and
recovery technique is implemented. In the event of a deadlock it would need to
restart, human may be needed to ease the process.
