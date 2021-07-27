---
title: "How to get the complete picture when monitoring Kubernetes costs"
date: 2020-11-16T11:34:30-04:00
canonical_url: "http://blog.kubecost.com/blog/complete-picture-when-monitoring-kubernetes-costs/"
classes: wide
categories:
  - blog
tags:
  - cost monitoring
  - Kubecost
---

[Kubecost](http://kubecost.com) now provides more than one thousand teams with visibility into their Kubernetes cost, efficiency, and resource consumption. 
Yet many teams, including our own, still spend considerable sums on cloud resources that aren’t visible from within a cluster. 
For that reason, we’re excited to announce a set of new features to help answer the question: besides Kubernetes resources, where else are we spending our money? 
This new functionality lets teams see all Kubernetes and out of cluster spend on cloud services in a single unified view.

![unified kubernetes cost monitoring](/assets/images/unified-cost-screenshot.gif)
 &nbsp;*A unified view of all Kubernetes assets and external AWS cloud services spend.*

<br/>
This feature is available for free today, is built on open source, and supports AWS and GCP. 
It joins real-time Kubernetes cost data with cloud billing data and allows teams to: 
<br/>

* Without any tagging, see costs broken down by Kubernetes and other cloud services.
* View unified costs by team, department, project, etc by joining costs incurred in a Kubernetes cluster with costs incurred outside, e.g. tagged RDS instances or S3 buckets.
* Drill down to individual Assets in a Kubernetes cluster, including nodes, disks, load balancers and more. These assets and their costs can be tied to owners with Kubecost Allocation APIs.
* Drill down to all out-of-cluster cloud resources, e.g. Lambda, DynamoDB, or storage buckets, to determine detailed cost drivers
* Supports multi-cluster, multi-account, and multi-provider views. Feature available on AWS, GCP, Azure and hybrid-cloud today.
* Tightly integrate on-prem Kubernetes clusters with cloud services for hybrid environments 

## How to Set Up

1. [Install Kubecost](http://docs.kubecost.com/install) for free (typically < 5 mins)
2. Connect a cloud account
    - [AWS integration](https://github.com/kubecost/docs/blob/master/aws-cloud-integrations.md)
    - [GCP integration](https://github.com/kubecost/docs/blob/master/gcp-out-of-cluster.md)
    - [Azure integration](https://docs.kubecost.com/azure-out-of-cluster.html)

After completing these steps, you will be able to view your unified costs in the [Kubecost Assets UI](https://github.com/kubecost/docs/blob/master/assets.md) as well as the backing JSON API. 

As always with Kubecost, you own and control your data and never have to share any information with remote servers.

## About Us

Kubecost provides cost visibility and cost optimization solutions built specifically for teams running Kubernetes. 
To learn more, we’ll be demoing and answering questions at [Kubecon North America](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/) so drop by our virtual booth in Startup Hall C. 
You can also email us at team@kubecost.com or join our [Slack community](https://join.slack.com/t/kubecost/shared_invite/enQtNTA2MjQ1NDUyODE5LWFjYzIzNWE4MDkzMmUyZGU4NjkwMzMyMjIyM2E0NGNmYjExZjBiNjk1YzY5ZDI0ZTNhZDg4NjlkMGRkYzFlZTU) at any time! 
