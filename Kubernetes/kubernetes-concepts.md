# Kubernetes Concepts

## Setting up kubernetes

### Local setup

* Use one of the following: docker desktop, minkube, kubeadm

### In cloud

* GCP: GKE
* AWS: EKS
* Azure: AKS

### Playgrounds

* kodekloud.com/k8s

## Pods

* Encapsulates containers
* A single instance of an application
* The smallest thing you can creat in k8s
* To scale an application you create more pods of the same app
* Pods can be deployed on any node of the cluster
* usually a 1:1 relationship to an application
* a pod can have multiple containers, but usually not of the same kind

* To run a pod:

```bash
kubectl run nginx --image nginx
```

* docker hub servers the image

* To get all pods

```bash
kubectl get pods
```
