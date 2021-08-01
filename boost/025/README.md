# Day 25 - 20210729222413

Terminal Shell Customization

## Overview

1. New ubuntu docker container for customizing config files
1. Finally starting to work on files and make progress
1. Use [freedesktop](https://freedesktop.org) for file infrastructure
1. Hidden directories and files:
 1. edit /etc/skel 	- every new user gets these files in their /home
 1. .bashrc
 1. .bash\_profile
 1. profile
 1. Collection of scripts + writing own automated scripts for daily use
1. Vim configurations
 1. .vim/.exrc
 1. .vim
 1. .vimplugins
 1. Snippets
1. TMUX
1. Using symlinks to centralize
1. Above and beyond
 1. fully automated
 1. all on github - centralized space for pulling on new systems
 1. all configuration files get symlinked on new system + all automated

## Docker

* Start service

```
sudo systemctl start docker
sudo systemctl status docker
```
* Use already created docker

```
sudo docker start hell
sudo docker attach hell
```

## First things

1. Unminimize system for man pages
1. Update system
1. Install vim
1. Create new user if needed; root is fine due to it being a container
