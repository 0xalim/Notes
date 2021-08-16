# The Process

*Crux: How to provide the illusion of many CPUs*

* Time sharing: Share time between processes so multiple can run
 1. Drawback: performance takes a hit

## Implementing Virtualization

**Low-level Machinery: (the how)**

*Mechanisms* are protocols that implement needed piece of functionality, e.g
context allows stopping of one process and start of another

**High level Intelligence:**

Policies are algorithms to make decisions within the os. E.g scheduling
policy, which process which run next given those in queue.

Scheduling policy values:
1. Historic call info
1. Workload knowledge
1. Performance metrics

Machine State: what a process can read/update at 'x' time it's running.
* Address space: space in memory process can read/write to.
* Registers: Instructions read/update from here e.g
 1. Program counter: wheres the next instruction
 1. Stack pointer: pointer at stack in LIFO manner
 1. Frame pointer: functions parameter, local vars, return address

Process API:
1. Create: create a new process
1. Destroy: Drestroy process forcefully
1. Wait: wait until process stops running
1. Misc control: e.g suspension of process
1. Status: interface for info on process

## Process Creation

Details:
1. Load code + static data (initialized vars) into address space 
1. Programs usually executable on disk/ssl; so os reads and puts in address
   space
1. Old vs. New:
 1. Old: eager loading
 1. New: loading loading (paging+swapping)
1. After code+static data is loaded, memory must be allocated for process stack
   and initialize some parameters like main()
1. Memory may also be allocated for heap (using malloc() in C) if using data
   structures and such. This memory is dynamically allocated.
1. Some os (Like UNIX) initialize tasks for I/O. Think stdin, stdout, stderr.
1. Finally jump to main() transferring control of CPU to process.

Process States:
1. Running: process running on processor, executing instruction
1. Ready: Ready to run but os has not chosen to run it yet.
1. Blocked: process not ready, e.g initiates a I/O request to disk.

Data Structures:
1. First of many, the process list keeps track of all processes
1. Many types of information need to be stored for processes that are blocked so
   that when they start back up, they have all the info they need.
1. Context switching: stopped processes register saved to memory location, and
   then restoring the registers to resume process.
1. Different process states:
 1. Zombie: Process that's completed execution but still in process table
 1. Unused
 1. Embryo
 1. Sleeping
