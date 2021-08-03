# Beyond Phyiscal Memory: Mechanisms

Going to be relaxing out assumption that every address space is really small, 
and that it all fints into physical memory perfectly. In reality we have cases
where too much stuff is in memory thus we need to move that to bigger but
faster space. Modern systems usually use the harddrive

*Crux: how can the os make use of larger slower device to provide the illusion
of a large virtual address space.*

*Why do we support large address spaces for processes?* Because it makes our
life easier as users. Imagine having to arrange for the code and data before
calling our function or accessing some data.

Other reasons we need this illusion:
1. With introduction to multiprogramming needing to swap space in needed
1. Ease-of-use as described above

### Swap Space

Is a reserved space on the disk for moving pages back and forth, moving it in
page-sized units. Os needs  to remember the disk address of a given page.
Sometimes data like binary files are swapped out in place of something more
important, because the computer knows this information is on disk at all times.

### Present Bit

Assuming hardware tlb:
1. If a tlb hit then just accesses the data from memory like normal
1. If a tlb miss, and checks that the present bit is set to 0, then it is a 
   page fault.

Present bits: If present set to '1' then page is present in memory and every
thing proceeds as normal, otherwise we have a page fault. Upon a page fault the
os runs the 'page-fault handler'.

### Page Fault

Whether it is *hardware* or *software* in this case, if a page is not present
then the os would be in charge of the *page-fault handler* 

Where:
Page tables are used to store the address of pages that are not present. The
os uses the bits in the PTE (used for data in pfn), to look into the PTE and 
finds the address and issues request to disk to fetch page into memory.

1. When I/O completes, os updates page table and marks page as present
1. Updates the PFN field of page-table entry to record the in-memory location
1. Retries the instruction

If a TLB miss occurs:
1. Service the tlb miss
1. Update the tlb with translation (could service when page-fault is serviced)
1. Restart would then be used to find the translation in the tlb
1. Fetch the data or instruction from memory at translated physical address

### What if Memory is full

Process of replacing or kicking a page out is called *page-replacement policy*.
Will be learning about this in next chapter, as kicking out a page that is 
needed will cause speeds to be similar to disk-performance instead of memory
performance. For this chapter, it's enough to assume a policy exists - end.

### Page Fault Control Flow

Three important cases:
1. page is present+valid -> tbl miss handler grabs the PFN from PTE and retry
1. page-fault handler must be run; valid but not in physical memory issue
1. bug in code potentially; invalid page

In software case:
1. OS must find physical frame for soon-to-be-faulted-in page to reside
1. If no such page, wait for replacement algorithm to run and kick pages out
   of memory
1. Handler issues I/O to read in page from swap space
1. OS updates page table and retries the instruction; resulting in tlb miss and
   then upon a retry should be a tlb hit.

### When Replacement Really Occur

The os doesn't normally wait for physical memory to fill up till it does a 
replacement, normally it keeps some space free no matter what.

Watermarks:
*High and low watermarks* help decide when to start replacing/evicting memory.
It works as follows:
1. Os notices <'x' amount of LW pages avaible
1. Background thread 'Swap daemon' evicts until enough HW available
1. Swap daemon goes to sleep then

Performing a number of replacements at once reduces some overhead; works by
grouping some pages together, sometimes called clusters, and write them out to
the swap partition. This changes up the flow mentioned above slightly, the
algorithm would check if there are pages available. If not, informs the 
background daemon to free some space. Then it would reawake the initial thread
to go about it's work with more space available.

fin
