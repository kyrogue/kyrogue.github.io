---
title: Kubernetes In Docker
date: 2020-09-13 22:06:13
tags: ["Docker","minikube","kind"]
categories:
- ["Tools"]
---
Recently, I switched from using minikube to [kind](https://kind.sigs.k8s.io/docs/user/quick-start/), kind is much more lightweight compared to using minikube for spinning up a local kubernetes cluster for testing purposes.
<!-- more -->
This is because kind creates the k8s cluster using the installed docker engine on your workstation, instead of minikube which virtualizes an entire VM.

I was also able to quickly spin up a multi-node k8s cluster with 1 control plane node and 2 worker node, this was good for me to test multi-node scheduling behaviour, and stuff such as testing application behaviour when k8s reschedules a pod onto another node when performing [voluntary disruptions](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/#voluntary-and-involuntary-disruptions).

The following sections demonstrates setting up a multi-node k8s cluster and mounting your workstation directory to the nodes

Create a cluster config yaml file, to be used when creating the cluster:

{% code cluster-config.yaml %}
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraMounts:
  - hostPath: /path/to/my/files/
    containerPath: /files
- role: worker
  extraMounts:
  - hostPath: /path/to/my/files/
    containerPath: /files
{% endcode %}

We can then use the ```kind``` command, along with providing the cluster to be using v1.16 of k8s:

```kind create cluster --config cluster-config.yaml --image kindest/node:v1.16.9```

To use the mounted volume on your k8s pods, we would then use [hostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) volumes and attach it to your pod.
