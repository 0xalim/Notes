# Complete Virtual Memory Systems

*Crux: what features are needed to build a complete vm system? How do they
improve performance? Increase security?*

*"Some ideas, even those that are 50 years old, are still worth knowing,
a thought that is well known to those in most other fields (e.g Physics),
but has to be stated in technology-driven disciplines."*

- Favourite quote from this book.

## VAX/VMS Operating Systems

* Vax-11 minicomputer architecture
* Introduced in 70's
* Digital Equipment Corporation

These guys wanted this architecture to work in the smallest of computers
to the best computers, at that time. That is one of their downfalls; guess
that wasn't possible at the time? (Linux does that nowadays). The other issue
they had was their terrible hardware issues were masked by great software.

### Memory Management Hardware

It had:
1. 32-bit virtual address per process
1. Divided into 512-byte pages
1. Virtual address had:
 1. 23-bit virtual page number
 1. 9-bit offset
 1. 2-Upper bits of vpn used to tell which segment page resided in
1. Hybrid of segmentation and paging

* Lower-half address space: process space
 1. First half of process space (P0) user program + heap; lower half stack (P1)
* Upper-half address space: system space (S)
 1. Contained protected os code + data

Issues:
1. Small size of pagse (512) would cause linear page tables to be very large
1. Make sure not to overwhelm memory with page tables
1. Putting page tables in kernel made lookups more complicated

Fix:
1. Segment user address space into 2
 1. P0 and P1
 1. Page table per region
 1. No page table for unused space between P0(heap) and P1(stack)
1. Places user page tables (P1 and P0, two per process) into kernel vm
 1. When allocating page table, it does so in space `S` 
 1. Memory under pressure then swap pages of page tables to disk
1. Lookups made faster by hardware managed TLBs

Base and bound:
1. Base: holds address of page table for that segment
1. Bound: holds size (number of page-table entries)

### Real Address Space

Address spaces that we have mentioned are very simple compared to real ones,
the address space in the below picture shows a more complex and complete one.
The code segment never begins at page 0, instead it's inaccessible to provide
support for detecting null-pointer access. A concern when designing address
space is the support for debugging.

![Address Space](https://imgur.com/SGTU6qd.png)

On a context switch the os changes `P0` and `P1` registers, however the `S` 
base and bounds are never changed. Providing the same kernel structure for each
address space.

*Why is kernel mapped into each address space?*
1. Easy to copy data from that pointer to it's own structure
 1. Os is written and compiled so that it doesn't worry about where the
    data comes from.
 1. Kernel in this case acts like a protected library
1. Protection bits are set in page table, the kernel has high protection bit
   so the privilege level of cpu must match that of the kernel page table in
   order to access and modify things in there. Failed attempts lead to 
   terminating the process

### Page Replacement

PTE contains:
1. Valid bit
1. Protection field (4bits)
1. Dirty bit
1. Os-reserved (5bits)
1. PFN
1. No Reference bit
 * So replacement algorithm has to do with no hardware support for determining
   which pages are active

Memory hogging was also an issue, in that programs that use lots of memory make
it hard for other programs to run. Policies that we have looked at like the LRU
are susceptible to hogging. Thus, developers created `Segmented FIFO`

Segmented FIFO:
1. Each process has maximum page number; Resident Set Size (RSS)
1. If RSS is exceeded, evict first-in page
1. No need for hardware support, so easy to implement
1. FIFO is bad so we also implement: `Seconday chance list`
 1. Placed in either clean list or dirty(modified) list
 1. Process Q requests free page, then take from clean list
 1. If process P faults before it's reclaimed, P reclaims it from dirty list

I/O Swapping:
1. Small pages, disk I/O is slow
1. Implement clustering:
 1. Group large pages together from dirty list
 1. Write them to disk together in one swoop

### Other Neat Tricks

Lazy optimizations:
1. Demand zeroing
1. Copy-on-write

Demand zeroing:

If a process requests a page, then the os has to zero the entire content of the
page to replace and then replace it. This is very demanding especially since 
the process can very possibly never access or reference that page. So initially
the naive implementation is to add the entry to the page table entry and thats
that. If the process then goes to access or reference the page then the os does
need to do the work of zeroing the page table and replacing it properly.

Copy-on-write:
When os has to copy page to another target address, it does not do so but 
naively maps the target address space to the page. And now both pages have
the permission of only *Read*. When the process actually goes to try and modify
the contents the os realizes it's a `copy-on-write` page and thus has to lazily
go and copy to the target address. Now process has private copy of that page.

Unix systems take copy-on-write to a higher performance by using it as sort of
a shared library, when using functions like `fork()` and `exec()` it can be
slow, but a copy-on-write avoids the needless copying and thus retains the
performance

## Linux VM System

(x86) Linux address space same as other systems:
1. Has user portion (code, stack, heap and other parts)
1. Kernel portion (kernel code, stack, heap and other parts)
1. Kernel portion is same when context switching
1. Accessing kernel requires a trap to get into privileged mode

![Address Space](https://imgur.com/qjdnvPL.png)

In 32bit linux user space is 2/3 of the way through (0x0 -> 0xBF~), and the 
kernel portion is (0xC0 -> 0xFF~). Linux kernel portion has 2 virtual 
addresses. A `logical and virtual`, logical is what we normally think of what
lies in kernel, data structures, page tables, per process kernel stack, etc.
`Kernel logical memory` **cannot** be swapped to disk.

Kernel to physical address is the most interesting aspect here however. There
lies a direct mapping between kernel logical addresses and first porion of
the physical memory. The direct mapping has 2 implications:
1. Simple to translate between kernel + physical
 1. Often treated as physical as a result.
1. If memory is contiguous in kernel, it is contiguous in physical. Making 
   memory allocated in this part suitable for operations which need contiguous
   physical memory to work, like I/O transfers via direct memory access (later)

Second type of kernel is `Kernel Virtual Address`; allocating to the logical 
kernel you use kmalloc while vmalloc for the virtual part. This virtual part
is non-contiguous, so easier to allocate. Good for large buffers where finding
large chunks of physical memory would be hard.

For 32bit linux, kernel virtual address enables kernel to address more than
1gb of memory. This was necessary for older machines but less urgent in 64bit
linux (not confined to only last 1gb).

### Page Table Structure

1. (x86) Hardware managed
1. Multi-level page table structure
1. 1 page per process

Os has to setup the mapping in memory, give a privileged pointer at start of
page directory, hardware handles the rest. Os has to get involved during 
process creation, deletion and context switching to make sure correct page
table is being used by hardware mmu.

Move from 32bits to 64bit is huge. Current multi-level page table that use 
64bit are at 4-level table. Later down the line it would become 6-level tables.
Thus requiring 6 levels of  translations for simple page lookup.

### Large Page Support

Intel x86 allows for huge page sizes (2mb -> 1gb+).

Benefits:
1. Bigger page size, less page entries, less mapping required
1. Better TLB performance (less misses)
1. When we inccur a TLB miss, it gets serviced quicker than smaller pages
1. Allocation is faster (in certain scenarios)

Huge page's were supported incrementally, so that applications that explicitly
called for huge pages got them. Otherwise keep using smaller page sizes. More
recently applications require better TLB behaviour. When this feature has been
selected, the os auto looks to create 2mb+ page size.

Drawbacks:
1. Internal fragmentation due to sparsely used pages
1. Fills memory with large but little used pages
1. Swapping does not work well, due to I/O
1. Allocation overhead can be bad

But the need for bigger page sizes is becoming more necessary, and the normal
use of 4kb page sizes is becoming more of a burden.

### Page Cache

Reduce use of persistent storage, better option is to use cache's when 
necessary for popular items. This aggressive caching is also done by Linux os's

Linux `page cache hash table` used for:
1. Memory-mapped files
1. File data + meta data from devices
1. Heap and stack pages that comprise each process (anonymous memory due to
   no named file underneath it, rather swap space)

Page cache also tracks clean and dirty entries. Dirty files are written to the
backing store (specific file, file data, or to swap space) by background
threads. Ensuring later on that modified data eventually gets written to the
persistent storage. Taking place after 'x' amount of time passed or too many
modified files.

### 2Q Replacement

* Uses standard LRU but with a bit of modification, due to having these issues:
1. Process constantly accesses large file (even size of memory or more) the
   LRU will kick every other file out of memory
1. Retains portions of this large file in memory, when they never get 
   re-referenced before getting kicked out.

2Q Implementation:

We keep 2 lists (inactive and active lists) and divide memory between the two.
When first-time access, push into inactie list but when re-referenced then
push into active list. When replacement is needed then take from inactive 
queue. Linux also dumps bottom of active list back to inactive after a short
period. Keep's active list about 2/3 of total page cache size.

## Security and Buffer Overflows

Modern era computing focuses a lot of defensive mechanisms (Linux, Solaris)
while those from the older era tend not to (VAX/VMS).

### Buffer Overflow

**Buffer Overflow:** find a bug in a system, where attackers can inject
arbitrary code into the target address space. Developers believe that the
user input would never be long (wrong!) and thus willingly go ahead with it.
An overflow occurs in the buffer, overwriting memory of the target.

*If target precisely crafts an input that injects code, a form of `privilege
escalation` can occur. If on a network connected device, system can be opened
up to the outside network or the internal one. At this point what you do
with this situation is endless. Very bad.*

*Fix: prevent code from running or executing within certain regions of an
address space - stack for example. AMD introduced the NX bit (no exec)
into their version of x86. Intel later on also introduced 'XD' bit in their
page table entries to accomplish this task.*

### Return-oriented Programming

*ROP: return-oriented programming, lots of 'gadgets' in code, especially C
program. These gadgets link to C library, so an attack can overwrite the
stack so the return address in the currently executing function points
to their desired malicious instruction (or series of). Followed by a
return instruction. So stringing these 'gadgets' together, the attacker
can execute arbitrary code.*

*Fix: `Address Space Layout Randomization` is a feature that got implemented
to counter ROP. The os randomizes the locations things point to, so attackers
can never change precise code consistantly (or at all). Later on it got
added to the kernel, and thus named `Kernel Address Space Layout Randomization`*

```C
int main(int argc, char *argv[]) {
	int stack = 0;
	printf("%p\n", &stack);
	return 0;
}
```

Output on early systems: (consistant)
```
0x7ffd3e55d2b4
0x7ffd3e55d2b4
0x7ffd3e55d2b4
```

Output on newer systems: (randomized)
```
0x7ffe1003b7f4
0x7ffe3e55d2bd
0x7ffe45522e84
```

### Meltdown and Spectre

World of systems security turned upside down due to, meltdown and spectre.
Discovered around the same time (2018). These researchers and engineers
have questioned the fundamental protections offered by the computer hardware
and the os above.

Url linked below shows papers on both meltdown and spectre:

<https://meltdownattack.com/>

* Spectre is considered the more problematic of the two so lets take a look at
that.

*CPU's nowadays perform crazy behind the scenes tricks to improve performance.
One such known technique is called speculative execution. The CPU guesses
which instruction will soon be executed in the future, and starts executing
ahead of time. If the guess is correct the program runs faster, if not the
CPU undoes the effects on state (register) and tries again.*

*Speculation leaves traces of execution in various parts of the system, such
as processor caches and branch predictors. These states can show contents of
memory, even memory that was protected by the MMU.*

*One fix is to increase kernel protection, via removing much of the kernel
address space from each user process and instead have separate kernel page
table for most kernel data (kernel page-table isolation). So instead of 
keeping datastructure, code and data into each process we only keep minimum.
When switching into the kernel, then a switch to kernel page table is needed.
This improves security and avoids attack vectors but slows performance by
a lot*


## Lastly, a Reminder

**We encourage you to read them on your own, as we can only provide the
merest drop from what is an ocean of complexity. But, you've got to start
somewhere. What is any ocean, but a multitude of drops?**
