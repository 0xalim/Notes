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
will be a tlb miss, however the remaining elemnts will get a tlb hit. this does not
result in a 100% hit rate however a decent one at least. 

the bigger the page size, the more elements within a page thus less tlb misses and 
an overall better performance for the tlb.

**temporal locality:** another tlb property where the quick re-referencing of memory
items in time lead to better tlb hits. if a program has these 2 localities then the
tlb hit rate will likely be high.
