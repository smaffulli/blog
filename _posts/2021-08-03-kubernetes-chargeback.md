---
title: "A Guide to Kubernetes Chargeback"
description: "Learn how to successfully implement a showback or chargeback strategy for Kubernetes."
date: 2021-08-03T8:00:00-04:00
canonical_url: "https://blog.kubecost.com/blog/kubernetes-chargeback"
classes: wide
categories:
  - blog
tags:
  - Chargebacks
  - Monitoring
---

Implementing an effective Kubernetes chargeback model is particularly challenging—compared to other IT infrastructure types, since Kubernetes is an inherently multi-tenant platform that hosts short-lived containers and possesses no built-in mechanism for cost management. Kubecost offers a way for organizations to meet this need, with a free and open-source tool designed to help you monitor and reduce Kubernetes spend. In this article, we’ll explain the benefits of the chargeback model, introduce some best practices for implementing a chargeback report for Kubernetes, and help you get started with Kubecost within minutes.


## Why Chargeback?

In the study of economics, the [tragedy of the commons](https://en.wikipedia.org/wiki/Tragedy_of_the_commons) refers to a situation where societal resources are unregulated and freely accessible to individuals, leading to overuse and depletion to the ultimate detriment of society at large. The same phenomenon can happen while using shared computing resources. It’s a common perception of application owners that cloud infrastructure is unlimited and therefore practically free, but this belief often leads to over-provisioning and underutilization. Enforcing efficient consumption requires a shared sense of accountability across stakeholders. When it comes to IT infrastructure, the relevant stakeholders are the finance department, DevOps (or infrastructure engineers), software developers, and the application owners (usually developers and/or product managers). 


## What Is Chargeback?

In a chargeback model, IT (or the relevant infrastructure team) becomes a commercial supplier to a cost center representing a business unit or an application owner (effectively, this cost center becomes IT’s customer). Just as in the business world, the cost center/customer may decide whether to purchase its infrastructure needs from the IT department or a third-party supplier offering better products and services at a lower price. Senior management measures each business unit’s profit and loss in such a model, requiring the cost center to set budgets and forecast spending—and making decisions (such as whether to keep offering an application or continue operating the business unit!) off of the numbers. 

However, the chargeback model is neither the starting point nor the end goal. As we explain later in this article, a chargeback program is one stage in the journey of creating and enforcing a culture of efficient infrastructure spending across a company. 


## The 4 Pillars of a Chargeback Program

A great chargeback program is one trusted by all stakeholders. The four pillars described below help build this trust.


![The four pillars of trust for chargebacks](/assets/images/2021-08-03-chargebacks/image12.png "The four pillars of trust for chargebacks")



### 1. Accurate

The costs must reflect the actual dollar value, down to the penny, of common spend categories like: installation, support, maintenance, and any taxes associated with the IT infrastructure. The organizers of a chargeback program must avoid estimation when it comes to reporting.


### 2. Timely

Delays make it harder for users to remember the events that led to the changes in their infrastructure usage. Correcting a misconfiguration that led to increased provisioning is painless when you can identify and act on it within a few hours. The same misconfiguration turns into a painful budgeting discussion when presented as an end-of-month billing surprise (sounds familiar?).


### 3. Fair

Allocating common charges can be subjective. For example, premium support offered by a Kubernetes cloud service provider may cost a few additional percentage points —this service might bevalued or even required by some of the cluster tenants, but go completely unused by others. Should the additional charge be evenly distributed across all Kubernetes cluster tenants? Or charged only to those tenants who require premium support? Or should it be divided proportionately to resource usage? These critical allocation decisions hinge on an organization’s own notion of fairness and, in turn, either inspire confidence in the chargeback program, or cause stakeholders to lose faith in the model. 


### 4. Complete

The calculation of Kubernetes costs is complete only when the chargeback report accounts for all shared resource costs across cluster tenants, including charges incurred externally to the Kubernetes cluster. For example, cloud-based volume storage, blob storage, backup, and data transfer (or networking) costs are considerable expenses that need to be included in a complete report. 


## Implementing Chargeback

As shown in the diagram below, efficient spending begins with allocating costs to each member of a multi-tenant platform based on their metered usage of the shared computing resources. 


![The journey to chargebacks](/assets/images/2021-08-03-chargebacks/image1.png "The journey to chargebacks")

A less involved stage before a chargeback program is the institution of “showback” reports. Showback reporting doesn’t require financial systems to transfer funds and generate financial statements by cost center as expected in a chargeback program. However, it still provides visibility to the cost center managers to appreciate their infrastructure usage trends over time. 

The ultimate goal is to empower development teams to _want_ to enforce responsible spending and avoid waste in the company, by bringing visibility and responsibility to the users who request infrastructure resources. Let’s cover each stage in some more detail.


### Cost Allocation

Since it’s common to use [Kubernetes namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) to separate administrative purview over a shared cluster among multiple tenants, we describe the categories of cost as in-cluster, shared, and external (from the vantage point of a Kubernetes namespace) in the table below. 

<table>
  <thead>
  <tr>
   <th>Cost Type
   </th>
   <th>Kubernetes Cost 
   </th>
   <th>Examples
   </th>
  </tr>
  </thead>
  <tr>
   <td>In-Cluster
   </td>
   <td>The cost of the cluster nodes
   </td>
   <td>CPU, memory, storage, network, and GPU 
   </td>
  </tr>
  <tr>
   <td>External
   </td>
   <td>The cost of services dedicated to an individual Kubernetes namespace
   </td>
   <td>Blob storage such as AWS S3, database service
   </td>
  </tr>
  <tr>
   <td>Shared
   </td>
   <td>The cost of services shared by all Kubernetes namespaces
   </td>
   <td>kube-system namespace, monitoring tool namespace
   </td>
  </tr>
</table>


Let’s assume that we share a Kubernetes cluster between two cost centers and have assigned a namespace to each cost center. The in-cluster costs include the cost of the nodes. An S3 bucket dedicated to namespace A is an external cost, while the cost of a database serving the entire cluster is considered a shared cost.


![An example Kubernetes cluster](/assets/images/2021-08-03-chargebacks/image2.png "An example Kubernetes cluster")


The complexity of cost allocation stems from the fact that it requires measuring resource usage, such as network bandwidth, down to every second. This challenge led to creating the Kubecost open-source project, as we explain below in this article. 


### Showback Reporting 

In the example scenario presented earlier in this article, a report separating the costs of namespace A and B would be a showback report. The term is a play on the word “chargeback” and the idea of “showing” charges by cost center. Showbacks are simply reports that allow the owner of each cost center to know the actual expenses incurred by their applications hosted on the Kubernetes cluster over time.

The challenge with a showback report lies in the four pillars presented earlier in this article: accuracy, fairness, timeliness, and completeness. Once again, the need for showback reporting conceived the Kubecost open-source project. It’s essential to recognize that a showback report is only relevant if the Kubernetes cluster already leverages labels and namespaces to organize workloads associated with cost centers. This level of organization requires representatives from each cost center to engage with Kubernetes administrators and care about monitoring their spending levels.


### Chargeback Program

A chargeback program is the next level of maturation in the process of establishing accountability in cost management. However, it requires the active participation of the finance department to implement accounting systems capable of generating profit and loss (a.k.a., P&L) financial statements by each cost center. 

Each department’s profit and loss statement requires separating sales and expenses by cost center to calculate the profit or loss. A relative ratio of employee count is a fair method to allocate the shared cost of office space rent, for example. A similarly equitable approach to allocating infrastructure costs is based on resource usage by application belonging to a cost center, as mentioned earlier in this article. 


### Enforcement

The enforcement stage represents the ultimate level of financial control that an organization can implement. In this stage, the profit and loss statements enable a budgeting process designed to share responsibility for cost management with all departmental line managers. 

Business managers forecast quarterly sales based on their market expectations and economic conditions. With the sales forecast finalized, financial managers can calculate the total expenses that each business unit can afford while preserving profit or loss margins ultimately expected by shareholders. Historical spending helps negotiate a budget for each line item at the beginning of each fiscal quarter, thereby enforcing spending on a detailed level across each business unit, including Kubernetes infrastructure consumption.


## Installing Kubecost 

Kubecost is a free and open-source tool created to help organizations manage the cost of multi-tenant Kubernetes clusters hosted in a data center, public cloud, or hybrid cloud. Within minutes after installation, it allocates cost by namespace or labels according to resource usage. It also provides industry best practices policies to detect performance bottlenecks or alert on an unexpected increase in spending.

Kubecost can be installed in minutes and is available for download as a Helm chart. You can also find find further instructions at [https://www.kubecost.com/install.html](https://www.kubecost.com/install.html). 

#### helm 3 install
```
helm repo add kubecost https://kubecost.github.io/cost-analyzer/
helm upgrade -i --create-namespace kubecost kubecost/cost-analyzer --namespace kubecost --set kubecostToken="aGVsbUBrdWJlY29zdC5jb20=xm343yadf98"
```

### Personalizing Your Settings

Once Kubecost is installed, you can navigate to the settings page (accessible in the lower left of the user interface) and choose a few options, such as a method for allocating idle resources to each cluster tenant.


![Kubecost custom settings](/assets/images/2021-08-03-chargebacks/image4.png "Kubecost custom settings")



### Viewing Cost Allocation Dashboard

The Cost Allocation dashboard is visible on the left navigation menu. This dashboard allows the grouping of costs based on their utilization across multiple dimensions, such as namespace, pod, or service.  In the screenshot presented below, Product, Owner, and Department shown on the menu are variables that can be mapped to your own custom labels. We explain this later in the article.


![Kubecost cost allocation dashboard](/assets/images/2021-08-03-chargebacks/image3.png "Kubecost cost allocation dashboard")


In the screenshot above, the costs of computing resources are shown as columns (namely CPU, RAM, GPU, storage, and network). The acronym PV used as a header for one of the columns stands for Persistent Volume. The network costs include the data transferred over VPN or across public cloud regions. The rows vary depending on the meta-data dimension chosen on the pull-down menu for grouping the cost values. 

With a couple of clicks on the settings page, you can allocate the costs of a shared namespace (such as the namespace where Kubecost is installed) to other namespaces. It’s just as easy to enter a fixed dollar value that may represent labor costs and divide it across namespaces or other grouping dimensions.

You may have noticed in the screenshot that one of the columns is labeled as “External.” This column contains the costs associated with resources that are outside of the cluster, such as a database server or AWS S3 bucket used by a particular namespace.

As mentioned earlier, the General Settings page (see screenshot below) offers the option to map the grouping dimensions of Owner, Team, Department, Product, Environment (common to most Kubernetes implementations) to your own custom labels. This helps keep the menu consistent and uncluttered while allowing you the flexibility to leverage the naming convention that you have used in the past.


![Kubecost owner annotations](/assets/images/2021-08-03-chargebacks/image5.png "Kubecost owner annotations")



### Saving Cost Allocation Reports 

Kubecost’s Saved Reports offer a convenient approach to customize and save Cost Allocation (and Asset) reports for periodic access. This functionality, simply titled Reports, is accessible in the lower section of the main navigation menu. 

Once you have settled on a cost allocation strategy using either namespaces or labels, you can use the Saved Reports to create daily, weekly, or monthly allocation reports commonly requested for engineering and finance teams. The periodic reports can be scheduled and received by the appropriate group via either email or Slack. The screenshot below shows the reporting portal with a summarized view of multiple saved reports.

![Kubecost saved cost allocation reports](/assets/images/2021-08-03-chargebacks/image7.png "Kubecost saved cost allocation reports")


The reports contain detailed cost allocation data including in-cluster, external, and shared expenses described earlier. The reports are available for download in a CSV format for further analysis in a spreadsheet. They are also accessible via API for integration with financial systems implemented as part of a chargeback program. 


![Kubecost detailed cost allocation report](/assets/images/2021-08-03-chargebacks/image10.png "Kubecost detailed cost allocation report")


The saved reports are rich with filtering, grouping, and sorting features, offering flexibility for reporting in various dimensions of meta-data. The Saved Reports also support more specialized and sophisticated cost allocation decisions. For example, you can choose whether to share the idle costs by node or by cluster; or decide whether to account for the cluster’s shared resources by evenly distributing them or weighing the distribution by cost (where the bigger namespaces would get a larger portion of the shared costs). The screenshot below shows some of the additional capabilities available on the configuration menu. 


![Kubecost custom reports](/assets/images/2021-08-03-chargebacks/image11.png "Kubecost custom reports")


YAML files can configure the reports and bypass the user interface for those who prefer to create reports programmatically as part of a continuous integration and continuous delivery (CI/CD) process. This [documentation page](http://docs.kubecost.com/saved-reports) presents the instructions necessary for configuring the reports via YAML and lists all of the available report configuration parameters. 


### Configuring Budgets and Cost Alerts 

Cost alerting is another popular and unique Kubecost feature.  Cost alerts help you with the following:



* Schedule a cost report (periodic update)
* Alert when a daily spend exceeds a threshold (the threshold is a daily budget)
* Alert when a daily spend is above a historical moving average (spend jump)
* Alert when clusters are either under or over-provisioned (efficiency)

The screenshot below is accessible on the left-hand navigation menu as a selection titled “notifications.” The notification configuration page is shown below, where users can choose a daily budget (spending threshold) for each namespace and select different recipients for each emailed alert. 

The maturity framework presented earlier in this document showed a final and fourth stage titled “Enforcement.” Kubecost supports this stage by allowing each cost center to manage its daily budget and receive alerts when the spending exceeds the preset budget. With Kubecost, cost overruns can be stopped on the first day before they evolve into an end-of-month management surprise.


![Kubecost alerts](/assets/images/2021-08-03-chargebacks/image8.png "Kubecost alerts")
 

A cost alert may be received as an email, a Slack message, or a Webhook (for easy integration into third-party tools). The screenshots below are examples of such alerts.


### Via Email 


![Kubecost email notification](/assets/images/2021-08-03-chargebacks/image6.png "Kubecost email notification")


Kubecost Email Notification


### Via Slack


![Kubecost Slack Notification](/assets/images/2021-08-03-chargebacks/image9.png "Kubecost Slack Notification")


Kubecost Slack Notification 


### Conclusion 

Enforcing organizational efficiency in Kubernetes usage across an enterprise requires both processes and tools. The essential functionality involves allocating cluster costs to each cost center which has been the focus of Kubecost since the open-source project’s inception. The process of enforcing begins by delivering a periodic showback report to the key stakeholders in each cost center. The ultimate form of enforcement is chargeback which holds each cost center responsible for its profitability by spending within preset daily budgets established for every expense line item. Installing Kubecost for free takes five minutes and starts [here](https://www.kubecost.com/install.html). 

