# Day 26 - 20210731212055

## Overview

1. why containers are best environment for learning
1. modify configurations to learn
1. push container to github if needed
1. create new user

### New user

* creating new user 'name'
```
usermod 'name'
```

* creating home directory
```
sudo mkhomedir_helper 'name'
```

* logging in as user 'name'
```
su - 'name'
```

* adding user to sudo
```
visudo
```

### .bash\* files

* .bash\_history: shows previous commands used
* .profile: script gets run when the user logs in	// not needed
* .bash\_logout: script gets run when the user logs out // not needed
* .bashrc: gets run when *exec bash* runs

during the next couple days we will be **renovating** our bashrc file so we 
can remove everything in it for now, except the globbing for interactive prompt

* tip: printing a command and replacing ':' with a \n
```
echo "{$PATH//:/\\n}"
```

that is bash parameter expansion, can use perl sed, awk, sh parameter expansion
or whatever you want (which is not posix complaint)

### bashrc file

things currently in my bashrc file: (may be updated)
1. case: \*i\* interactive shell; otherwise return
1. environment variables
 1. term set to xterm-256color
 1. editor set to vim
 1. visual set to vim
1. pager: think for colors?
 1. no color options set 
 1. test -x for /usr/bin/lesspipe
1. shell options: (may be updated - more to be added)
 1. globstar
1. aliases:
 1. c = clear

fin
