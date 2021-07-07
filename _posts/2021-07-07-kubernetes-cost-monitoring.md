---
title: "On-Premise Kubernetes Cost Monitoring"
description: "Learn how to manage the cost of on-premise Kubernetes clusters by using Kubecost to allocate costs to workloads, set budgets, get reports, and alert on overages."
date: 2021-07-07T8:00:00-04:00
canonical_url: "https://blog.kubecost.com/blog/kubernetes-cost-monitoring"
classes: wide
categories:
  - blog
tags:
  - Finops
  - Security
  - On-prem
  - Monitoring
---

With the power of automated orchestration, Kubernetes (a.k.a, K8s) has solved many of the complex container management problems administrators faced and unleashed a global adoption of containerized workloads. The next frontier in container management has shifted to monitoring the ever-expanding costs of Kubernetes clusters. 

In this article, we specifically explain how to track the costs of an on-premise Kubernetes cluster, and how to use [Kubecost](https://www.kubecost.com/) as an open-source tool to automate much of this process. 

Monitoring the cost of a Kubernetes workload requires a process involving multiple steps:

1. Assign an hourly operating cost to the K8s cluster and its supporting assets
2. Track usage by cluster tenant by using features such as labels or namespaces 
3. Allocate resource usage and associated costs to each cluster tenant
4. Detect spending overages relative to a specified per-tenant daily budget 
5. Monitor health to avoid performance bottlenecks while reducing costs

However, monitoring the cost of a cluster residing in a data center is more challenging than a cluster hosted in a public cloud simply metered based on an hourly price. The total cost of ownership of an on-premise cluster is more difficult to assess due to a mix of capital expenditure, operating expense, and labor cost. It’s also the starting point for monitoring the cost of a Kubernetes cluster, as shown below.

![Five stages to on-premise kubernetes cost monitoring](/assets/images/2021-07-07-onprem/image2.png "Five stages to on-premise kubernetes cost monitoring")

This article will explain each of the five stages shown in the diagram above and introduce Kubecost.

## 1. Assign Value 

A first step in monitoring the cost of a Kubernetes cluster is by assigning value to the total cost of owning the underlying cluster nodes including the attached storage underlying network. But, let’s first gain context by reviewing the price of a Kubernetes cluster hosted in a public cloud using AWS pricing as an example. 


### Hosted Kubernetes Pricing

Customers pay $0.10 per hour to use an AWS EKS cluster and pay for the provisioned EC2 instance and EBS storage nodes. The minute you terminate the cluster and the underlying nodes, the AWS charges stop for that cluster. Tools such as the [AWS Cost Explorer ](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) and the [Cost and Usage Report ](https://aws.amazon.com/aws-cost-management/aws-cost-and-usage-reporting/)(CUR) record and display the costs. So it’s easy to know how much you have spent each hour of each day. 


### On-Premise Kubernetes Cluster Costs

The cost of a node of an on-premise Kubernetes cluster is more complicated to calculate since you pay for the hardware upfront as a capital expenditure and use the servers (that make up the cluster) for an estimated period of 5 years before disposing of them. You may also install licensed software such as a Windows operating system. Installing the servers in a data center also requires space, power, and cooling. In addition, you must account for the labor costs to install and maintain the server over time. 

As you can see, various line items must be accounted for and added together to derive the final daily cost of using an on-premise Kubernetes cluster. The table below summarizes these costs over five years and amortizes (or spreads) those costs over months, days, and hours by simply dividing them. For example, the upfront server cost is first divided by 5 to reflect an annual cost, and then it’s divided by 12 to show its amortized monthly cost. We have arbitrarily chosen five years because of our earlier assumption that the server hardware will be disposed of after five years to purchase a newer model. 


<table>
  <thead>
  <tr>
   <th>Cost Item
   </th>
   <th>Over 5 years
   </th>
   <th>Per month
   </th>
   <th>Per day
   </th>
   <th>Per Hour
   </th>
   <th>Note
   </th>
  </tr>
 </thead>
  <tr>
   <td>Server
   </td>
   <td><p style="text-align: right">
$1,500</p>

   </td>
   <td><p style="text-align: right">
$25</p>

   </td>
   <td><p style="text-align: right">
$0.83</p>

   </td>
   <td><p style="text-align: right">
$0.03</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>Windows Server Edition license
   </td>
   <td><p style="text-align: right">
$900</p>

   </td>
   <td><p style="text-align: right">
$15</p>

   </td>
   <td><p style="text-align: right">
$0.50</p>

   </td>
   <td><p style="text-align: right">
$0.02</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>SSD Storage
   </td>
   <td><p style="text-align: right">
$750</p>

   </td>
   <td><p style="text-align: right">
$13</p>

   </td>
   <td><p style="text-align: right">
$0.42</p>

   </td>
   <td><p style="text-align: right">
$0.02</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>Router Port
   </td>
   <td><p style="text-align: right">
$250</p>

   </td>
   <td><p style="text-align: right">
$4</p>

   </td>
   <td><p style="text-align: right">
$0.14</p>

   </td>
   <td><p style="text-align: right">
$0.01</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>Cabling
   </td>
   <td><p style="text-align: right">
$150</p>

   </td>
   <td><p style="text-align: right">
$3</p>

   </td>
   <td><p style="text-align: right">
$0.08</p>

   </td>
   <td><p style="text-align: right">
$0.00</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>Uninterruptible power supply (UPS)
   </td>
   <td><p style="text-align: right">
$100</p>

   </td>
   <td><p style="text-align: right">
$2</p>

   </td>
   <td><p style="text-align: right">
$0.06</p>

   </td>
   <td><p style="text-align: right">
$0.00</p>

   </td>
   <td>Upfront purchase
   </td>
  </tr>
  <tr>
   <td>Installation labor costs
   </td>
   <td><p style="text-align: right">
$250</p>

   </td>
   <td><p style="text-align: right">
$4</p>

   </td>
   <td><p style="text-align: right">
$0.14</p>

   </td>
   <td><p style="text-align: right">
$0.01</p>

   </td>
   <td>One-time payment
   </td>
  </tr>
  <tr>
   <td>Datacenter space, cooling, and power
   </td>
   <td><p style="text-align: right">
$3,000</p>

   </td>
   <td><p style="text-align: right">
$50</p>

   </td>
   <td><p style="text-align: right">
$1.67</p>

   </td>
   <td><p style="text-align: right">
$0.07</p>

   </td>
   <td>Paid monthly
   </td>
  </tr>
  <tr>
   <td>Maintenance labor costs
   </td>
   <td><p style="text-align: right">
$600</p>

   </td>
   <td><p style="text-align: right">
$10</p>

   </td>
   <td><p style="text-align: right">
$0.33</p>

   </td>
   <td><p style="text-align: right">
$0.01</p>

   </td>
   <td>Paid monthly
   </td>
  </tr>
  <tfoot>
  <tr>
   <td><b>Total</b>
   </td>
   <td><p style="text-align: right">$7,500</p></td>
   <td><p style="text-align: right">$125</p></td>
   <td><p style="text-align: right">$4.17</p></td>
   <td><p style="text-align: right">$0.17</p></td>
   <td></td>
  </tr>
  </tfoot>
</table>


The resulting hourly rate of a server installed in a data center should work out to be generally in line with the cost of an EC2 with a similar configuration. The main difference is that you can stop using the EC2 whenever you choose, but you purchase the hardware for a data center and commit to using it over its lifespan. If you virtualize your hardware using a VMware or KVM hypervisor, then you would divide the node cost into the number of virtual machines (VM) provisioned on the physical node, assuming the VMs have an identical configuration. 

You may take one additional step to decompose a node’s monthly cost ($125) shown in the table above into separate computing resources, namely CPU, RAM, GPU, and Storage. Later in this article, we will discuss allocating costs to various Kubernetes cluster tenants. By decomposing costs into computing resource vectors as presented in the table below, a cost allocation report can assign a more accurate value to a GPU-intensive workload than a storage-intensive workload. The decomposition simply uses a percentage-based estimation in our example. Note that the presented values are for illustrative purposes.


<table>
  <thead>
  <tr>
   <th>Computing Resource</th>
   <th>Est. %</th>
   <th>Per month</th>
  </tr>
  </thead>
  <tr>
   <td>Monthly CPU Cost</td>
   <td><p style="text-align: right">25%</p></td>
   <td><p style="text-align: right">$31</p></td>
  </tr>
  <tr>
   <td>Monthly RAM Cost</td>
   <td><p style="text-align: right">25%</p></td>
   <td><p style="text-align: right">$31</p></td>
  </tr>
  <tr>
   <td>Monthly GPU Cost   </td>
   <td><p style="text-align: right">40%</p></td>
   <td><p style="text-align: right">$50</p></td>
  </tr>
  <tr>
   <td>Monthly Storage Cost</td>
   <td><p style="text-align: right">10%</p></td>
   <td><p style="text-align: right">$13</p></td>
  </tr>
  <tfoot>
  <tr>
   <td>Total Node Cost   </td>
   <td><p style="text-align: right">100%</p>   </td>
   <td><p style="text-align: right">$125</p>   </td>
  </tr>
  </tfoot>
</table>



## 2. Organize Workloads 

Departments, teams, applications, and projects typically share one Kubernetes cluster. A shared tenant model saves on the general costs involved in purchasing, provisioning, and operating a cluster. Sharing has implications in administration, security, performance, and reporting. This level of control requires a logical separation or at least a designation of the Kubernetes components owned by each tenant. The most common designation approach is to “label” (similar to tagging) the dedicated [Kubernetes pod](https://kubernetes.io/docs/concepts/workloads/pods/) (along with its containers) hosting the workload. 

In the example below, labels are used to associate a pod with an environment (staging vs. production) and a team (named kube-team).


```
apiVersion: v1
kind: Pod
metadata:
 name: my-pod
 labels:
   environment: staging
   team: kube-team
spec:
 containers:
   - name: my-container
     image: "k8s.gcr.io/my-app:v0.1"
     resources:
       limits:
        cpu: 1
```


Another strategy for larger environments is to create a [Kubernetes namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) for each team or application to offer more configuration independence. In the next example, a kubectl command lists the namespaces provisioned in a cluster, including the namespaces automatically created by the Kubernetes system, such as “kube-system”. 


```
kubectl get namespace
NAME                        STATUS     AGE
default                     Active       1d
kube-node-lease             Active       1d
kube-public                 Active       1d
kube-system                 Active       1d
```

The use of labels and namespaces is necessary within a Kubernetes cluster to allocate resource usage to individual tenants. This topic is the focus of this article’s next section.


## 3. Allocate Usage and Cost 

Once labels identify cluster workloads, the next challenge is to allocate a specific cost to each tenant based on usage. This challenge has many facets since, for each tenant, you must calculate:

1. The usage of CPU, GPU, memory, network, and persistent storage.
2. The amount of requested but unused resources that causes waste.
3. The usage of out-of-cluster resources such as databases or blob storage.
4. The proportionate use of cloud instance reservations and support services.

Let’s take network usage as an example. Native Kubernetes tooling doesn’t calculate egress traffic usage by default. Measuring network activity would require installing a [script (or lightweight agent)](https://blog.kubecost.com/blog/network-egress-traffic-costs/) to collect data directly from the [Linux kernel’s nf_conntrack module](https://www.kernel.org/doc/html/latest/networking/nf_conntrack-sysctl.html). The IP addresses would then have to be mapped to labels and namespaces and then allocated to an individual cluster tenant. Once each unit of traffic (e.g., KiloByte) has an exact value assigned to it, only then can administrators attribute a dollar value to each tenant’s network use. 

This example hints at the magnitude of complexity involved in allocating total costs to each Kubernetes cluster tenant. This challenge led to founding the open-source project [Kubecost](https://www.kubecost.com/) that provides this exact functionality as a core feature. 

## 4. Detect Spending Overages

Cost monitoring is helpful only if it can lead to adherence to a budget. The only way to stay within budget is to stop overspending before it becomes an end-of-month surprise. Once each Kubernetes tenant knows its own allocated daily spending according to usage, each tenant can set a daily budget based on historical use and forecasted business plans. A cost alert would simply compare each day’s allocated charges to a preset daily budget value and generate a notification to alert the right person who can decide how to reduce usage to stay within the daily budget. 

[Kubecost](https://www.kubecost.com/) offers four types of alerts to help with this process:

* **Recurring Update**: Schedule reports of per-tenant spending. 
* **Budget**: Detect a budget overrun relative to a defined threshold.
* **Spend Change**: Detect a jump relative to a historical [moving average](https://en.wikipedia.org/wiki/Moving_average). 
* **Efficiency**: Detect over and under-used computing resources. 


## 5. Monitor Health

All of the focus on staying within budget and reducing unnecessary spending can inadvertently create resource bottlenecks during critical peak business hours. The best way to ensure cost management measures aren’t cutting too deep into performance is to rely on health checks configured based on best practices and customized policies. 

For example, comparing CPU and memory usage over the past 24 hours and comparing it to provisioned capacity would indicate if the workloads lack the required headroom to operate safely. Another example is tracking out-of-memory (OOM) events and detecting an increase in their occurrence over ten minutes. One more example is a policy that would follow the total requested memory by all workloads and compare it to the overall available cluster memory. When these two numbers are too close, it would mean that the failure of one node would create havoc in the entire cluster. 

Kubecost pre-populates its product with [health checks](http://docs.kubecost.com/alerts.html#type-cluster-health) based on industry best practices to track various performance and security aspects of a Kubernetes cluster so that users can feel safe and be alerted of any violations while reducing unnecessary spending. 


## Getting Started With Kubecost


### Installing Kubecost

As referenced earlier in this article, Kubecost is a free and open-source tool designed to monitor your on-premise cluster costs. The usage data it collects never leaves your cluster, so there is no need to worry about security and privacy. 

To get started:

1. Use a Helm chart to install Kubecost (it should take about 5 minutes)
2. Enter estimated costs for your cluster’s on-premise infrastructure (see below)

Kubecost is available for download as a Helm chart at this link that contains further instructions: [https://www.kubecost.com/install.html](https://www.kubecost.com/install.html). 

### Entering Custom Pricing Values

Once Kubecost is installed, you can navigate to the settings page (accessible in the lower left of the user interface) and “enable custom pricing,” as shown below. 

![Enable custom pricing](/assets/images/2021-07-07-onprem/image6.png "Enable Custom Pricing")

Once custom pricing is enabled, you will see the form below to enter your estimated custom pricing for your cluster nodes’ computing resources. The values shown in the screenshot below are meant for illustrative purposes. 

![Enter the custom prices](/assets/images/2021-07-07-onprem/image5.png "Enter the custom prices")

In the first section of this article, we introduced an approach for calculating the costs of a node supporting a Kubernetes cluster and decomposing it into cost values for CPU, RAM, GPU, and Storage. By following the approach that we introduced, you can calculate the cost of a hardware node used in your data center, decompose that cost into computing resources (CPU, RAM, GPU, and storage), and enter them in the form shown above. It may be helpful to know that Kubecost also accepts a CSV file to input custom pricing values. This feature is included in both the Business and Enterprise [pricing tiers](https://www.kubecost.com/pricing).

The two fields in the screenshot above related to Spot CPU and RAM pricing are only relevant if your Kubernetes cluster is hosted in a public cloud and using [spot instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html) as cluster nodes.


### Viewing Cost Allocation Dashboard

Once you access the user interface, the Cost Allocation dashboard is visibly present on the left navigation menu. This view allows you to select various grouping dimensions for allocating cluster costs. The screenshot in the example below shows costs allocated to each of the “Product” label values used in the cluster. Further down in this section, we explain how you can map Product (or Owner, Department) to your own custom labels.

![Cost Allocation dashboard](/assets/images/2021-07-07-onprem/image4.png "Cost Allocation dashboard")

As you can see, the cluster costs are grouped by namespace as rows and broken by computing resources (CPU, RAM, GPU, storage, network) as columns. The column titled PV represents the storage cost of Persistent Volumes (PV), and the network costs include the data transfer costs to and from the internet or Virtual Private Network connections. The Shared costs may represent shared infrastructure services (such as a database cluster) used by all namespaces or a fixed overhead dollar value. The External cost may be made up of charges directly incurred by each namespace outside of the Kubernetes cluster, such as a database or blob storage dedicated to one namespace.

Note that the screenshot presented above shows a selector for grouping costs by various dimensions. Kubecost provides the ability to map custom labels to popular grouping dimensions (Owner, Team, Department, Product, Environment) based on labels. This mapping feature avoids polluting the grouping selector with too many label options that may be cryptic or irrelevant to end-users. This mapping (as presented below) is available on the General Settings page, accessible from the lower-left corner of the main page’s user interface. 

![Create custom labels](/assets/images/2021-07-07-onprem/image1.png "Create custom labels")

### Viewing Cluster Health

Another easy-to-find report on the left navigation menu is titled Health. Preset Kubernetes administration best practices power this view to help you identify resource constraints, bottlenecks, and security risks. This view helps you detect performance bottlenecks such as under-provisioned memory or OOM (out of memory) events caused by taking away too much computing resource in an effort to save money. This functionality is crucial in helping you strike the correct balance between performance and cost savings.

![View Cluster Health](/assets/images/2021-07-07-onprem/image7.png "View Cluster Health")

### Configuring Cost Alerts 

As mentioned earlier in this article, Kubecost’s cost alerts help:



* Schedule a regular cost report (recurring update)
* Alert in case a daily spend exceeds a threshold (budget)
* Alert when a daily spend is above a historical moving average (spend change)
* Alert when cluster resources are under or over-provisioned (efficiency)

The screenshot below shows a form in the Kubecost user interface accessible on the left-hand main navigation menu under a section titled “notifications”. You may enter a threshold for daily spending in this form and receive an alert notification if your actual usage exceeds your daily budget threshold. This feature helps stop an overrun right when it starts and before it goes unnoticed for weeks.

![Configure cost alerts](/assets/images/2021-07-07-onprem/image9.png "Configure cost alerts")
 
The notification of a spending overrun alert may be in the form of an email, a Slack message (as shown below), or a Webhook (for easy integration into third-party tools such as PagerDuty, for example).

### Via Email 

![Kubecost Email Notification](/assets/images/2021-07-07-onprem/image3.png "Kubecost Email Notification")
Kubecost Email Notification


### Via Slack

![Kubecost Slack Notification](/assets/images/2021-07-07-onprem/image8.png "Kubecost Slack Notification")
Kubecost Slack Notification 


## Conclusion

A public cloud provider meters the costs of a hosted Kubernetes cluster so you can know your exact hourly spending and stop it when you would like. On the other hand, the sunk cost of a node supporting an on-premise cluster is more complicated to calculate but possible to estimate using the method proposed in this article. Once you estimate the value of an on-premise Kubernetes node, Kubecost can help you monitor your cluster’s costs with cost allocation reports, cost alerts, and health checks. Kubecost’s functionality allows you to safely strike a perfect balance between your cluster’s performance and cost.
