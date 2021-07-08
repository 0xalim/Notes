# Segmentation

issue is that potentially space 'allocated' for heap+stack is not being
used; thus it's useless and a waste of space. need for more dynamic
properties.

crux: support large address space (with free space in the middle)

# Generalized base+bounds:
1. every component of a process can have it's own base+bound, so 3 total 
 pairs of base+bound.
1. location of each component of the process does not matter, so change
 it to not have wasted space.
1. base+bound pairs placed for each segment so they are independant in 
 memory.

1. each base+bound has it's own size(limit/size)
 1. now hardware can tell exactly how many bytes are valid in the segment

# which segment are we reffering to?
1. explicit approach: take the top few bits of the virtual address to 
 determine the type of segment + offset.
 1. via example in 16.1:
  1. 00 = code
  1. 01 = heap
  1. 11 = stack
  1. first 2 bits = type of segment; rest 12 bits are offset
 1. adding base register + offset = physical address space
 1. if offset !< bounds = illegal address

1. implicit approach: hardware determines segment by how addres is formed.
 1. if address from PC -> code segment
 1. if address from stack/base pointer -> stack segment
 1. if address from other- > heap segment

# What about the stack?
1. because segment can grow in the opposite direction (decreasing value)
 we need hardware for direction of growth.
 1. 1 positive direction
 1. 0 negative direction

1. translating negative direction: {11110000000000} {max segment = 4KB}
 1. hardware uses 2 top bits to designate segment (stack in this case: 11)
 1. left with offset of 3kb; minus max segment from 3; 3-4 = -1kb+28kb = 27kb
 1. absolute value of negative offset |-1kb| <= size(2kb)

# Support for sharing
1. sharing certain memory segments between address spaces
 1. code can be shared, via multiple process having access to the code segment
 1. extra hardware bits are needed for rwx. 
 1. processes that have access to the shared code cannot write but only r-x
 1. with the protection bits, need exception handling for processes that try to
     --x from r-- segments, or -w- from r-x segments. Permissions!

# Finegrained vs Coarsegrained segmentation
1. above explained segmentation: coarse grained (large segments)
1. finegrained: lots more flexible smaller segments
1. hardware support for segment table to support different segmentation types
 is needed

# OS support:
1. what should the os do in a context switch?
 1. segmentation registers need to be saved+restored (for every process)
1. os interaction when segments grow/shrink (heap using malloc/realloc).
 1. calls sbrk() to change size returning pointer if success/null otherwise

1. important issue: external fragmentation
 1. where the memory is now full of little holes caused by a number of segments
     having free space between them. difficult to allocate new segments/grow
     existing ones now.

1. solution(sorta): compact the physical memory by rearranging the segments,
 via stopping the process, saving registers, copy data to contigious regions
 of the memory. now the allocation would succeed.
 1. however compaction is expensive, memory-intensive + uses lots of cpu time

fin

