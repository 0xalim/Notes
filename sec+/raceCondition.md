# Race Condition

Race condition:
1. Developer problem in programming
1. Things working concurrently and race each other
1. Time-of-check to time-of-use (TOCTOU) Attack:
 1. Checking for things that are being modified in the system
 1. Something might happen between check and use

Example:
1. 2 users pulling money from account 1 to 2
1. Transfer 50 from 1 to 2
1. Both do it at almost same time
1. System thinks it contains different value than reality

U1 100/100->100/150->50/200-\>Done
U2  100/100->100/200->50/200-\>Done

Supposed to be a transfer, but 50 dollars extra!

Example1:
1. Rover reboots if something is wrong
1. Fs curropt on boot so infinite reboot
1. Had to enter safe mode manually and fix

Example2:
1. 3 power lines failed at same time, only some got alerted
