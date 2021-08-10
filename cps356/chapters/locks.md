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

Earlier solutions involved stopping interrupts completely before entering into
critical sections, and re-enabling them after thread has completed the task.
This was used in single processor machines. It's simplicity was an advantage
as implementation was easy.

The negatives were far too many however, due to needing to stop interruptions
(a privileged operation) we heavily relied on trusting the thread. If it were
greedy it would lock at the beginning and then monopolize the processor. A
malicious program could lock and never unlock thus ending in an infinite loop
(needed to restart). 

This method of stopping interrupts don't work on multiprocessors systems as
other threads could just try to access the critical section. Need a *better*
solution. Lastly, turning off interrupts for longer periods of time can lead
to interrupts becoming lost, causing system problems. If CPU doesn't realize
that device has finished reading, how will it know to wake up the processs
after the read? This method is too inefficient (too slow for modern CPU) and
has too many drawbacks.

## Failed Attempt: Using Loads/Stores

Via the help of CPU hardware and instructions to build a proper lock. We will
first build a simple one using a single flag variable `flag`. First thread 
that tries to access will cal lock(), if lock is set to 0 (unlocked) then the
thread can hold the lock, otherwise it is already locked. When thread is done
with critical section it calls unlock() and clears flag back to 0.

If a thread tries to access a locked flag it will be caught in a loop that
until the first thread unlocks will just `spin-wait`. When unlocked then the
thread can go ahead and lock as needed. This solution has 2 issues; correctness
and performance issues.

If there so happens to be an untimely interrupt, the first thread will set 
lock to 1, an interrupt occurs, thread 2 sets flag to 1. This is exactly the
same issue we had with concurrent programs in general. The basic requirement
of providing `mutual exclusion` was not met. Terrible. That is the issue of
correctness.

For performance, spin-waiting, the endless loop the thread goes into when
trying to access a held lock if terrible for performance. It wastes time
for another thread to release a lock. The performance is even worse in single
processor due to os context switching to a thread that idly sits waiting in
this endless loop.

## Building Working Spin Locks with Test-And-Set

Different architectures support test-and-set but call it different names. On
SPARC it's called load/store unsigned byte instruction (ldstub); on x86 it is
locked version of atomic exchange (xchg)

Need implementation that works on multiprocessors and just a better one due
to loads and stores not working. So hardware support for locking was needed
even on early systems like the `Burroughs B5000` in early 60's. Simplest bit
of hardware instruction is the test-and-set (or atomic exchange instruction).
Shown below:

```C
int TestAndSet(int *old_ptr, int new) {
	int old = *old_ptr; // fetch old value at old_ptr
	*old_ptr = new;     // store 'new' into old_ptr
	return old;	    // return the old value
}

typedef struct __lock_t }
	int flag;
} lock_t;

void init (lock_t *lock){
	// 0: unlocked; 1: locked
	lock->flag = 0;
}

void lock(lock_t *lock) {
	while (TestAndSet(&lock->flag, 1) == 1)
	; // spin-wait (do nothing)
}

void unlock(lock_t *lock) {
	lock->flag = 0;
}
```

The instruction above returns the old value pointed to by `old_ptr` and at the
same time updates the value to `new`. The key is it is atomically done. Reason
this is called test and set is it enables you to *test* old value (which is
returned) and *set* memory location to new value at the same time. This 
instruction alone is powerful enough to build a simple spin and lock.

Lock is set to 1, pass in 1 to new value. This passing of *new* value is what
allows the endless loop. Unless the current thread in critical section calls
the unlock() function then the &lock-\>flag will always equal to 1, always in
this spin lock.

We can see why this type of lock is called a spin lock. Simple lock that only
spins until it becomes available. Need a `preemptive scheduler` for this to 
work on single cpu (interrupts thread via timer, to run another thread) 
otherwise the cpu will never regain control.

### Evaluating Spin Locks

We can start evaluating how effective it is compared to our previous example.
* Does it provide Correctness? Yes
* Is it fair? Not for spin locks, it can spin forever under contention
* Performance?
	1. For single CPU machines, too much performance overhead
	1. For multi CPU machines it works well (if #threads ~= #CPU)

## Compare-And-Swap

Called compare-and-swap instruction in SPARC machines, others may call it in a
different name. Implementation is similar to test-and-set and the code itself
is very similar. See below:
```C
int CompareAndSwap(int *ptr, expected, new) {
	int original = *ptr
	if (original == expected)
		*ptr = new		// new thread here
	return original
}

...

void lock(lock_t *lock) {
	while (CompareAndSwap(&lock->flag, 0, 1) == 1)
	;	// endless spinning
}
```

So the value of flag (1 or 0), whether unlocked or locked is compared to 0
(unlocked). If it is then assign the thread to this lock, but you always
return `original` is any case.

*The compare-and-swap holds more power over spin lock; however that is only
applicable when we look at lock-free synchronization. For the time being
the implementation of spin-lock and compare-and-swap hold the same outcome.*

## Load-Linked and Store-Conditional

Used in MIPS architecture the load-linked and store-conditional instructions
are used together to build locks. Other architecture such as ARM provide a
similar instruction set.

*Essentially `load-linked` loads from whatever register we want, but the 
important conclusion comes from `stored-conditional`. If there have not been
any locks to my destination since my current load-Linked then I can dump
whatever im holding to the address.*

```C
int LoadLinked(int *ptr) {
	return *ptr;
}

int StoredConditional(int *ptr, int value) {
	if (no update to *ptr since LoadLinked to this address) {
		*ptr = value;
		return 1;	// success
	} else {
		return 0;	// fail
	}
}

void lock(lock_t *lock) {
	while (LoadLinked(&lock->flag) ||
		!StoredConditional(&lock->flag, 1))
		;		// spin endlessly
}
```

The above implementation of the function lock is the smaller one develoepd by
undergraduate student David Capel. Uses boolean conditionals. Implementation
details are exactly as described above. Cannot dump into register if another
thread has modified the register since my load-linked providing mutual
exclusion.

## Fetch-And-Add

Use `fetch-and-add` to build a `ticket lock`. Essentially every single thread
has it's own ticket. So that every thread has the chance to have a turn. If
`myturn == true` then that thread can now run. Otherwise it has to let the
tickets (or threads) in front of it to execute first.

```C
typedef struct __lock_t {
	int ticket;
	int turn;
} lock_t;

void lock_init(lock_t *lock) {
	lock->ticket	= 0;
	locker->turn	= 0;
}

void lock(lock_t *lock) {
	int myturn = FetchAndAdd(&lock->ticket);
	while (lock->turn != turn)
		;	// spin endlessly
}
```

One major difference in this impolementation compared to all the other ones
explained above is that for this one, you know that every single thread will
execute as long as tickets are being incremented. Every single thread
will have a go at holding the lock and being in critical section. Which gives
this implementation *fairness*.

## Simple Approach: Just Yield, Baby

Implement a primitive `yield()` function which a thread calls to give up the
CPU and let another thread have a go. Three possible thread states are possible
(running, ready, blocked). Yield is a system call that moves thread from 
running to ready, promoting another thread to run instead. Essentially the
thread has just descheduled itself.

*In the case of 2 threads, our approach works well. Thread holds lock, gives up
the CPU to another thread. Second thread runs and finishes whatever in critical
section. Easy enough.*

*In case of 100 or more threads contending, one of them will acquire the lock
and is preempted before releasing it. The other 99 will find it locked and
yield the CPU. All 99 threads has to execute this yield code before the thread
can run again. Not good performance.*

*We also have a possibility of starvation, where endless yield calls prevent
the thread from running. It is only yielding to all others before it executes
itself.*

```C
void lock() {
	while (TestAndSet(&flag, 1) == 1)
		yield();
}
```

## Using Queues: Sleeping Instead Of Spinning
