# Paging Faster Translation

paging needs a lot of mapping information; an extra memory lookup thus it tends to
be much slower. having to grab memory for translation every fetch/load is slow.

*crux: how can we speed up translation; how can we avoid extra memory lookup; any
hardware or os involvement?*

### TLB + algorithm
tlb: translation-lookaside buffer (old name) is essentially an address-translation
cache. hardware checks here first before using another memory lookup. allows popular
processes to run fast. mmu.

algorith:
1. extract vpn from virtual address
1. check if tlb holds translation for this vpn

1. if yes: (fast)
 1. tlb hit!
 1. extract pfn, cat to offset (from original vaddress) -> physical address
 1. all this assuming protection checks do not fail (has permission)

1. if no: (more work)
 1. hardware access page table to find translation
 1. assuming reference is valid (no segmentation fault) + accessible (permission)
 1. update tlb with translation
 1. retry instruction; this time tlb contains it so it's quick

**spatial locality:** tlb abuses this idea of spacial locality where elements of a 
process are tightly packed next to one another in a page. so that the first element
will be a tlb miss, however the remaining elements will get a tlb hit. this does not
result in a 100% hit rate however a decent one at least. 

the bigger the page size, the more elements within a page thus less tlb misses and 
an overall better performance for the tlb.

**temporal locality:** another tlb property where the quick re-referencing of memory
items in time lead to better tlb hits. applications which access an array right after
completion for example .if a program has these 2 localities then the tlb hit rate 
will likely be high.

### Who handles TLB Misses?

Old  times: the hardware
1. hardware knows exactly where page table is in memory (via ptable base register)
   and the exact format
1. on a miss:
 1. hardware walks the page table
 1. finds correct page-table entry
 1. extract desired translation
 1. update tlb with translation
 1. retry - tlb hit!
1. x86 intel architecture for example uses multi-level page table

Modern era: the software
1. on a miss:
 1. hardware raises exception - pausing current instruction stream
 1. raise privilege to kernel mode
 1. jump into trap handler (code within os written with purpose to handle tlb miss)
 1. code runs, looks up translation in page table
 1. uses privileged instructions to update tlb
 1. return from trap
 1. hardware retries - tlb hit !
1. MIPS R10k; Sun's SPARCV9 are software managed tlb's


*note on modern era software managed tlb's:*

*the system return-from-trap call (seen before) resumes instructions from the
one that caused the trap. in this case, the hardware must retry for a tlb hit.
this mean's the program counter saved must be different depending on the 
exception that was caused.*

*secondly, os needs to be careful not to cause an infinite loop of tlb misses.
2 solutions for fixing this is, to keep the tlb miss handlers in physical memory
unmapped and subject to modification/removal. the other solution is to create
a permanent address translation and store the tlb miss handlers there.*

advantages of software\>hardware:
1. more flexible, subject to data structure change if needed (not hardware)
1. simplicity (raises exception and lets os handle it)

### TLB Contents

1. 32, 64. 128 entries
1. fully associative: translations can be anywhere
1. hardware will search entire TLB in parallel for desired translation

TLB Entry Example: `VPN     |     PFN     |     other bits`

1. Other bits possibilities:
 1. valid bit: whether the entry is a valid translation or not
 1. protection bit: determines access of a page
 1. address-space identifier
 1. dirty bit (modified or not)
 1. and more

### TLB Issue: Context Switches

when switching to another process to run, hardware+/os has to make sure that the
about-to-run process does not accidentally use translation from previous process

example:
1. p1 is running, assumes tlb cache translations are valid for it (same page table)
1. assume 10th vpage of p1's page table mapped -> physical frame 1000
1. os performs switch to p2 and runs it.
1. 10th vpage of p2 mapped -> phyical frame 170

*  vpn translates to same address for p1 and p2; how can hardware distinguish this?

*crux: when context switching, translations in the tlb for last process are not
meaningful to the next process. what should hardware+/os do in this case?*

fix 1: flush tlb
1. set all valid bits to 0 thus flushing the tlb
1. working solution! not.
 1. each new context switch incurs tlb miss
 1. frequent switching leads to higher cost (slower speed)

fix 2: introducing address-space identifiers
1. hardware support
1. essentially process id (with fewer bits) for each process

example:
```
	VPN    |    PFN    |    VALID    |     prot    |    ASID
	10     |    100    |      1      |      r-x    |      1
	10     |    171    |      1      |      r-x    |      2
```

scenario: what if PFN is the same while VPN is different (2 processes using same code)
```
	VPN    |    PFN    |    VALID    |     prot    |    ASID
	10     |    101    |      1      |      r-x    |      1
	50     |    101    |      1      |      r-x    |      2
```
sharing physical address space, same code for example, is good because it reduces
memory overheads
