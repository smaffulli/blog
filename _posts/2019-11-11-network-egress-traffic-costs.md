---
title: "Measuring Kubernetes cluster egress & network cost"
date: 2019-11-11T08:34:30-04:00
canonical_url: "http://blog.kubecost.com/blog/network-egress-traffic-costs/"
classes: wide
categories:
  - blog
tags:
  - cost monitoring
  - Kubecost
---

Late last year, we had an unpredicted experience while on-boarding a gaming company to the [Kubecost](http://kubecost.com) platform. 
A principal engineer on their team was investigating latency in a related service and discovered a large amount of network traffic to a country where they had little to no users. 
Following this suspicious trail, he eventually discovered cryptomining malware in their Kubernetes cluster that was installed after exploiting a docker vulnerability. 
The infected container had been cleaning its traces for weeks and it was only this network traffic that tipped their team off.

The fallout from this situation inspired our team to get out and talk to our users about how they protect against this risk. 
We wanted to both share the lessons we learned from this experience and to learn about how other users were monitoring network egress.
This issue was important to us because of its impact on both security/privacy as well as cost management. 
What we learned surprised us. 
We found that most teams have no insight into their cluster egress and network costs, operating as if they were a black box.
Specifically, it was really hard for them to determine what jobs/pods were creating internet egress or cross region traffic. 

## Our solution

This feedback inspired the **launch of the Kubecost network tracker** last month. 
This lightweight tool helps teams gain visibility into their network egress. 
It’s already being used to determine the root cause of network cost increases and identify pods that are unwantedly sharing data outside of k8s clusters.

![network cost allocation](/assets/images/network-cost-allocation.png)

Network traffic and cost visibility is available by Kubernetes namespace, deployment, pod/job, and even IP address. 
This feature can be turned on in seconds ([docs](http://docs.kubecost.com/network-allocation)) after 
completing the [installation](http://kubecost.com/install) for the free or paid version of Kubecost. It is available for Kubernetes clusters on AWS, GCP, and Azure. 

## How it works

This feature collects data by connecting directly to the Linux kernel via the nf_conntrack module. It builds a map within your cluster to determine all known IP addresses. Any unknown IP addresses are assumed to be internet egress. For known IP addresses in your VPC, we enable whitelisting CIDR blocks or a list of IP addresses. 


We hope this tool is useful for you in avoiding major privacy/security issues and cost overruns! Please reach out (team@kubecost.com) if we can help or if you have any questions!
