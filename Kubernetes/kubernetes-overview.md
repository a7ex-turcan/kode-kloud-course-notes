# Kubernetes

> A container orchestration technology

## Container orchestration

* Automatizes the deploying and the management of container
* Allows for high availability
* Allows load balancing
* Allows scaling the phisical nodes easily

## Architecture

### Nodes

* A machine - phisical or virtual
* A worker node
* You usually need more than one node to

### Cluster

* A group of nodes grouped together
* Allows for high availability and scalability

## Master Node

* watches over the worker nodes
* is responsible for the workestration of containers on the worker node

## Components of a cluster

### API Server

* the frontend for k8s

### etcd

* a keystore used to store the data that k8s requires
* responsible for implementing locks, so there are no conflicts between the masters

### Kubelet

* the agend that runs on each node in the cluster

### Container runtime

* the underlying software tha runs the containers, one of the following:
    * containerD
    * rkt
    * cri-o

### Controller

* contains the logic of managing containers

### Scheduler

* distributes works accross nodes

### Master vs Worker Nodes

#### The worker nodes

* has a container runtime
* has the kubelet agent

#### The master node

* has the kube-apiserver
* has the etcs
* has the controller
* has the scheduler

### The `kubectl` command

The main command used to manage a cluter

Example:

```bash
# runs a container
kubectl run hello-minikube

# returns the info about the cluster
kubectl cluster-info

# list all nodes
kubectl get nodes
```

## Docker vs ContainerD

* k8s used to work only with docker
* once other containerization technologies (rkt, etc.) became popular, k8s introduced the `Container Runtime Interface (CRI)`

* This allowed any vendor to work as container runtime as long as they adhered to the Open Container Initiative (OCI):
    * `imagespec` - the specification of how an image should be built
    * `runtimespec` - how any container runtime should be developed

* to continue to support docker, k8s introduced `dockershim`
* in more recent versions - support for docker has been removed in favor of `containerD`

### Running containers with just containerD

#### The `ctr` cli tool

* quite limitted
* mainly for debugging containerd

Example:

```bash
ctr image pull
```

#### The `nerdctl`

* provides a "docker-like" CLI for containerd
* supports docker compose
* pretty much mirrors docker cli instructions

#### The `crictl`

* provides a way to interact with CRI compatible runtimes
* it's a tool developed by k8s
* it's a debugging tool
* is also aware of pods
* also follows the same syntax as the docker `cli`
Example:

```bash
crictl pull busybox
```
