# Paging: Smaller Tables

*Crux: liner page tables are too big and take up too much memory in our system.
how can we make them smaller? key ideas? inefficiences arise due to new data 
structre?*

### Simple Solution: Bigger Pages

* Reduce page table size via increasing size of pages.

Problem: internal fragmentation due to waste within each page. applications
allocate pages using only bits and pieces of each, and memory fills up with 
too large pages.

Common system size page sizes:
1. 4kb (x86 architecture)
1. 8kb (sparcv9 machines)

### Hybrid Approach

Using both paging and segmentation together we can use the advantages of both
these systems while minimizing their drawbacks.

*Too big pages: internal fragmentation due to all space not being used*

*Too much segmentation: internal fragmentation of small sizes* 

Hybrid approach: instead of single page table for entire address space of the 
process, we have one per logical segment.

Example:
1. page table for code
1. page table for heap
1. page table for stack

Segmentation gives us feature fo base+bounds.
1. Instead of base pointing to the segment itself, we use it to point to the 
   physical address space of the page table.
1. Bounds indicates end of page table (how many valid pages it has)

*how do you determine which segment an address refers to?*  Using the top 2 
bits:
1. 00 -> unused segment
1. 01 -> code
1. 10 -> heap
1. 11 -> stack

Assuming **only 3** base+bound pairs, the base points to the physical address.
Now every process has 3 page tables assigned to it. On context switches, the 
registers must change to reflect correct locations of current process.

*On hardware*; tlb miss:
1. Hardware uses segment bits (SN) to determine which base+bound to use
1. Takes physical address and combies with virtual page number
1. forming the address of page table entry

```C
SN	     = (VirtualAddress & SN_MASK ) >> SN_SHIFT
VPN	     = (VirtualAddress & VPN_MASK ) >> VPN_SHIFT
AddressOfPTE = Base[SN] + (VPN * sizeof(PTE))
```

This hybdrid approach heavily relies on the value of *bounds*; simply if our
component segment, say code segment, uses only 2 pages then if it tries to 
access anything more than those 2 an exception will be thrown. This saves us
from wasted space, and the abundance of invalid pages.

Issues with the hybrid approach:
1. Fragmentation still possible leading to some waste of space
1. External fragmentation possible due to arbitrary page sized units

### Multi-level Page Tables

Also described as a *tree*, so effective that modern systems like x86 use this.

Effective due to it's use of **Page Directory:**
1. Page directory can tell where the page of a page table is 
1. Can tell if the entire page of the page table is (in)valid

Image below shows difference between a linear approach against our page directory
![Linear vs. directory](https://imgur.com/n4ajILe.png)

We can see how the invalid pages of the page table are not in memory via the
directory approach, and only those that are valid are stored in memory. The
page directory contaisn one entry per page of the page table. It has the
number of page directory entries (PDE). PDE has a valid bit and PFN. The value
of this bit states that if the PDE is valid, at least one page of the page 
table the entry points to is valid. If the PDE is not valid, then the rest 
is not defined.

Advantages:
1. Compact due to space used is the space that is valid (currently in use)
1. With good construction, each page table fits within a page. Easier to manage
1. On TLB hit, same performance as linear approach

Drawbacks:
1. On TLB miss, 2 hits are necessary; 1 to the PDE other to the PTE itself
1. Complexity increases whether software or hardware

### More Than Two Levels

We *require* page tables fit in a single page for best use case, but what if
that's not possible? The solution is deeper tree's than two level like the one
discussed above

To remedy this issue, we split the page directory itself into multiple pages,
and add another page directory another on top of that

Approach:
1. to index upper-level page directory use top bits of virtual addres (PDIn 0)
1. The index is used to fetch page directory entry from the top-level page
   directory
1. If valid, second level of page directory consulted by combining PFN from top
   level PDE and the next part of the VPN (PDIn 1)
1. If valid, PTE address can be formed by using the page-table index combined
   with the address from the second-level PDE.

### Inverted Page Tables

*Extreme space saving data structure*. Instead of having many page tables (one
per process) we keep a single page table that has an entry for each physical
page of the system. 

Entry tells us which process is using the specific page, which virtual page
of that process maps to the corresponding physical page. Scanning this page 
table entry is usually done in a hash table as linear scans would be too slow
and expensive.

The whole idea of page tables is that they are essentially just a data
structure, so anything can be done to compromise whatever property the creator
wants. 

### Swapping the Page Table to Disk

This topic is discussed more indepth later throughout. But the basic idea is 
that our assumption that the page table residing in kernel-owned physical 
memory can sometimes become a little too large. Thus swapping it to disk is 
needed. So these page tables reside in *Kernel Virtual Memory* allowing for the
swap to happen.
