---
title: "How to use the Kubecost extension with Lens IDE"
description: "Monitor the costs of Kubernetes resources directly from
Lens IDE with the open source Kubecost extension."
date: 2021-06-15T11:34:00-04:00
canonical_url: "http://blog.kubecost.com/blog/lens-kubecost/"
classes: wide
categories:
  - blog
tags:
  - IDE
  - Lens
  - Developers
  - Engineers
---

[Lens](https://k8slens.dev/) is the most powerful IDE for those who need to deal with Kubernetes clusters on a daily basis. It allows you to interact with your cluster and view important metrics, including memory, compute, etc. A Kubecost and Lens integration allows you to visualize Kubernetes costs directly in the Lens UI. With Lens and Kubecost you can view costs and spend efficiency by namespace, pod, deployment and more!

For most teams, it’s really difficult to understand the cost of Kubernetes workloads in an even somewhat static multi-tenant environment. When concepts like autoscaling are introduced, it becomes far more complex. This creates many challenges, including unintentional overspending, poor visibility for business planning, and missing costly bugs/mistakes 

The [open source integration](https://github.com/kubecost/kubecost-lens-extension) provides a direct view into these costs at different levels so you can understand what is driving overall costs and how efficiently they are being spent. In this first release you can see cost trends for any workload aggregation, e.g. pod, deployment, statefulset, namespace, etc. 

<video src="/assets/images/2021-06-10-lens-kubecost/lens-kubecost-extension.mov" width="860" height="537" controls preload></video>

## Install Kubecost in your Kubernetes cluster using Lens IDE

Get started by installing the Kubecost stack in your cluster. Open Lens and connect to your Kubernetes cluster.  Go to the Apps directory and search for the _cost-analyzer_ chart.

![Find Kubecost from the Lens Apps directory](/assets/images/2021-06-10-lens-kubecost/image3.png "Install Kubecost from the Lens Apps directory")


Create a namespace called “kubecost” and install Kubecost cost-analyzer in it

![Install Kubecost in the kubecost namespace](/assets/images/2021-06-10-lens-kubecost/image5.png "Install Kubecost in the kubecost namespace")

Once Kubecost is installed, you can add the Kubecost extension


*   Download the [latest tar.gz file release from GitHub](https://github.com/kubecost/kubecost-lens-extension/releases). Alternatively, just copy the download URL of the release artifact.
*   Open Lens application and select Lens menu, and then click Extensions item, or press Shift + Command + E to open the Manage Lens Extensions page.
*   Specify the path (.tar, .tgz) or download URL to the extension package and click Install.

![Install Kubecost Lens extension](/assets/images/2021-06-10-lens-kubecost/image7.png "Install Kubecost Lens extension")

This will add a new panel in Lens UI showing costs of resources per namespace. 

![See the new Costs panel in the Lens UI](/assets/images/2021-06-10-lens-kubecost/image6.png "See the new Costs panel in the Lens UI")


To see the full Kubecost UI, from the Lens terminal, setup a port forward to your Kubernetes cluster with kubectl:

```

kubectl port-forward --namespace kubecost deployment/kubecost-cost-analyzer 9090

```

Open your browser pointing at [http://localhost:9090](http://localhost:9090) to view cost data.


![The Kubecost GUI in the browser](/assets/images/2021-06-10-lens-kubecost/image2.png "The Kubecost GUI in the browser")


This installs the open source version of Kubecost which is good for monitoring one cluster with a default metric retention of 15 days. Other features are available in the [Business and Enterprise versions](https://www.kubecost.com/pricing/) of the product. Join [Kubecost Slack community](https://join.slack.com/t/kubecost/shared_invite/enQtNTA2MjQ1NDUyODE5LWFjYzIzNWE4MDkzMmUyZGU4NjkwMzMyMjIyM2E0NGNmYjExZjBiNjk1YzY5ZDI0ZTNhZDg4NjlkMGRkYzFlZTU) or [contact us](mailto:team@kubecost.com) if you want more! 


