# Mechanism: Address Translation

deploying similar strategy for the memory as we did for cpu, we want no
applications to affect the os or other applications. Thus, offering 
a form of isolation by keeping efficient virtualization.

crux: how to build efficient virtualization of memory? how to provide
flexibility needed to applications (flexibility: let the process do
what it wants with the address space provided). how to maintain control
over memory.

# Hardware-based address translation: 
1. when the process tries to access memmory (load, store, w/e) it  
 points to x location of memory, but it's virtualized. 
1. hardware needs to translate this address to the physical location  
 on the memory

1. hardware jobs:
 1. manage memory
 1. keep track of which locations are free
 1. which locations are in use
 1. intervene when necessary to memory access by process

# Assumptions:
1. user address space placed *contiguously* (sharing a boundary)
1. size of address < physical memory
1. each address space is the same size

# Example:
1. example code of taking value; adding 3; storing it back:

```C
int x = 3000;
x += 3;
```

1. using objdump:

```C
128: movl 0x0(%ebx), %eax		// load 0+ebx into eax
132: addl $0x03, %eax			// add 3 to eax reg
135: movl %eax, 0x0(%ebx)		// store eax back to mem
```

# Dynamic (hardware-based) relocation:
1. dynamic relocation <-> base and bounds (same meaning)
1. base = starting position; bounds = end position
1. physical address space = virt address = base. e.g:

```
base = 32kb
virtual address = 0
physical address = 0 + 32 = 32 (physical address)
*translation done!*
```
	
1. Thus a simple virtualization is present via base+bounds
1. *dynamic* because relocation at runtime; can move address space
1. 1 base+bounds per cpu (on chip) sometimes called mmu (mem mgt unit)
	
# Hardware requirements summary:
1. privileged mode: prevent user-mode process from executing priviOps
1. base+bound: support address translation
1. translate virtMem -> Phys mem + check if OOB
1. privi instructions to update base+bound (before process runs)
1. privi instruction for exception handling: illegal address access
1. ability to raise exceptions. (location of handler = priviOperation)

# Operating system issues: {via example 15.2}
1. os 1st slot; 2 slot empty; 3 slot process; 4 slot empty; free list
1. os does work if process is terminated/killed (free data structure) +  
 put memory back on free list
1. 1 base+bounds and it may differ per process so save+restore values  
 when context switching

1. os can move address space of process when it's stopped/not running  
 by just copy pasting it to new address space and changing  
 base register pointing to correct location now

1. exception handling: functions installed during boot time (priviOp)  
 that handles when process accesses oob/runs instructions it  
 cannot. usually terminating the process.

# Total boot table now:
	
```
os@boot			hardware			no prog
init trap table		
			address of sysCall handler
			timer handler
			illegal mem-access handler
			illegal inst-handler

start interrupt
timer			timer start; interrupt after
			xms

init process table
init free list
```	

fin
