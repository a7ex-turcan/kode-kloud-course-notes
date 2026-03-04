# Docker intro

## Overview

* Run each component in separate container
* Makes devs lives easier

### Containers
* completely isolated envs
* their own processes
* their own networks, mounts
* all containers use the same OS kernen

* Docker utilizes LXC containers

### Containers vs VMs
* Each virtual machine has its own OS
* Docker reuses the host's kernen
* VMs use a hypervisor
* Docker doesn't
* VMs have higher resource consumption
* Docker has less isolation 

### Containers vs Images
* Images are basically blueprints for containers
* You can have any numbers of containers running for the same image
 
## Getting started with docker
* avaialble on any major OS
 
### installing on a linux machine
* the easiest way is to run the convenience script