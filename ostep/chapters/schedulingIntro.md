# Scheduling Introduction

*crux: how to develop a framework for thinking about scheduling policies? key 
assumptions? what approaches have been used already*

workload assumptions: {about processes}
1. end goal: fully operational scheduling discipline
1. assumptions:
 1. each job runs for x amount of time
 1. all jobs arrive at same time
 1. once started, each job runs till completion
 1. all jobs only use cpu (no i/o)
 1. run-time of each job is known

scheduling metric:
* metric is something we use to measure something
1. simple metric: turnaround time (performance based scheduling)
 1.  time of completion - time of birth
 1. since all jobs start at same time; t-birth = 0
 1. ultimately t-turnaround = t-completion
1. fairness metric
 1. will look at later

### Turn around time

first in first out (fifo):
1. drop assumption 1:
 1. the convoy effect: where short consumers get queued behind a heavy consumer
 1. similar to being at cashier and having to wait for month groceries, to pay
    for 1 single item

shortest job first:
1. runs the shortest job first, then the second shortest, etc
1. drop assumption 2:
 1. if the heavyweight consumer arrives before the lesser consumer then it is
    not any different then fifo (convoy affect)
1. issue: not preemtive

shortest time to completion first:
1. drop assumption 3 + add timer interrupts + context switching
1. can preempt job runs, run a shorter job and maybe continues the job it dropped
   later on

### Response time

response time:
 1. t-response = t-scheduler - t-arrival

round robin:
1. runs a job for a time slice (or time quantum) then switches to the next
1. repeat switching until all jobs are finished
 1. length of time slice has to be multiple of timer-interrupt
1. bad turn around rate (A finished at 13, B 14, C 15, so on)
1. very good response time

comparison: 
1. always a trade off; can't have your cake and eat it too
1. shortest job first + shortest completion time first = good turn around, bad
   response time
1. round robin = good response time bad turn around

### Taking I/O into account

incorporating i/o:
1. bad to run job A (as it requests i/o) and wait idling till it's done
 1. scheduler could make decisions to run A after i/o done or continue with
    with B
1. good idea is to allow for overlap, while cpu is being used for job x while
   waiting for i/o of another process to complete via splitting job x into sub
   jobs


### Final Assumptions

*crux: removing the assumptions that we know how long each job runs for, how
do we build approaches for sjf/stcf without this information?*
