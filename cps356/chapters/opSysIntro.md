# Operating Systems 356

### Virtualization

crux: how does the os do what it does + efficienctly? Hardware suport? Virt?

os: make sure os is running correctly + easy-to-use + efficiently
1. virtualization: physical resource -> general + virtual form
1. api: system call to run, access memory+devices
1. resource management: run multiple instances of program

virt cou: make it seem a single+core cpu has infinite number of cpu's by
allowing programs to run simultaenously

virt memory: running multiple instances of a process on the same address space
making it seem like the os is allocating private memory instead of sharing
physical memory per process

### Concurrency

crux: if there are multiple processes executing threads within the same address
space, how to build a correctly working program?

scenario: multiple executing threads in same address space, for example running
2 threads incrementing loop. if the value is set high then sometimes the value
we get is not what we expect, and also different value as well

```C
volatile int counter = 0;
int loops;

void *worker(void *args) {
	int i;
	for (i = 0; i < loops; i++) {
		counter++;
	}
	return NULL;
}

int main(int argc, char *argv[]) {
	if (argc != 2) {
		fprintf(stderr, "usage: threads <val>\n");
		exit(1);	// error exit in *nix + C
	}
	loops = atoi(argv[1]);
	pthreads_t p1, p2;
	printf("initial value: %d\n",  counter);

	Pthread_create(&p1, NULL, worker, NULL);
	Pthread_create(&p2, NULL, worker, NULL);
	Pthread_join(p1, NULL);
	Pthread_join(p2, NULL);
	printf("final value	: %d\n", counter);
	return 0;		// no error exit in *nix + C
}
```

```
prompt> ./thread 1000000
Initial Value : 0
Final value: 143012
Expected : 2N = 2000000
```

### Persistance

crux: how does the os store data persistently, and manage it. what sort of 
algorithms are needed + policies for good performance. what happens if there
is hardware/software failure

scenario: device drivers; write protocol for system crashes; os as standard
library

```C
int main(int argc, char *argv[]) {
	int fd = open("/tmp/file", O_WRONLY|O_CREATE|O_TRUNC,
			S_IRWXU);
	assert(fd -> -1);
	int rc = write(fd, "hello world\n", 13);
	assert (rc == 13);
	close (13);
	return 0;
}
```

### Overview

os tasks:
1. virtualization
1. concurrency
1. persistance

how: via abstraction making it easier to understand pieces of the os

goal:
1. performance: meaning minimum overhead (more instructions, extra space)
1. protection: isolation of processes to not harm other process &&|| os itself
1. others:
 1. energy efficiency
 1. security
 1. mobility (mulitple devices)

fin
