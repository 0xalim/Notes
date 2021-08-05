# Beyond Physical Memory: Policies

Source for every content here is as usual:
[Ostep](https://pages.cs.wisc.edu/~remzi/OSTEP/)

Cases where we are under memory pressure; the os has to start paging out pages
and start making room for newer stuff. This replacement policy is an important
aspect. 

*Crux: How does the os decide which page(s) to evict from memory? This is 
done by the replacement policy, which has to follow some principles to avoid
corner cases*

### Cache Management

Our goal is:
1. Reduce number of cache misses via replacement policy
1. Maximize number of cache hits 
1. Calculate average memory access time for a program (AMAT)

```
AMAT = Tm + (Pmiss * Td)

Tm    = cost of accesssing memory
Td    = cost of accessing disk
Pmiss = Probability of not finding the data in cache (miss)
```

*Assumptions*:
1. Cost of accessing memory (Tm) = 100 nanoseconds
1. Cost of accessing disk (Td) = 10 milliseconds

Cost of disk access is high in modern systems, so missing will dominate
the overall AMAT of running programs. 

## Optimal Replacement Policy

Developed by *Belady*, called MIN; we throw out the page that will be accessed
the furthest in the future. The ones we access now hold more value to us. We
will access other pages before we access the one furthest out.

This replacement theory is essentially 'optimal' but not very possible; it
begs the question, how do you know which page will be accessed the furthest
out?

This is essentially only a *comparison* rather than proper policy.

### Simple Policy: FIFO

Simple implementation, pages are put in queue and when a replacement occurs
the tail of the queue (first-in page) is evicted.

Very easy to implement and low complexity, very bad in hit rate due to high
miss rate. 'nough said, moving on.

### Another Policy: Random

Due to the nature of randomness, it all depends on luck on how well it does 
on replacing the optimal pages. Sometimes it does as good as optimal, while
other times as bad as fifo.

### Using History: LRU

Need to up the complexity for better outcomes now; LRU uses history as it's
guide to choosing which is best to replace.

What information could be used?
1. Frequency: More times means it should be less likely to replace
1. Recency: More recently means more likely to be accessed

Principle locality: The above family of policies are called principle of
locality. Because of how common these programs tend to access certain codes
and sequences, we should keep these in memory when it comes to eviction time.
Producing what is known as *Least Frequently Used*. Another is called *Least
Recently Used*.

These policies are better than random and fifo; the opposite of these 
algorithms do exist. *Most Frequently/Recently Used*. However, they don't 
perform as well.

## Workload Examples

*Look at more examples, but instead of small traces we want bigger workloads
to see how good these policies are(or not)*

* These worklaods have no loaclity, as random as possible accessing pages.

![Workload](https://imgur.com/lbERsWm.png)

1. y-axis: hit rate policy achieves
1. x-axis: cache sized

Page above shows difference between policies we have discussed already. They
are all essentially the same besides the optimal policy.

### 80-20 Workload

Exhibits locality; 80% of references are make to 20% of pages, while the 
remaining 80% of references are made to 20% of pages. Hot pages are ones
that are referenced most of the time, cold are the remainder.

![80-20](https://imgur.com/qNpE141.png)

As seen from the graph above, we have better outcomes all over the board. But
LRU does noticably well, more so than random and fifo. Optimal is still best.

### Looping Sequential Workload

For example, we refer to 50 pages in sequence (from 0 - 49 and loop back). We
then repeat this process for 10,000 accesses to 50 unique pages. 

![Looping-sequential](https://imgur.com/IpVyex1.png)

This workload is common in commercial applications, like databases has worst
case for both the LRU and FIFO. Because we're looping the workload, the older
pages are going to be accessed sooner than the page the policy prefers to keep
in cache. Random is much better, reasoning for this is random has property to 
not have weird corner-cases unline FIFO and LRU

## Implementing Historical Algorithms

*Crux: now we understand these algorithms and worklaods more, how do we implement
them? Won't we have too much overhead and cause performance issue? Even
with hardware support scanning after each memory reference is too much 
overhead; so can we survive with just an approximation?*

### Approximating LRU

Approximation works fine in this case. The implementation requires hardware
support via a *use bit* sometimes called *reference bit*. One use bit per 
page of the system, and it lives in memory somewhere. When the page is accessed
the use bit is set to 1 by the hardware. Hardware never reset it, thats the job
for the os. 

1. Use bit in memory
1. Each page has use bit
1. Memory reference: hardware updates use bit to 1
1. Os resets use bit when necessary back to 0

Clock algorithm:
1. Imagine circular list
1. Clock hand points to page P (doesn't matter which)
1. When replacement is needed, check if use bit is 1
1. Is it?
 1. Then replace use bit to 0 and increment P+1
1. Is it not?
 1. Candidate for replacement 
1. Worst case: Every bit was set to 1 and we looped entire list

*Any algorithm would suffice as long as it would differentiate between use 
bit 0 and use bit 1; the clock algorithm was good because it didn't scan entire
memory for unused pages*

### Considering Dirty Pages

Modification of clock algorithm: has page been modified while in memory?
1. Yes: bit is dirty, needs to write back to disk (which is expensive)
1. No: bit is clean, eviction is free and no need to write to disk.

Implementation:
1. Include modified bit (dirty bit)
1. Clock algorithm can scan for pages that are clean + set to 0
1. If none is found, then evict those with 0 and dirty bit

## Other VM Policies

* Page selection policy, assuming that if code page P is accessed then most
likely page P+1 may be accessed so the os will prefetch.

* On-demand is what we have seen so far, when the page is needed to be accessed
we call for a replacement / fetch.

* Clustering / grouping is when many pages need to be written to disk, so in-
stead of writting each page back when it needs to we group them into clusters 
and write them all at once. Efficient because of nature of disk drives, it's 
better to perform a large write more efficiently than many small ones.

## Thrashing

* Thrashing: when there just isn't enough space, the system will constantly be
paging.

* Admission control: idea is, it's better to do less work well then everything
at once poorly. So we kill off some subprocesses in the hopes that it free's
up space for progression. Needs mechanism to detect + cope with trashing.

* Draconian approach: used more nowadays, in some linux environments for
example there is a out-of-memory killer (oom reaper). If not enough memory is
avaiable, then kill the most instensive. Obviously this may be bad if we killed
the X server for example rendering display obsolete.

fin
