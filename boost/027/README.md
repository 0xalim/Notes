# Day 027 - 20210803231530

Colorizing pager; files and directories

## Overview:

1. Add terminal escapes to special variables for less
 1. Never use `MANPAGER` color method
1. Add directory colors using dircolor
 1. Use `command -v dircolors; echo $?` to check if it exists
1. Add custom ~/.dircolors
 1. Use curl to get files off the internet (.dircolors)


### Colorozing pagers

```sh
export LESS\_TERMCAP\_md= "[35m" # magenta
```

Example code for magenta color up there bossman ^

### Colorizing directories

```sh
if command -a dircolors &>/dev/null; then
	if test -r  ~/.config/dircolors; then
		eval "$(dircolors -b ~/.config/dircolors)"
	else
		eval "$(dircolors -b)"
	fi
fi
```

The above code checks if you do have dircolors installed, could be done a lot
simpler if needed. Check ./bashrc.

Should create a .profiles to keep files in .dircolors in. A custom .dircolors
can be created or just pick one off the internet and save it in .profiles or 
w/e custom file for configuration files like this. Make sure to change the 
directory path in .bashrc.
