# Condition Variables

In order to continue building concurrent programs, we still need more
primitives. We *need threads to be able to check whether something is true
or not before continuing it's execution.*. Example, parent thread checking
whether or not child process has completed.

We can easily develop a `spin-based` approach, like the one shown below:
```C
volatile int done = 0;

void *child(void *arg) {
	printf("child\n");
	done = 1;
	return NULL;
}

int main(int argc, char *argv[]) {
	printf("parent: begin\n");
	pthread_t c;
	Pthread_create(&c, NULL, child, NULL);	// child process
	while (done == 0)
		; // parent spinning...
	printf("parent: end\n");
	return 0;
}
```

*Wasted CPU time leads to inefficient use of time in general, need a better
more concurrent approach! Better to put the parent to sleep as it is waiting.*

*Crux: Multi-threaded programs require some threads to wait for certain
conditions to be met, spinning is disgustingly inefficient; how should threads
wait for a condition?*

## Definition and Routines

A condition variable is queue that a thread can put themselves in when a
condition has not been met. Rather than letting the thread spin, put it to
sleep. When a thread has completed their work, they can signal that the
condition has been met and wake up the sleeping thread(s).

A condition variable is declared like this: `pthread_cond_t c` where c is the
variable name. 2 operations are given: `wait()` and `signal()`. The wait()
operation is to put a thread to sleep, signal to signal that thread that the
condition has been completed and wake it up.

```C
int done = 0;
pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t c = PTHREAD_COND_INITIALIZER;

void thr_exit () {
	Pthread_mutex_lock(&m);	  // lock
	done = 1;		  // complete critical section work
	Pthread_cond_signal(&c);  // wake up parent process
	Pthread_mutex_unlock(&m); // unlock
}

void *child(void *arg) {
	printf("child\n");
	thr_exit();
	return NULL;
}

void thr_join() {				// initial caller to thing
	Pthread_mutex_lock(&m);			// lock
	while (done == 0)			// while work not yet done
		Pthread_cond_wait(&c, &m);	// set process to sleep (zzz)
	Pthread_mutex_unlock(&m);		// unlock when done
}

int main(int argc, char *argv[]) {
	printf("parent: begin\n");
	pthread_t p;
	Pthread_create(&p, NULL, child, NULL);	// new thread
	thr_join();				// call this function?procedure
	printf("parent: end\n");
	return 0;
}
``` 

*1st possible case: parent creates thread, call thr_join(), waits for child
process to complete. So, it acquires lock, checks if child is done (no) so it
goes to sleep (releasing the lock, wait() releases the lock). Child prints its
message and call thr_exit() which locks, sets done to 1, wakes up parent,
parent runs and unlocks. Prints final message..*

*2nd possible case: child runs upon creation, set done to 1, signals parent
to wake up (it never went to sleep), and is done. Parent calls thr_join and
see's that done is already set to 1 so just returns.*

*using while loop in thr_join rather than if; better to do so actually. Will
see why later.*

### Bad Implementation Examples

Couple bad implementation scenarios to avoid:
```C
void thr_exit() {
	Pthread_mutex_lock(&m);
	Pthread_cond_signal(&c);
	Pthread_mutex_unlock(&m;)
}

void thr_join() {
	Pthread_mutex_lock(&m);
	Pthread_cond_wait(&c);
	Pthread_mutex_unlock(&m;)
}
```

*If child runs before parent says so then it will try to wakeup a thread that
never went to sleep in thr_exit(). When parent runs thr_join() it sleeps
forever.*

```C
void thr_exit() {
	done = 1;
	Pthread_cond_signal(&c);
}

void thr_join() {
	// this example is not real as wait requires mutex, but for sake of
	// this negative implementation it's good enough.
	if (done == 0) {
		Pthread_cond_wait(&c);
}
```

*No locks around anything here, possible interrupt occcurs between checking if
done is 0 and wait(). CPU then runs thr\_exit and tries to wakeup thread that
never went to sleep, context switch back and parent does wait() forever.*

## Producer/Consumer (Bound Buffer) Problem

Some background information, first posed by Dijsktra. Led to Dijkstra and his
co-workers to create a generalized semaphore (used as lock + condition variable)
learn more about that in the next chapter.

Producer is one that generates and places them in a buffer somewhere while the
consumer takes that and does something/work with it. Example is HTTP requests
in a webserver goes into work queue(bounded buffer); a consumer thread takes
these requests out and processes them.

```
	grep foo file.txt | wc -l
```

The example above writes from file.txt with foo in them to stdout, pipe
redirects that output to wc -l which counts number of lines. Grep is the
producer here, wc is the consumer and there is an in-kernel buffer somewhere.

### Single Variable Example

Create an example with the buffer being a single variable, producer pushes
something into it and a consumer pops something out of it. We will use asserts
to make sure something is in or not.

```C
int buffer = 0;
int count = 0;

void put (int value) {
	assert (counter == 0);
	count = 1;
	buffer = value;
}

int get () {
	assert (count == 1);
	count = 0;
	return buffer;
}

// producer loops some numbers into buffer
void *producer (void *arg) {
	int i; 
	int loops = (int) arg;
	for (i = 0; i < loops; i++) {
		put(i);
	}
}	

// consumer just prints out (endless loop) all values
void *consumer (void *arg) {
	int i;
	while (1) {
		int tmp = get();
	 	printf("%d\n", tmp);
	}
}
```

Now we need routines that know when it is ok to access the buffer to put or
get data from it. Conditions are: put when count is 0, only get when count is 1.
If we put data into a full buffer or get from an empty one, we have done
something wrong (check via asserts).

### A Broken Solution

In our implementations we have critical sections within put() and get() but
locks won't work here. Need conditional variables.

```C
int loops;	// init like above somewhere
cond_t		cond;
mutex_t		mutex;

void *producer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		Pthread_mutex_lock(&mutex);
		if (counter == 1)
			Pthread_cond_wait(&cond, &mutex);
		put(i);
		Pthread_cond_signal(&cond);
		Pthread_mutex_unlock(&mutex);
	}
}

void *consumer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		Pthread_mutex_lock(&mutex);
		if (counter == 0)
			Pthread_cond_wait(&cond, &mutex);
		int tmp = get();
		Pthread_cond_signal(&cond);
		Pthread_mutex_unlock(&mutex);
		printf("%d\n", tmp);
	}
}
```

*First issue: Imagine 2 consumers running and 1 producer, a consumer can sneak
in and consume existing values in the buffer (due to if statement). When the
other consumer tries to run it tries to get from an empty buffer. Should have
tried to stop the consume after the buffer was empty right?*

![trace](https://imgur.com/GPOnnmz.png)

The problem is with signaling you are only relaying to the thread to wake up,
it hints that change has taken place (added items). But not specifically what
has changed. The state may not be as desired due to multiple consumers in this
case. This is called `Mesa Semantics`.

*Hoare sematics came later, harder to build but guarantees that an awoken
thread immediately runs. Every system ever built runs Mesa semantics.*

### Better, But Still Broken. While, Not If

Easy fix to this is change `if` to a `while` loop. Consumer wakes up with
lock, checks if buffer is empty. If empty go to sleep. Always safe to use
while loops in this case.

*Second issue: something to do with having only one condition variable. Look
ahead for issue, first will explain via an example.*

![trace2](https://imgur.com/3i9qQw9.png)

Going through trace:
1. Consumer1 + consumer2 run first
1. Both go to sleep
1. Producer runs, dumps item, signals a consumer; consumer1 for example
1. Producer loops back (release+require lock) and tries to dump more items
1. Now consumer1 ready to run; consumer2 + producer asleep
1. Consumer1 wakes up from wait(), rechecks condition and consumes value
1. Consumer signals on condition - should only wake 1 thread.
 1. It should wake producer could it ate up all the buffer items right?
 1. If it wakes up consumer2 (possible) we have a problem!
1. Consumer2 wakes up tries to eat from empty buffer.
1. Now consumer1+2 and producer are all asleep.

*Issue: consumers should not wake up other consumers, only producers and vice-
versa.*

### The Single Buffer Producer/Consumer Solution

Solution is simple; use two condition variables instead of one.

```C
cond_t empty, fill;
mutex_t mutex;

void *producer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		Pthread_mutex_lock(&mutex);
		while (count == 1)
			Pthread_cond_wait(&empty, &mutex);
		put(i);
		Pthread_cond_signal(&fill);
		Pthread_mutex_unlock(&mutex);
	}
}

void *consumer (void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		Pthread_mutex_lock(&mutex);
		while (count == 0)
			Pthread_cond_wait(&fill, &mutex);
		int tmp = get();
		Pthread_cond_signal(&empty);
		Pthread_mutex_unlock(&mutex);
		printf("%d\n", tmp);
	}
}
```

With the implementation above, a consumer can never wake up a consumer and 
the opposite is true as well.

### The Correct Producer/Consumer Solution
