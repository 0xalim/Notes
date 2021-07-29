# Paging Introduction

segmentation is breaking things (code, stack, heap) ito variable sized pieces,
however paging is breaking things into fixed sized pieces (pages). the memory
is a fixed sized array that contains slots called page frames.

*crux: how to virtualize memory with pages, to avoid segmentation cons, what
are the basic techniques and how we make them work well? esp. with minimal
space and time overheads*

## Simple Example and Overview
paging format:
1. fixed-sized variables
1. virtual addresses placed anywhere

paging advantages:
1. flexibility: effective use of address abstraction by:
 1. not caring about direction heap+stack grow
 1. how heap+stack are used
1. simplicity: simple free-space management
 1. just find the first free space that is enough and place it in

*per-process data structure:* os keeps per-process data structure known as 
page table; major role is to translate virt address to physical address. e.g:
 1. virtual page 1 = physical frame 3
 1. virtual page 2 = physical frame 1
 1. virtual page 3 = physical frame 5

address translation example:
instruction:

	```
	movl 21, %eax

	0  1  0  1  0 1
	----  ---------
	VPN    OFFSET
	```

1. take virtual address = 21
1. convert to binary= 010101
1. because address space is 64 bytes; 2^x = 2^6
1. split address to 6 pages
1. first 2 pages are virtual page number
1. last 4 pages are used in offset
1. now we have our index(1, 5)
1. in physical frame number 01(1) == 111(7)
1. thus final translation in physical address index(7, 5)

	```
	1  1  1  0  1  0  1
	-------  ----------
	 PFN	  OFFSET
	```

to be answered:
1. contents of page tables + their size?
1. is paging too slow?

## Where are Page Tables Stored?
assuming easy linear array for simplicity now; os indexes by *vpn* and looks 
up the page-table entry *pte* at that index to find the *pfn*.

contents of pte:
1. valid bit: whether the translation is valid. if the process tries to access
space between the stack+heap (which will be marked invalid) os will terminate
 1. this valid/invalid idea is to save space on physical frames by not needing
  to allocate anything
1. protection bits: indicates whether rwx is possible; generates trap
1. present bit: indication to whether the data is in swap
 1. swap: space on disk for moving data from memory to here if not used
1. dirty bit: indicates whether changes have been made since
1. reference bit: indicates whether page has been accessed (determines popular
 pages). good for page replacement (later topic)

## Paging is Too Slow:
instruction:

	```
	movl 21, %eax
	```

1. first translate vaddress (21) to correct physical address (117)
1. fetch pte from process page table, perform translation;
1. load data from physical memory

to complete the above, hardware needs page table location for process. thus 
our assumption: 1 page-table base register contains pAddress of page table.
need to know location of pte now:

	```C
	vpn	= (virtualaddress & vpn_mask) >> shift;
	pteaddr = pagetablebaseregister + (vpn * sizeof(pte));
	```

1. if *vpn_mask* set to 0x30 (110000) and shift is set to 4 (bits in offset)
 move vpn bits down to form correct vpn.
 1. 21 *010101* + masking -> *010000*; shift turns it into 01, page 1.
1. use value as index in array of pte's shown via page table base register.
1. hardware fetches pte from memory and extracts the pfn
1. add pfn+offset

in other words; pfn left shifted by *shift*. then or'd with offset.

	```C
	offset   = virtualaddress * offset_mask
	physaddr = (pnf << shift) | offset
	``` 

this whole protocol performs an extra memory reference in order to fetch
the translation from the page table. slows down process by factor 2+

issues present due to this protocol:
1. system runs too slow
1. takes up too much memory

fin
