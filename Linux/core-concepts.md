# Files
>
> every thing is a file in linux

## Types of file

* Regular (images, scripts, config/data etc.)
* Directory (home/bob, /root, etc,)
* Special files
  * character files (serial devices: mice etc, located under /dev)
  * block files (block devices, located under /dev)
    * links
      * hard links
      * soft link (sim link) -> pointer to another file
  * socket file
    * named pipes - communication between processes in a unidirectional manner

## Commands

* `file` - displays the file type
* `ls -ld` 

# Filesystem Hierarchy
* `/home` - home dir for all users except the root user
* `/opt` - for 3rd party software
* `/mnt` - temporary location for mounts
* `/tmp` - teporary files
* `/media` - external media (USB sticks etc.). use `df -hP` to print all the mounted file sytem
* `/dev` - device files
* `/bin` - basic programs and commands
* `/etc` - configs
* `/lib` - shared libraries
* `/usr` - user land application and their data
* `/var` - logs, and cached data