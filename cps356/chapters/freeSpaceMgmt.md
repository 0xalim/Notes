# Free space Management

free-space management is hard when we have variable sized units. so in malloc()
and free() + segmentation to implement memory. the free space is not contiguous
thus requests cannot be satisfied even when the total space is equal or more 
than the request. known as external fragmentation.

crux: how to manage free-space; how to minimize fragmentation; time+space overhead
of alternate approaches.

# Assumptions
1. basic interface: malloc() free() exist.
1. once memory is handed out, cannot change location.
1. no compaction due to assumption 2.
1. single fixed sized contiguous region given by allocator.

to assign memory in heap:

	```C
	void *malloc(size_t sized);
	```

the above code takes in a single parameter and spits out a void pointer; or
null depending on if it succeeds or not. the size of the area is given here.
to free the memory we use:

	```C
	void free(void *ptr);
	```

when using free we only give the pointer to free memory at; library has to fig-
ure this out. a free-list is a list (or data structure really) that keeps track
of free space in the memory.

fragmentation types:
1. external fragmentation (focusing on this one)
1. internal fragmentation (occurs between segments)

focusing on external fragmentation because:
1. simplicity
1. more interesting :)

## Low-level Mechanisms:
goals:
1. basics of *splitting* + "coaelescing"
1. how to track size of allocated regions easily
1. how to build free-list to keep track

### Splitting:
1. find free space via free-space list
1. if requested space == free space; just plop it in
1. if requested space < free space; splitting occurs:
 1. 1 chunk goes to caller
 1. 1 chunk remains in list

### Coalescing:
issue is when calling free(), the address space is put back into the free-list,
but is now divided into smaller chunks. thus if a process requests space larger
than all the chunks, it would be denied even though the memory could potentially
be empty.

e.g divided list:

	```
		head  ->  add:10  ->  add:0   -> add:20  ->  null
			  len:10      len:10	 len:10
	```

1. call free() on address space
1. check if address sits next to one another (or more) in list
1. merge into larger free chunk in free-list

### Tracking size of Allocated Regions
when deallocating memory using free() additional information can be found in a
header above the space that contains e.g:
1. size of the allocated region
1. integrity number (magic number)

	```C
	header_t *hptr = (header_t *) ptr - 1;	//location of header
	assert(hptr->maigc == 129763);
	free_space = hptr + allocated_space;
	```

### Embedding a Free List
* need to build the list inside the free space itself
to manage the space as a free-list:
1. initialize the list:

	```C
	// description of node in list
	typedef struct __note_t {
		int size;
		struct __note_t *next;
	} node_t;
	```	

	```C
	// mmap() returns pointer to free space
	note_t *head = mmap(NULL, 4096, PROT_READ|PROT_WRITE,MAP_ANON|
					MAP_ANON|MAP_PRIVATE, -1, 0);
	head->size = 4096 - sizeof(node_t);
	head_>next = NULL;
	```

when a request for 100b is sent:
1. library finds first chunk large enough for request
1. split chunk in 2 (1 chunk for request, and free-space chunk)
1. allocates requested space + header for easier free()
1. returns pointer start of allocated space
1. shrinks remaining free space

upon calling free():
1. application returns allocated memory by free(x) (x is pointer)
1. library finds size of freed region; adds it to free-space list
1. change *magic* in header to *next* pointing to next free-space
1. result is fragmented free space
1. start coalescing by going through list + merging close nodes

### Growing heap
* the heap may just run out of space; the easiest approach to this is just to
fail honorably. returning a null as needed.

* unix systems request more heap memory using sbrk() (not typically used in code)
and allocate more chunks from there.

* the os finds free physical pages, maps
onto address space of requesting process and returns pointer of end of new heap

### Basic strategies

best fit:
1. search through free-list
1. find chunks that are as big/bigger than requested
1. choose smallest candidate 
1. pro: only need 1 free-space list pass to determine location
1. con: bad implementation pay heavy performance penalty

worst fit:
1. search through free-list
1. find largest chunk and return requested amount; keeping largest on free-list
1. pro: only need 1 free-space list pass to determine location
1. pro: performs badly + excess fragmentation + high overhead

first fit:
1. search through free-list
1. first block that is big enough is chosen
1. pro: speed , no exhuastive searching
1. con:
 1. pollutes beginning of free space with small objects
 1. how allocator manages free list becomes an issue.
1. fix: address-based ordering
 1. keep list ordered by free-space; coalescing becomes easier + fragments less

next fit:
1. extra pointer to first fit that keeps last pointer position
1. idea is to prevent picking off the beginning of the free-list
1. pro: speed is kept due to no exhuastive searching

fin
