# Lock-based Concurrent Data Structures

We will look into how to use locks in common data structures. We obviously want
these locks to be both correct and have good performance. 

*Crux: When given a data structure, how do we add locks to it so that it would
work correctly? More so, how do we add locks that yield good performance,
allowing many threads to access the structure at once (concurrently)?*

## Concurrent Counters

One of the simpler data structures are counters, commonly used and has a simple
interfance. 

### Simple But Not Scalable

The code for a `counter` is pretty simple, so I don't really need to include it
here. It contains an init to initialize the counter, increment to increase by 1
and a decrement to decrease by 1. Also a get function to return the value.

*Implementation is simple, you add a single lock which is acquired when calling
a routine that manipulates the data structure. Lock is released when returning
from the call. It is similar to data structure built with monitors, where locks
are acquired and released automatically as you call and return from methods.*

```C
typedef struct __counter_t {
	int		value;
	pthread_mutex_t lock;
} counter_t;

void init (counter_t *c){
	c->value = 0;
	Pthread_mutex_init(&c->lock, NULL);
}

void increment (counter_t *c) {
	Pthread_mutex_lock(&c->lock);
	c->value++;
	Pthread_mutex_unlock(&c->lock);
}

void decrement (counter_t *c) {
	Pthread_mutex_lock(&c->lock);
	c->value--;
	Pthread_mutex_unlock(&c->lock);
}

int get (counter_t *c) {
	Pthread_mutex_lock(&c->lock);
	int rc = c->value;
	Pthread_mutex_unlock(&c->lock);
	return rc;
}
```

*At this point we have a simple working concurrent data structure the only
issue (and for the rest of this chapter) is to get better performance. If the
data structure is not too slow then we are basically done. No need to do any
fancy stuff when something simple will work just fine.*

![Time vs #Threads](https://imgur.com/DxGH5R5.png)

Scalability of this is terrible, a single thread can complete the counter
updates in ~0.03 seconds, having two threads do that task would take ~5 seconds.
This gets worse and worse as you keep adding more threads.

### Scalable Counting

We will be looking at more scalable counters, as they matter in operating
systems even today. In Linux for example, it would suffer from scalability
performance on multicore machines without the research that has been done. We
will be looking at `approximate counters`.

*Approximate counter represents a single logical counter via, local physical
counters (one per CPU core) and a single global counter. So a machine with
four CPU's has four local counters + the one global one. Every local counter
has a lock for itself + global has a lock for itself.*

*Implementation is as follows: when a thread running on 'x' core wants to
increment the counter, it increments the local counter. This local counter is
synchronized with the correct corresponding lock. So, threads across CPUs can
update local counters without issues, so we have scalable counters.*

```C
typedef struct __counter_t {
	int		global;
	pthread_mutex_t glock;
	int		local[NUMCPUS];
	pthread_mutex_t llock[NUMCPUS];
	int		threshold;
} counter_t;

void init (counter_t *c, int threshold) {
	c->threshold = threshold;
	c->global = 0;
	pthread_mutex_init (&c->glock, NULL);
	int i;
	for (i = 0; i < NUMCPUS; i++) {
		c->local[i] = 0;
		pthread_mutex_init (&c->llock[i], NULL);
	}
}

void update (counter *c, int threadID, int amt) {
	int cpu = threadID % NUMCPUS;
	pthread_mutex_lock(&c->llock[cpu]);
	c->lock[cpu] += amt;
	if (c->lock[cpu] >= c->threshold) {
		pthread_mutex_lock(&c->glock);
		c->global += c->local[cpu];
		pthread_mutex_unlock(&c->glock);
		c->local[cpu] = 0;
	}
	pthread_mutex_unlock(&c->llock[cpu]);
}

int get(counter_t *c) {
	pthread_mutex_lock(&c->glock);
	int value = c->global;
	pthread_mutex_unlock(&c->glock);
	return value;
}
```

To keep global counters up to date the local value sometimes dumps into value
adding it onto global counter, incrementing the local counter's value and
setting the local counter to zero. How often depends on this threshold value,
the smaller this threshold value the more this counter acts like the non-
scalable counter above. However, approximation decreases in accuracy. An
example is shown below

![Approximate counter](https://imgur.com/yK4fmaw.png)

When a local's value reaches 5 then dump it into the global, then reset itself
back to 0. In the graph above we can see approximate values are hardly any
different when increasing threads (CPU cores). The threshold value is 1024,
performance is excellent. Graph below shows different threshold values and how
they change in performance/precision.

![scaling](https://imgur.com/ZcF8nqG.png)

*The lower threshold, slower it is but more accurate global value. Higher the
threshold, much better performance but tradeoff is less accurate global.*

## Concurrent Linked Lists

Things will be much simpler, removing lots of common things like delete,
lookup and such - just focus on inserting in a linked list. Also will be
removing some obvious routines a normal linked list would have.

```C
typedef struct __node_t {
	int		key;
	struct __node_t *next;
} node_t;

typedef struct __list_t {
	node_t		*head;
	pthread_mutex_t lock;
} list_t;

void List_Init (list_t *L) {
	L->head = NULL;
	pthread_mutex_init(&L->lock, NULL);
}

int List_Insert(list_t *L, int key) {
	pthread_mutex_lock(&L->lock);
	node_t *new = malloc(sizeof(node_t));
	if (new == NULL) {
		perror("malloc");
		pthread_mutex_unlock(&L->lock); // unlock before fail insert
		return -1;
	}
	new->key = key;
	new->next = L->head;
	L->head = new;
	pthread_mutex_unlock(&L->lock);
	return 0;
}

int List_Lookup(list_t *L, int key) {
	pthread_mutex_lock(&L->lock);
	node_t *curr = L->head;
	while (curr) {
		if (curr->key == key) {
			pthread_mutex_unlock(&L->lock);
			return 0;
		}
		curr = curr->next;
	}
	pthread_mutex_unlock(&L->lock);
	return -1;
}
```

*The control flow is quite error prone, Linux kernel patch study has found that
around 40% of bugs are found on rarely-taken code paths (malloc exit which is
quite rare). Thus, we challenge ourselves by rewriting the code (insert and
lookup) to remain concurrent but avoid failure path which requires an unlock.*

*It is possible by allowing a lock and unlock that only surrounds critical
section of the code (insert code). And, by adding a general exit path in lookup.
The locking change works because only a part of the lookup needs not be locked.
Assuming `malloc()` is thread safe and no bugs will occur, each thread can call
into it. Only when updating the list does a lock need to be held.*

*For the lookup routine, a simple code transformation to jump out the main
search loop to a single return path. This reduces bugs by reducing lock
acquire/release points in the code.*

```C
void List_Insert(list_t *L, int key) {
	node_t *new = malloc(sizeof(node_t));
	if (new == NULL) {
		perror("malloc");
		return;
	} // No lock/unlock before and after malloc here.
	new->key = key;

	// Lock here where it actually matters, reducing lock points
	pthread_mutex_lock(&L->lock);
	new->next = L->head;
	L->head   = new;
	pthread_mutex_unlock(&L->lock);
}

int List_Lookup(list_t *L, int key) {
	int rv = -1;	// default to failure for return
	pthread_mutex_lock(&L->lock);
	node_t *curr = L->head;
	while (curr) {
		if (curr->key == key) {
			rv = 0;	// set to success if found
			break;
		}
		curr = curr->next;
	}
	pthread_mutex_unlock(&L->lock);
	return rv;	// common exit for success/failure
}
```

### Scaling Linked Lists

Linked lists do not particularly scale well. Technique hand-over-hand locking
or lock coupling.

Implementation:
1. Every node has it's own lock
1. When traversing:
 1. Grab next codes lock
 1. Release current node's lock

*Performance takes a hit due to this many locks, especially larger lists. A
hybdrid approach could be looked at where you have a lock every so node, not
seen into yet im guessing?*

## Concurrent Queues

Looking at a concurrent queue data structure designed by Michael and Scott.
There are two locks, one for the head and one for the tail. Goal of these two
locks is to enable concurrency of enqueue and dequeue.

```C
// create node typedef (int value, struct *next)
// create queue typedef (node_t *head, node_t *tail, mutedx hlock, tlock)
void Queue_Init (queue_t *q) {
	node_t *tmp = malloc(sizeof(node_t));
	tmp->next = NULL;
	q->head = q->tail = tmp;
	pthread_mutex_init(&q->head_lock, NULL);
	pthread_mutex_init(&q->tail_lock, NULL);
}

void Queue_Enqueue (queue_t *q, int value) {
	node_t *tmp = malloc(sizeof(node_t));	// tmp node
	assert(tmp != NULL);	// make sure could allocate
	tmp->value = value;	// new node value
	tmp->next = NULL;	// new node not connected yet

	pthread_mutex_lock(&q->tail_lock);	// need lock here
	q->tail->next = tmp;			// setting og tail next to tmp 
	q->next = tmp;				// set next node to tmp
	pthread_mutex_unlock(&q->tail_lock);	// unlock here, done
}

int Queue_Dequeue(queue_t *q, int *value) {
	pthread_mutex_lock(&q->head_lock);	// lock here
	node_t *tmp = q->head;			// new tmp node equal to head
	node_t *new_head = tmp->next;		// 'new_head' tmp var to 2node
	if (new_head == NULL) {			// no 2node, empty
		pthread_mutex_unlock(&q->head_lock);
		return -1;			// unlock and error
	}
	*value = new_head->value;		// value to 2node value
	q->head = new_head;			// 2node now is head
	pthread_mutex_unlock(&q->head_lock);	// unlock
	free(tree)				// unallocate node
	return 0;				// success
}
```

*Now we get concurrency in this data structure via the two locks `head_lock` and
`tail_lock`. Enqueue access to tail\_lock and dequeue access to head\_lock. In
the `Queue_Init` we can see a tmp dummy node that seperates the head and tail.
Required for this implementation.*

*Queue's are used in multi-threaded applications, however this one does not
really meet the needs of these applications. A more developed one will be shown
in the next chapter (Condition Variables)*

## Concurrent Hash Tables

Last data structure we talk about is a `hash table`, this one does not resize
at all. That's for an exercise later on :)

The code for a hash table below is easy, built using the concurrent lists we
mentioned and implemented earlier. Good performance due to having a lock per
hash bucket instead of one single lock for entire structure.

```C
#define BUCKETS (101)

typedef struct __hash_t {
	list_t lists[BUCKETS];
} hash_t;

void Hash_Init (hash_t *H) {
	int i;
	for (i = 0; i < BUCKETS; i++) {
		List_Init(&H->lists[i]);
}

int Hash_Insert(hash_t *H, int key){
	return List_Insert(&H->lists[key % BUCKETS], key);
}

int Hash_Look(hash_t *H, int key) {
	return List_Lookup(&H->lists[key % BUCKETS], key);
}
```

![hash vs list](https://imgur.com/NeIOSby.png)

Graph above shows the performance comparison, we see how good a hash table
scales compared to normal concurrent list structure. There is a reason that
this is the most used compared to all the other ones.

One more thing I'd like to add is this idea of premature optimization that
is talked about. We have to start out very simple, a big lock that surrounds
the entire system and then start to optimize when we need more performance.
Ostep included this quote from *Knuth*.

**Premature optimization is the root of all evil.** - Knuth's Law.

Linux and Solaris had a big lock on kernel, even named `Big Kernel Lock` that
was used in single CPU systems. When multi-CPU systems were starting to come
about it was then changed, Linux and Solaris had different ideas for
optimization, but it all started out as a simple big kernel lock.
