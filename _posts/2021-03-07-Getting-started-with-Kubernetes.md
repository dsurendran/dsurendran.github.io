---
layout: post
title:  "Getting started with kubernetes commands"
date:   2021-03-07 10:53:21 +0530
categories: jekyll update
---


With a basic understanding of kubernetes and the concepts like namespaces, context and pods, This blog will provide with set of command to get started.

# Starting a local minikube cluster

```console
minikube start
```

minikube supports multiple drivers to start minikube with docker driver and with 40GB of disk space allocation.


```console
 minikube start --driver=docker --disk-size=40g
```

## start a multinode cluster

```console
start --driver=docker --disk-size=20g --nodes 2 -p multinode-demo
```

## get list of nodes
```console
kubectl get nodes
```

# Namespaces

Namespaces provide logical separation of resources within the cluster. Resources within the namespace cannot overlap with other namespaces they are unique within a namespace

```console
kubectl create namespace demo
```

# Context

Kubernetes context allows to group access parameters with a name
Parameters like namespace, user.
Current context can be retrived from ```$HOME/.kube/config```

To see the current context

```console
kubectx
```

We can switch the contexts by

```console 
kubectx multinode-demo
```

# Pods
https://kubernetes.io/docs/concepts/workloads/pods/

To view the list of pods under the cluster

```config
kubectl get pods
```

As of now there won't be any pods running.
We can create pods using 

```console
kubectl apply -f \
https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

This creates kubernetes dashboard in new namespace ```kubernetes-dashboard```

The current context should use the newly created namespace to list the available pods

```console
kubectl config set-context --current --namespace=kubernetes-dashboard
```

kubectl get pods should list 2 pods 

```
~❯ kubectl get pods                                  
NAME                                         READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-7b59f7d4df-8pp7q   1/1     Running   0          4m25s
kubernetes-dashboard-74d688b6bc-pkfsh        1/1     Running   0          4m25s
```

We can view the individual logs of kubernetes using ```kubectl logs pod-name```

```console
kubectl logs dashboard-metrics-scraper-7b59f7d4df-8pp7q 
```


switch namespaces - ```kubectx <namespace>```

Port forwarding

```console
kubectl port-forward <pod-name> local-port:remote-port
```
