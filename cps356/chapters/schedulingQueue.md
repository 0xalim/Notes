# Scheduling: Multi-level Feedback Queue

tries to solve: good turnaround time + good response time (interactive).
Crux: How can we build a scheduler minimize both response time + turnaround time
without knowing runtime of jobs prior?

# mlfq basic rules:

1. If prioA > prioB; A runs B doesn't
1. If prioA = prioB; A & B in rr

# simulation
	1. start job; if cpu heavy then lower prio
	1. if job takes little time (less than timer-interrupt) keep prio
	
# How to change prio?
	1. rules to keep consistency:
		1. when job enters system, place in highest prio
		1. if job uses entire slice, reduce prio
		1. if job gives up cpu before time slice, stay in prio

	1. Example 1: Single long running job:
		1. job enters highest prio
		1. after time slice drop prio
		1. keep dropping if longer than time slice
		1. finally lowest prio

	1. Example 2: Along came a short job:
		1. assume a running for some time; meaning it dropped to lowest
			possible prio
		1. job b comes along and schedular assumes it's a short job
			so run it
		1. b drops in prio if needed but ends up higher prio than a
		1. then complete a as needed

	1. schedular assumption: need to assume the arriving job is a short job
		everytime and drop prio as needed

# What about I/O?
	1. If requests i/o before time slice then keep priority
	1. good because if the job is an interactive one, it keeps response time

# Issues with current system:
	1. starvation: too many interactive jobs prevent longer jobs from ever
		running (thus starving the job).
	1. users could rewrite program to perform i/o operation before time
		slice, even when it doesn't need to thus keeping prio of 
		job high.
	1. programs can change behaviour overtime, thus changing from heavy cpu
		user to an interactive job.


# Priority boost:
	1. to prevent starvation: periodically reset priority; throwing all
		jobs to top of queue again. This prevent jobs that are now 
		interactive jobs sitting at lowest prio.
	1. anothe issue arrises: how long to set S (periodic reset timer)?

# Better Accounting
	1. to prevent gaming the scheduler: give all jobs a time allotment
		so that their overall timer matters instead of short burst
		timer. 

# Tuning mlfq timers:
	1. Different systems use different methods of values for these 
		variables (S, timer-interrupt, etc)
		1. Solaris: Time sharing scheduling class configurable table
		1. FreeBSD: using math formulae + decay-usage algorithm
		1. Os gets highest prio, user jobs never.
		1.  Using 'nice' command

fin
