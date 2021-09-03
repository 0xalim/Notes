# Concurrency Introduction

Last 2 sections of book [ostep](https://pages.cs.wisc.edu/~remzi/OSTEP/), we
have created illusions of virtual memory. How it is able to trick programs
into thinking it is running in it's own space, while multiplexing address
spaces. We have also seen how to take a single physical CPU and turn it into
multiple virtual ones.

In this (potentially last) section we create a new abstraction, threads. 
Multi-threaded programs have more than a single execution point (multiple 
pc's). Threads may be seen as a separate process, but they can share the same
address space, meaning they can access the same data.

Thread vs process states:
1. Both have registers that can be dumped/restored, but threads only have to
   replace registers and not page tables (same address space).
1. Process have process control blocks; threads have thread control blocks
   to store the state of each thread of a process.
1. `single-threaded process` as we can call them now, have a single stack. In
   multi-threaded processes each thread has a stack. The *things* we put into
   the stack is thus sometimes called `thread-local storage`.

## Why use threads?

`Parallelism:` executing a task via multiple processors each using a single
thread the program can thus run much faster.

We can prevent blocking program progress due to I/O. Via running multiple 
threads, if there is an I/O request on one, then we can just rely on another
thread to continue completion of a task. `Threading` enables overlap of I/O
with other activties within a single program. So systems like web-server
and dbms's make use of threads in their implementations.

The tasks mentioned above could possibly be used via just multiple processes
instead of threads, but where threads shine is when address space can be 
shared. So if data structures need to be shared then threads more more sense
than processors.

### Thread Creation

```C
int
main(int argc, char *argv[]) {
	pthread_t p1, p2;
	int rc;
	printf("main: begin\n");
	Pthread_create(&p1, NULL, mythread, "A");
	Pthread_create(&p2, NULL, mythread, "B");
	Pthread_join(p1, NULL);
	Pthread_join(p2, NULL);
	printf("main: end\n");
	return 0;
}
```

The ordering of this small program can be different depending on what the
scheduler decides to do first. If it wants thread T2 to print out before
T1 than it can do so, even if T1 finishes beforehand. Or obviously, it could
decide to print out T1 before T2.

One way to think about thread creation is similar to that of a function. A 
normal function gets executed and then returns to the caller; a system can 
create a new thread of execution for the routine that is being called and the
return depends on the scheduler.

## Worsening: Shared Data

Next we will observe how threads react when accessing shared data. In this
example we will be modifying a shared global value, looping 1e7 per thread
(2 threads total; 2e7 should be result) and then adding them. I won't 
include the code, however it is in chapter 26.3 in the book. 

*Sometimes* we the run the program and the result is as expected:
```
prompt> gcc -o main main.c -Wall -pthread; ./main
main: begin (counter = 0)
A: begin
B: begin
A: done
B: done
main: done with both (counter = 20000000)
```

Other times we run the program and we get weird results (even on single
processor machines):
```
prompt> ./main
main: begin (counter = 0)
A: begin
B: begin
A: done
B: done
main: done with both (counter = 19345221)
```

Aren't computers supposed to be deterministic? Why are we getting different
results in some cases different every execution?

### Uncontrolled Scheduling

During a timer interrupt in the middle of incrementing (50->51), the os saves 
the state of the running thread to the threads `tcb`. Then thread 2 is chosen 
to run now, enters the same code and executes the same instruction. Pull value
add 1 and push back in. But counter is at 50 again so we push back 51. Another
context switch occurs, thread 1 resumes and now executes the previous push
instruction it never completed before. We now save counter to 51 again.

*The problem mentioned above is simply, that the counter that has been executed
twice (52) has only saved as if it executed once (51). This is called a `data
race`, which is also in 474 so pay attention!*

Critical sections is code that accesses a shared variable and thus should not
be executed on by more than one thread at a time. We want mutual exclusion,
which means that if one thread is running this then the other threads should
be prevented from doing so.

### Wish for Atomicity

*Atomicity: To group together 'things' so that it becomes a entirely single
entity instead of multiples. Essentially all or none. In our previous example,
instead of pulling adding and pushing, we would have that be a single
instruction `memory-add` for example.*

In many cases we do not want atomicity, in the case of B-trees for example we
would take a long time to complete some of these longer instructions. So 
instead we will ask the hardware for instructions to build a set of 
`Synchronization primitives`. Using hardware + software support to allow
multi-threaded code to access the `critical sections` in a synchronized
manner.

*Crux: What support do we need from hardware + os to build useful
synchronization primitives? How do we build these correctly and efficiently?
How can programs use them to get desired results?*

## Another Problem: Waiting For Another

We have only mentioned one problem that arrises in concurrency however another
does exist in this realm. The interaction arises when a process performs a
disk I/O and is put to sleep; when the I/O completes the process need sto be
*roused* from its slumber so it can continue. Will be learning more about this
in the **condition variables** chapter.
