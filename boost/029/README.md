# Day 29 - 20210804202228

Aliases and Exported Bash Functions

## Overview:

1. Create and use aliases
1. Why aliases > functions
1. Use `shopt expand aliases` it is enabled by default - I think
1. Shellshock

### Create aliases

`alias="example command"`

Aliases can be written in bashrc, main use is to have smaller commands that
are dynamic (change if needed) instead of writting larger commands.

### Functions

Functions in .bashrc are insta loaded into memory as soon as sourced; so they 
are faster than script functions. Major difference between script functions and
functions in .bashrc is that in scripts it starts a subshell and does whatever
instructions. So example, using `cd` in script in function does not work on 
parent. But writing function in .bashrc that uses cd will work.

### Expanded\_aliases

To use alises defined in ~/.bashrc in other scripts you need to:
1. `shopt -s expand_aliases` in the script
1. source ~/.bashrc

OR

* make the alias a function && export *function name*
