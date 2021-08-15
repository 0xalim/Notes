# Semaphores

Dijkstra invented semaphore as a single primitive for all things related to
synchronization; can use semapphores as both a lock and condition variable.

*Crux: How to use semaphores instead of locks and condition variables? What
is the definition of a semaphore? What is a binary semaphore? Possible to build
locks and condition variables out of semaphores and vice-versa?*

## Semaphores: Definition

Semaphores are objects that hold an integer value, and this value can be
modified; in PISOX via: `sem_wait()` and `sem_post()`. The initial value of a
semaphore determines the behaviour, so init first before calling routines.

```C
#include <semaphore.h>

// declare and init semaphore. 0 means can share between threads in same proc
// while 1 is the value of semaphore here.
sem_t s;
sem_init(&s, 0, 1);

int sem_wait (sem_t *s) {
	// decrement value of semaphore by 1
	// wait if value is negative
}

int sem_post (sem_t *s) {
	// increment value of semaphore by 1
	// if no threads waiting, wake one up
```

*Won't look too much into implementation yet, but only the use of these
primitives. Calling into sem_wait() will decrement if possible and immediately
return, or wait for subsequent sem_post() requests. Multiple threads could be
queued up if needed. Second, sem_post() only increments and wakes a thread up
if needed. No need to wait for anything. When value of s is negative, it shows
how many threads are waiting.*

We will take care of race conditions soon, don't worry.

## Binary Semaphores (Locks)

Start using semaphores, our first implementation is a lock, simply surround
post and wait around critical sections starting at m value of 1.

```C
sem_t m;
sem_init(&m, 0, 1);

sem_wait(&m);
//critical section here
sem_post(&m);
```

A simple example here, two threads present in this case. First thread calls
wait decrementing value to 0. Since value is >=0 then it will just return and
the thread now has access into critical section. Finishes and calls post
incrementing value back to 1.

Another case is that a second thread calls wait while a thread is in critical
section, decrementing to -1. This will put it to sleep, and when first thread
calls into post and increments back to 0 then the second thread can wake up
and do whatever it does. Finally increments to 1 after it's done. Called a
`Binary semaphore`.

## Semaphores For Ordering

Semaphores can be used to order events in concurrent programs, so a thread
may want a list to become non-empty to delete an entity. This pattern of
usage we always see a thread waiting for *something* to happen and a thread
doing that *something*. `Ordering primitive` approach.

```C
sem_t s;

void *child (void *arg) {
	printf("child\n");
	sem_post(&s);
	return NULL;
}

int main(int argc, char *argv[]) {
	sem_init(&s, 0 , 0);
	printf("parent: begin\n");
	pthread_t c;
	Pthread_create(&c, NULL, child, NULL);
	sem_wait(&s);
	printf("parent: end\n");
	return 0;
}
```

*Parent calls wait and child calls post. The initial value of semaphore should
be 0. Imagine these 2 cases:*
* Parent creates child, but child has not run yet (ready, not running). If the
value of semaphore was 1 instead, then parent would enter critical section
instead of child. So 0 should be the correct initial value. Parent calls value
is now -1, parent sleeps child runs, calls post and returns.

![case1](https://imgur.com/U1FmqGz.png)

* Child runs immediately after creation before parent call, increments to 1 due
to post call. Child tries to wake up thread but none are sleep and just returns.
When interrupt back to parent occurs, it runs wait see's value is now 1 thus
decrement back to 0 and immediately return from wait without sleeping. Output
is the same.

![case2](https://imgur.com/UguEHkg.png)

The init number of semaphore can sometimes be thought of, how many of something
can be given away in this scenario. With the lock problem we could give away a
lock; the ordering problem nothing could be given unless child ran first then
we have created a resource. Maybe easier to think about it this way.

## Producer/Consumer (Bounded Buffer Problem)

Looking into the `Producer/Consumer` problem introduced in condition variables
chapter. Using semaphores instead obviously.

### First Attempt

Need 2 semaphores, one for empty and one for full.

```C
int buffer[MAX];	// assume max = 1 for now
int fill = 0;
int use = 0;

void put (int value) {
	buffer[fill] = value;
	fill = (fill+1) % MAX;
}

int get () {
	int tmp = buffer[use];
	use = (use+1) % MAX;
	return tmp;
}

sem_t empty;
sem_t full;
void *producer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		sem_wait(&empty);	// P1
		put(i);			// P2
		sem_post(&full);	// P3
	}
}

void *consumer (void *arg) {
	int i, tmp = 0;
	while (tmp != -1) {
		sem_wait(&full);	// C1
		tmp = get();		// C2
		sem_post(&empty);	// C3
		printf("%d\n", tmp);
	}
}

int main (int argc, char *argv[]) {
	// ...
	sem_init(&empty, 0, MAX);	// MAX are empty
	sem_init(&full, 0, 0);		// 0 are full
	// ...
}
```

Assuming Consumer runs first:
1. Run C1 first
1. Fill goes from 0 -> -1
1. Block the consumer thread
1. Wait for thread to call post on full

Assuming Producer runs first:
1. Run P1 first
1. Call empty
 1. And continue due to empty init set to MAX(1 here)
1. Empty goes from 1 -> 0
1. Producer dumps data into buffer
1. Run P3, fill goes from -1 -> 0
1. Waking Consumer to `Ready` state.
1. One of two things could happen here
	1. Producer loops and hits P1, blocked as value is 0
	1. Consumer runs, returns from wait and eats values

*Now we assume that MAX is set to 10 instead of 1. Assume we also have multiple
producer and consumer threads. A race condition happens in the `put()` function.
If two producer threads are inputting values in the buffer and an interrupt
occurs without incrementing the counter, then second thread could overwrite
the value dumped.*

### Adding Mutual Exclusion

We can add a binary lock around our put() and get() section of the code,
however deadlock occurs. The imeplementation is shown below.

```C
void *producer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		sem_wait(&mutex);	// lock start
		sem_wait(&empty);	// P1
		put(i);			// P2
		sem_post(&full);	// P3
		sem_post(&mutex);	// lock end
	}
}

void *consumer (void *arg) {
	int i, tmp = 0;
	while (tmp != -1) {
		sem_wait(&mutex);	// lock start
		sem_wait(&full);	// C1
		tmp = get();		// C2
		sem_post(&empty);	// C3
		sem_post(&mutex);	// lock end
		printf("%d\n", tmp);
	}
}
```

*You would think adding a lock around the put and get sections of our code
works, however deadlock occurs. Imagine this, a consumer runs first and sees
that no values are in the buffer. Consumer has a lock around the 'table' of 
peaches and wakes up producer. Producer tries to dump peaches on the table but
consumer is holding the lock still.*

More technical explanation:
1. Consumer runs first, runs lock start
1. Consumer runs C1, no values so wake up producer
1. Producer wakes up tries to run lock start
1. Producer cannot continue because consumer holds lock.
1. Deadlock!

```c
// Correct implementation
void *producer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		sem_wait(&empty);	// P1
		sem_wait(&mutex);	// lock start
		put(i);			// P2
		sem_post(&mutex);	// lock end
		sem_post(&full);	// P3
	}
}

void *consumer (void *arg) {
	int i, tmp = 0;
	while (tmp != -1) {
		sem_wait(&full);	// C1
		sem_wait(&mutex);	// lock start
		tmp = get();		// C2
		sem_post(&mutex);	// lock end
		sem_post(&empty);	// C3
		printf("%d\n", tmp);
	}
}
```

Simple fix here is to reduce the scope of the binary lock, we only want it to
surround the critical section.

## Reader-Writer Locks

Need more flexible locks, for example looking up values in a list don't really
need any protection from multiple threads modifying it. Lookup's only read the
value in a list, so having concurrent operations is completely fine here. This
is known has reader-writer lock.

```C
typedef struct __rwlock_t {
	sem_t lock;
	sem_t writelock;
	int   readers;
} rwlock_t;

void rwlock_init(rwlock_t *rw) {
	rw->readers = 0;
	sem_init(&rw->lock, 0, 1);
	sem_init(&rw->writelock, 0, 1);
}

void rwlock_acquire_readlock(rwlock *rw) {
	sem_wait(&rw->lock);
	rw->readers++;
	if (rw->readers == 1)
		sem_wait(&rw->writelock);
	sem_post(&rw->lock);
}

void rwlock_release_readlock(rwlock *rw) {
	sem_wait(&rw->lock);
	rw->readers--;
	if (rw->readers == 0)
		sem_post(&rw->writelock);
	sem_post(&rw->lock);
}

void rwlock_acquire_writelock(rwlock_t *rw) {
	sem_wait(&rw->writelock);
}

void rwlock_release_writelock(rwlock_t *rw) {
	sem_post(&rw->writelock);
}
```

*Implementation is simple, is a thread wants to modify then it should acquire
a writelock and release writelock via given procedures/functions. Only writer
is allowed for obvious reasons.*

*The implementation for a readlock is more interesting here. First, reader gets
a lock and increments number of readers in the data structure. The first ever
reader acquires a readlock(), and the last reader out removes readlock(). Any
amount of readers are allowed at a time, however if a thread calls writelock
then it has to finish until all readers are done. Acquire readlock calls wait
on writelock, release readlock calls post on writelock. Should be obvious how
the implementation now works.*

Main drawback of this implementation is starvation, readers could starve
the writers! The implementation has lots of overhead, complex implementations
could have no advantages over simple primitves so be cautious in use. This
just shows flexibility of semaphore usage.

## The Dining Philosophers

Posed and solved by the one and only, Dijkstra! Less practical as it is fun and
'intellectually interesting' (don't like this phrase but oh well). Lets look at
a broken implementation first

```C
void get_forks(int p) {
	sem_wait(&forks[left(p)]);
	sem_wait(&forks[right(p)]);
}

void put_forks(int p) {
	sem_post(&forks[left(p)]);
	sem_post(&forks[right(p)]);
}
```

Imagine 5 philosophers and 5 forks. Each philosopher has time to think (wait) and
time to eat. But to eat, they must use 2 forks instead of 2. Below is a simple
pictoral simulation.

![dining](https://imgur.com/scGVt1C.png)

The implementation above is flawed due to a deadlock. There is a possibility
that every philosopher grabs hold of one fork before they are able to grab 2.
E.g p0 cannot grab f0 AND f1 before p1 grabs f1. If that happens all round the
table, then a deadlock occurs all philosophers waiting for someone to finish
their eating but noone will.

Dijkstra solved this problem by letting the last philosopher grab their right
fork first. The implementation is below.

```C
void get_forks(int p) {
	if (p == 4) {
		sem_wait(&forks[right(p)]);
		sem_wait(&forks[left(p)]);
	} else {
		sem_wait(&forks[left(p)]);
		sem_wait(&forks[right(p)]);
	}
}
```

There are similar problems to this one that are fun and intellectually 
stimulating. They get you thinking about concurrency properly. Cigarette
smokers problem, and sleeping barber problem are two others. Multiples are
out there if needed.

## Thread Throttling

Form of admission control, once in a while programmer is in a situation where
they want to control(threshold) how many threads can run concurrently. Given
a situation where a memory-intensive region can be accessed by threads, a
programmer would like to throttle how many threads can enter that region. To do
so, they would need to init the semaphore value to maximum number of threads
that should enter that region and surround that region would a wait() and
post().

## How to implement Semaphores

This section, we build out own semaphores called `Zemaphores`.

```C
typedef struct __Zem_t {
	int 		value;
	pthread_cond_t  cond;
	pthread_mutex_t lock;
} Zem_t;

// only 1 thread can call
void Zem_init(Zem_t *s, int value) {
	s->value = value;
	Cond_init(&s->cond);
	Mutex_init(&s->lock);
}

void Zem_wait(Zem_t *s) {
	Mutex_lock(&s->lock);
	while (s->value <= 0)
		Cond_wait(&s->cond, &s->lock);
	s->value--;
	Mutex_unlock(&s->lock);
}

void Zem_post(Zem_t *s) {
	Mutex_lock(&s->lock);
	s->value++;
	Cond_signal(&s->cond);
	Mutex_unlock(&s->lock);
}
```

Difference between this implementation and Dijkstra's is that this one will
never be negative. We throw a wait if value <= 0. This behaviour is easier to
implement + matches current Linux implemention.

*Building condition variables out of semaphores is much tricker, some
programmers tried for the Windows Environment and many bugs ensued.*
