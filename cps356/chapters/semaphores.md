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
producer and consumer threads. A race condition happens *
