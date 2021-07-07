# Interlude: Memory API

crux: How to allocate and manage memory in Unix / C programs; what interfaces
	are used + mistakes to avoid.

## Types of memory:
	1. stack memory: (de)allocations are handeld implicity by compiler,
		called auto memory. declaring memory e.g:
		
		```C
		void func() {
		int x;
		}
		```

	1. calling into the function allocates memory to the stack
	1. when returning it deallocates the memory
	1. if need info beyond call invocation, don't leave on stack

	1. heap memory: (de)allocation handled explicity by the programmer,
		declaring memory e.g:

		```C
		void func() {
		int *x = (int *) malloc(sizeof(int));
		}
		```

	1. code snippet above shows stack memory allocation for int x and,
		heap memory allocation using malloc()
	1. issue with heap memory is programmer inability to clean/understand
		and this leads to lots of security issues too.

## The malloc() call:
	1. use malloc() to allocate, pointer if succeeds or NULL if failure	
	1. programmer often do not supply numbers in malloc() instead vars e.g:

	```C
	int *x = malloc(10 * sizeof(int));
	printf("%d\n", sizeof(x));
	```

	1. sizeof() is often used, however need to be careful
	1. in the code above we suppply 10 x sizeof(int); however we then
		printf sizeof(x) which on 32bit = 4; 64bit = 8.
	1. sizeof() only thinks what the size of x is.

	1. using sizeof() for string is not good, instead use strlen(), e.g:

	```C
	malloc(strlen(s) + 1);
	```	

	1. gets the length of string + 1 for end-of-string

# The Free call:
	1. free(): used to deallocate memory in heap. e.g:

	```C
	free(x);
	```

	1. free() takes 1 argument, the pointer returned by malloc(), so it has
		to keep track of itself

# Common errors:
	1. forgetting to allocate memory. e.g:

	```C
	char *src = "hello";
	char *dst;		// unallocated memory here
	strcpy (dist, src); 	// segmentation fault + dies
	```	

	1. fix: {allocat mem, could even use strdup for easier use}

	```C
	char *dst = (char *) malloc(strlen(src) + 1);	
	```

	1. not allocating enough memory: {buffer overflow}

	```C
	char *src = "hello";
	char *dst = (char *) malloc(strlen(src));	// too small
	strcpy(dst, src);
	```

	1. forgetting to init allocated memory. e.g:

	```C
	int *x = (int *) malloc();
	```

	1. uninitialized read error;
	1. it tries to read from random value, if unlucky it may contain
		information and thus be harmful (potentially)

	1. Forgetting to free memory. e.g:	

	```C
	!free();
	```

	1. memory leak, in long running applications casues oom thus a restart
		is required.
	1. in short running applications it's not as bad, when it returns
		the os will remove all allocated memory. but it's a bad
		habit

	1. freeing memory before you're done with it. e.g:

	```C
	free(x);
	*x=+&x;
	```

	1. called a dangling pointer; can crash a program or rewrite data

	1. freeing memory repeatedly. e.g:

	```C
	free(x);
	...
	...
	free(x);	// without allocating anything to it again
	```

	1. results are unidentified; commonly are crashes

	1. calling free() incorrectly. e.g:

	```C
	free(wrongVar);
	free(123);
	```

	1. passing it wrong pointer; invalid frees are not good!

# Valgrind:
	1. check out valgrind (shown in perugini's course being used +
		it's in the syllabus)

# Underlying os support:
	1. malloc() and free() are lib calls not sys calls.
	1. built ontop of sys calls, such as brk and sbrk
		they are used to increase/decrease (sbrk increments) memory
	1. brk and sbrk should never normally be used as something will most
		likely go wrong.
	1. mmap() used to create anonymous memory region within program,
		associated with swap space.
	1. other calls:
		1. calloc() allocates memory then zeros it before retuning.
		1. realloc() used for memory not in use, makes new larger 
		region of memory by copying old region into it

fin
