# Event-based Concurrency (Advanced)

We've only done threads as a way of concurrent programming; but event-based 
concurrency is another method of doing concurrency. Node.js uses this, but
roots are in C/UNIX systems.

Problem in event-based concurrency:
1. Managing locks, deadlocks and such
1. No control over scheduler

*Crux: How do we build a concurrent server, without threads and also keep
control over concurrency. How do we avoid some of the problems that plague
multi-threaded applications?*

## The Basic Idea: Event Loop

*Event-based concurrency: wait for something (an event) to occur, and when it
does you check what type of event it is. Then complete the work it requires.*

```C
while (1) {
	events = getEvents();
	for (e in events)
		processEvent(e);
}
```

Above code is an `event loop`, loop just waits for something to do and then for
every returned event process it. The code that does the processing is called
`event handler`. When it is processing an event, that is the only thing it
does in the entire system. Thus, it's almost an auto scheduler. This control
over scheduling is why event-based is really good.

## Important API: `select()` (or `poll()`)

* How do you receive events? `select()` or `poll()` system calls.

From Linux manpage:
```
 int select(int nfds, fd_set *restrict readfds,
           fd_set *restrict writefds, fd_set *restrict errorfds,
           struct timeval *restrict timeout);
```

The function select waits and examines the file descriptors passed if they
are ready for reading, writing or have `exceptional condition pending`. The
first parameter `nfds` sets the range of descriptors to be checked in each set.
On return select select replaces the descriptor sets with subsets consisting
of those descriptors that are ready for the requested operation.

Select lets you read and write to, in server setting reading determines if a
new packet has arrived (for processing); writing lets it know when it is ok
to reply (outbound not full).

Considering the timeout argument, if set to NULL it causes select to block until
a descriptor is ready. More robust servers will specify a timeout, setting to 0
will return immediately.

These primitives allow a non-blocking event loop. We can check for incoming
packets, read from sockets with messages and reply as needed.

## Using Select()

Lets see some example code to make this more concrete:
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <sys/type.h>
#include <unistd.h>

int main(void) {
	// open and setup sockets (not shown)
	// main loop
	while (1) {
		fd_set readFDs;		// init readFDs
		FD_ZERO(&readFDs);	// set to zero

		// set bits for descriptors the server cares about
		// in this case all for simplicity.
		int fd;
		for (fd = minFD; fd < maxFD; fd++) {
			FD_SET(fd, &readFDs);

		// check for available data
		int rc = select(maxFD+1, &readFDs, NULL, NULL, NULL);

		// check which actually has data using FD_ISSET()
		// and process it
		int fd;
		for (fd = minFD; fd < maxFD; fd++)
			if (FD_ISSET(fd, &readFDs))
				processFD(fd);
		}
}
```

## Why Simpler? No Locks Needed

In single CPU machines, event based application does not have inherent problems
present in concurrent applications commonly. Because there is only one event
ongoing at a time, no thread interruption can occur.

## A Problem: Blocking System Calls

Imagine a request comes from a client into a server, to read contents of file on
disk. The evnet handler has to issue an open() system call, and multiple read()
calls too. On thread based programming this is fine, because multiple threads
can be running, so it can service this while doing other things. With event
based handlers the entire system will be blocked if it cannot do so. Only the
main loop is what's running during that time, huge waste of resource. Rule is
for no blocking calls allowed with event-based systems.

## A Solution: Asynchronous I/O

An interface has been created to fix the issue mentioned above. `Asynchronous
I/O`. This allows an application to issue an I/O but return control whether or
not the I/O has completed. Below is an interface on Macs `aiocb` AIO control
blocks.

```C
struct aiocb {
	int		aio_fildes;	// file des
	off_t		aio_offset;	// file offset
	volatile void	*aio_buf;	// location of buffer
	size_t		aio_nbytes;	// length of transfer
}
```

An application issuing a AIO will fill the above with correct information. Then
issue that asynchronous call to read the file, on Macs again code is below.

```C
int aio_read(struct aiocb *aiocbp);
```

If the above is successful, return right away and the server can continue 
it's work again. How can we tell when an I/O is complete? The data gets sent
to aio\_buf, so it should have the requested data. On Mac this is the last API
call we need.

```C
int aio_error(const struct aiocb *aiocbp);
```

Call above checks if request has completed by returning 0 (success). it will
return `EINPROGRESS` if not. So an application can poll the system via call
to aio\_error() to determine whether the I/O has completed.

Another way we can tell whether or not an I/O has completed, is via a UNIX
signal that would inform the application. This prevents constant polling to
check whether or not the I/O has finished.

Systems without asynchronous I/O lack event-based approach but some people have
put together a working replacement via using a hybrid approach of events that
process packets and a `thread poll` that manages I/O's.

## Another Problem: State Management

Code for event-based approach is harder to write because: when a event handler
issues an I/O, it has to package the program state for next event handler to
use. This isn't needed for thread based, because all the information is on the
threads stack. This issue is called `manual stack management`.

Thread based server example:

```C
int rc = read(fd, buffer, size);	// read from fil descriptor
rc=  write(sd, buffer, size);		// write to socket descriptor
```

This example is trivial for thread based approach. For event based this is
made harder.

Event-based approach:
1. Issue asynchronous read using aio shown above
1. Periodically check or wait for signal that I/O is done
1. When we are informed it's done now what?

Have to use old programming construct `continuation`. Essentially we record the
information in some kind of data structure (i.e hash table). And when event
needs the info, just look it up in the data structure.

In the case of dumping content to socket descriptor above, we have to record
the socket descriptor in a data structure, indexed by the file descriptor.
When the I/O is done the handler would use the file descriptor to lookup the
continuation, which returns value of socket descriptor to caller. Then the
server can do work and write data to socket.

## What Is Still Difficult With Events

All this we talked about is all good and useful on single CPU systems. When we
use multiple CPUs we now bring in the issue of critical sections, and locks
back. Multicore systems are not as easy as singlecore ones for event-based
approach. 

Another problem is because event-based handles blocking badly, page faults do
not integrate well. Handler page-faults and will be blocked, the entire now is
blocked and won't make progress until the page-fault is done. This implicit
form of blocking due to page faults is hard to avoid, and leads to large
performance problems.

Third problem is that event-based code changes over time, the semantics and such
change and what used to be non-blocking is now blocking. So programmers have
to stay up to date with semantics in API in event uses.

Lastly, asynchronous I/O is possible on essentially all platforms but it took
a long time to get there, so it never really integrates with asynchronous
network I/O very easily. You would have to use select() to manage networking
and AIO calls for disk I/O is still required.
