---
title: GKE and EKS
date: 2020-09-07 19:41:34
tags: ["aws","gcp"]
categories:
- ["Cloud","Kubernetes"]
---
Recently, I had the chance to upgrade AWS EKS with managed node group, and having worked with GCP GKE before, I came to a conclusion that GKE seems to be more matured overall.
<!-- more -->
A google search to validate my opinion yielded the following Reddit posts:

- <https://www.reddit.com/r/googlecloud/comments/ejxxn5/has_anyone_done_a_thorough_diff_analysis_between/>
- <https://www.reddit.com/r/kubernetes/comments/9qg1wz/how_good_is_eks_vs_gce/>

A quick look through the comments and we can identify that most users have a more positive experience with GKE.

From my experience, i had the following peeves about AWS EKS:

- we need to maintain add-ons installation and upgrade on the cluster, by add-ons i mean stuff like cluster-autoscaler, kube-proxy, kube-dns... etc
  - this took the managed experience out of AWS EKS.
- we have no way to configure the way AWS surges node when performing node eviction, this is directly comparing to <https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-upgrades#surge>
  - there can be situations whereby AWS can overprovision nodes, increasing the amount of time it takes for an upgrade to complete.
- the aws documentation on the update behaviour is at a bare minimum, we do not have an idea of what are the conditions during the upgrade that can cause failure
  - comparing <https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-upgrades#node_pool_upgrades> to <https://docs.aws.amazon.com/eks/latest/userguide/managed-node-update-behavior.html>
- there are additional initial setup that needs to be done during creation of a cluster to get to a good working state, whereas GKE normally provides these setups in terms of cluster configurations through flags.
