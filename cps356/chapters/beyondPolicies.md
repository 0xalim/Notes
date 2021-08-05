# Beyond Physical Memory: Policies

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

### Optimal Replacement Policy

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

### Workload Examples

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
