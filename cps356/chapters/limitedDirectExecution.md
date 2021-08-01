# Limited Direct Execution

Steps:
1. create entry in process list
1. allocate memory for program
1. load program into memory
1. set up stack
1. execute main() call
 1. run main()
 1. execute return from main
1. free memory of process
1. remove from process list

Idea:
* running the program directly on the cpu (hence direct execution)
* crux:
 1. how to make sure program doesn't do anything we don't want it to
 1. hwo does os stop program from running + switch to another process
    (time sharing requires virtualizing the cpu)

### Problem One: Restricted Operations
1. cpu modes:
 1. user mode: restricted to what program can do (no i/o request)
 1. kernel mode: program can run what it likes (including i/o request)
1. issue: how to let user program do some privileged operations?
 1. system calls: allows kernel to carefully expose key pieces to program
1. trap instruction:
 1. the instruction jumps into kernel raising the privilege level
 1. the program can do whatever it wants (if allowed)
 1. finished; returned-from-trap instruction to user privilege
1. how to stop program from jumping to whatever it wants?
 1. via trap  table during boot time
 1. during boot pc is in kernel mode
 1. os informs hardware of location of trap handlers with instructions
 1. the hardware thus knows when system calls and exceptional events take place
1. specifying system calls:
 1. via system-call number; each system call has a number assigned
 1. when os handles system call inside the trap handlers make sure the correct
    number has been assigned+valid
 1. this overhead serves as os protection rather than the program knowing the 
    exact address of something, it only knows this assigned number

### LDE Protocol
1. step 1: during boot time
 1. kernel init's trap table
 1. cpu remembers location for use
1. step 2: whne running a process
 1. kernel sets up things (ndoes on process list, mallic) before using return-
    from-trap to start execution of process
 1. cpu now in user mode; begins running the process
 1. when process issues system call, trap instruction, and return once again
 1. program now has complete all its work and returns from main()

### Problem 2: Switching Between Processes
* crux: when a process is running on the cpu it means the os is not running.
  so how can the os do anything at all? how does the os regain control of the
  cpu so it can switch between processes.

1. approach 1: cooperative
 1. via a trust that the process will give control back via system call
 1. so when the process starts reading a file, writing, or w/e it does a system
    call (yield) to regive control to the os
 1. when a process does an illegal task it gives back control (e.g divide by 0)
 1. problem: what if the process never gives control back? (e.g) calls the
    yield system call
1. approach 2: uncooperative
 1. crux: how to prevent rogue process that take control of the machine
 1. problem solved: timer interrupt; every so many seconds/mseconds an
    interrupt is raised so the process halts
 1. at boot time os decides what code to run when the timer interrupts
 1. at boots time os starts time which is privileged instruction
 1. timer can also be turned off (concurrency later)
 1. hardware has responsiblity to save enough of process state so that when it
    starts running again it is able to do so where it left off
 1. ^ this process is similar to explicit trap in kernel where registers, stack
    and more is stored and easily restored via return-from-trap
1. saving and restoring:
 1. context switch: save few register values to kernel stack or other space and
    restore a few for soon-to-be-executing process (from kernel stack)
 1. making sure when the return-from-trap instruction is called it doesn't
    return to process that was running but executes another process
 1. to save context, low level assembly is run to save general purpose register
    and pointer, swithes to kernel stack for the soon-to-be-executing process
1. quick note on concurrency:
 1. issue: what is an interrupt occurs during an interrupt/trap handler
 1. easy method: disable intterupts while in an interrupt; however this leads
    to lost interrupts which is bad (will know in concurrency chapter)
 1. another method: locking schemes to protect concurrect access to internal 
    data structures. however this becomes complicated fast
