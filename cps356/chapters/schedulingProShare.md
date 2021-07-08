# Scheduling: Proportional Share

Fair-share scheduler instead of turnaround or response time, this scheduler
instead focuses on giving every job a % of cpu time.

Every so often, hold a lottery to determine the next running process, but
processes that should run more often get more chances to win the lottery.

Crux: How to design a scheduler that shares cpu time proportionally? What are
the mechanisms + how effective are they?

# Basic concept: Tickets represent your share
1. tickets: used to represent the share of a resource the process 
 should receive. e.g A = 75 tickets (75%), B = 25 tickets (25%)
1. this is a nondeterministic method.
1. scheduler must know total number of tickets.
1. then chooses number between 0 - 99

# Ticket mechanisms:
1. ticket currency: allows user to ticket to allocate tickets among their 
 own jobs (using w/e currency)
1. auto converts w/e currency to global value. e.g:
 1. A = 100 tickets {shared A1 = 500 A2 = 500; total = 1000}
 1. B = 10 tickets {B = 10; total = 10}
 1. convert:
  1. A1 = 50 global
  1. A2 = 50 global
  1. B  = 100 global


1. ticket transfer:
 1. jobs can transfer ticket to highten prio. e.g:
  1. client transfers ticket to server
  1. server completes job with higher prio
  1. server transfers ticket back to client

1. ticket inflation:
 1. in environment where process has other processes trust it temp inflate
     tickets if it needs more cpu time. good because no communication 
     occurs between processes.

# Implementation:
1. each process has x number of tickets
1. choose random number as winner (harder than it seems)
1. have counter start at 0 and count up x number of process tickets
1. until counter > winner, thus the process it's on is the winner

# Unfair metric:
1. simulate example with 2 jobs a, b with equal number of tickets
1. one has to finish before the other due to the lottery system
1. calculate unfairness metric via Aendtime / Bendtime = U
1. Best scenario is U = 1 (not possible, but closest is better)
1. running simulation from 1 to 1000 R (runtime), the lower the runtime the 
 farther U is from 1. 

# Why not deterministic?
1. randomness sometimes doesn't deliver, esp. if short time scale.
1. stride scheduling: deterministic fair-share scheduler
 1. stride inverse proportional to ticket number (largeX/tickets)
 1. pick process with lowest pass value to run next pass value is 
     counter (init=0)
 1. increment pass with stride value after process is done

1. issue with stride scheduling: has global state. when new process enters, 
 the pass value would = 0, thus takes all cpu until another pass 
 value is lower.

# Linux Completely Fair Scheduler
1. advanatge of cfs over others: little overhead (5% only); due to:
 1. good datastructure
 1. inherent design

1. divide cpu time evenly among all process
1. counting-based virtual runtime (vruntime)
1. process runs += vruntime (real time)
1. pick process to run next by lowest vruntime

1. how does cfs know when to stop running current process?
 1. sched_latency: parameter determining slice time (dynamic)
 1. typical sched_latency = 48ms
 1. 48ms / n (number of process) to divide slice time per process

1. what if high *n*?
 1. min_granularity: 6ms default (process cannot have time slice less
     than this value)

1. cfs timer-interrupt = 1ms that it wakes up and makes decisions

# Weighting (niceness)
1. cfs  gives control of process priority to users/admins allowing them 
 to change the 'nice' value (-20 -> 19)

1. in book shows examples of running through tiime.slice(k) equation
 and vruntime(i). Eq. shown in book fig 9.1+2

# Red-black trees
1. cfs uses rb-trees as data structure with low depth to search through
 possible proces.
 1. only running/runnable processes are in this tree
 1. if process sleeps/performs i/o it will drop it from the tree
 1. rb-tree remember are logarithmic + balancedh

# Dealin with i/o + sleeping processes

process A continuously runs while B goes to sleep for 10s, after B wakes
up it will have vruntime 10s less than A. meaning cfs will choose this and 
it will hog the cpu time instead, cfs pushes it into the minimum value
in the tree avoiding starvation. however, jobs that sleep for short time 
frequently don't get their fair share of cpu

fin
