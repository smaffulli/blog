---
title: "Kubecost raises $5.5 million to help teams monitor and reduce their Kubernetes spend"
date: 2021-03-23T09:04:30-08:00
canonical_url: "http://blog.kubecost.com/blog/announcing-kubecost-first-round/"
classes: wide
categories:
  - blog
tags:
  - Kubecost
  - cost monitoring
  - Kubernetes cost management
---


In late 2019, we launched the Kubecost [open source project](https://github.com/kubecost/cost-model) to help give engineers visibility into spend and resource efficiency in Kubernetes environments. Today, we’re incredibly proud to announce that we have built an amazing team, formed a company around [Kubecost](http://kubecost.com/), and raised $5.5 million in funding led by First Round Capital.

## Massive Kubernetes growth, but a lack of visibility

Over the last five years, we’ve witnessed an enormous number of companies across all industries migrate to Kubernetes and the cloud native toolchain. Adoption of the platform has grown by 300% since 2016, the year of the first CNCF adoption survey. More than 70% of the Fortune 100 now use Kubernetes [1]. Millions of developers leverage the platform, and every day new companies are running K8s in production. But this new ecosystem brings new layers of complexity, especially as teams begin to think about observability and optimization. Many teams struggle to understand where their cloud spend is going, i.e. which team, project, or application is spending how much and on which infrastructure services. Engineering and infrastructure teams all too often end up with a big bill at the end of the month—and occasionally a bit of heat from their colleagues in finance, too.

Long before we launched Kubecost, we experienced these cost-related complexities while working on infrastructure monitoring at Google. Working on Borg and Google Cloud, we observed both technical and behavioral challenges as engineering teams shared infrastructure in dynamic, containerized environments. Teams were unintentionally overspending and forgetting to clean up resources. Many lacked visibility into their costs and usage, which prevented them from having true ownership over their resources—and the problem only got worse as engineers began shipping applications on Kubernetes infrastructure. With systems to track overruns and nudge developers and tools to show the savings of proper capacity management, Kubecost aims to provide developers building on shared platforms with the same level of insight as in-house tooling built at companies like Google. 

Solving the problem of runaway Kubernetes spend—and empowering developer teams to manage these costs—starts with giving DevOps and engineering teams visibility into the black box that is Kubernetes spend. We saw early on that teams running Kubernetes needed a solution purpose-built for them, and the idea for Kubecost was born. 

![allocation-mar-23-2021](https://user-images.githubusercontent.com/298359/112073726-e2d1f100-8b31-11eb-9bd0-7fc3ef5b445d.gif)

## Giving over a thousand companies insight into $1B in spend

Today, we have over a thousand companies—including nearly a hundred enterprise teams such as Adobe, HP, and Siemens—using Kubecost to shine a light into this black box and manage their Kubernetes-related spend. Collectively, we’re now providing our users with visibility into more than $1 billion in spend, giving them savings tips and optimization insights, and most importantly, helping them solve ongoing governance concerns. The majority of these teams are able to reduce costs by 30-50% with Kubecost, some by even more than 50%. We’re honored to be working with an amazing and diverse array of customers, including Broadcom, Electronic Arts, Equifax, Capital One, Namely, Scale.ai, and many more.    

It’s deeply important to us that our solutions are accessible to teams of all sizes, budgets, and levels of familiarity with Kubernetes—hence our focus on open source and our commitment to providing an always-available free installation. We are provider-agnostic, with official support for AWS, Azure, GCP, and those running Kubernetes in their on-premises environments. We even support air-gapped environments. Because our software is deployed in an end user’s environment directly, we’re a natural fit for security-minded and heavily regulated industries. You can install Kubecost in less than five minutes without ever sharing any data with us.

We’re honored to be working with the team at [First Round Capital](https://firstround.com/) and are already learning a lot from them as well as our peers in their family of portfolio companies. We have many other great investors joining in this round, including [Afore Capital](https://afore.vc/), [Andrew Lee](https://www.linkedin.com/in/startupandrew/), [David Lieb](https://www.linkedin.com/in/davidlieb/), [Cack Wilhelm](https://www.linkedin.com/in/cackferrell/), [Chris Aniszczyk](https://www.linkedin.com/in/caniszczyk/), [Michael Stoppelman](https://www.linkedin.com/in/michaelstoppelman/), [Tim Chen](https://www.linkedin.com/in/timchen/), and the co-founders of [Replicated](https://www.linkedin.com/company/replicated/). 

This funding will help us serve even more teams and provide them with a delightful experience. This is a big step toward our long term goal of making it easier to operate cloud native infrastructure at scale. We’re starting with monitoring and optimizing Kubernetes spend, but there is so much more work we can do to help teams manage cost, performance, reliability, and other operability challenges with modern service-based applications. To do this, we plan to triple our engineering team and build a world-class support and success organization. If you’re interested in joining the cause, check out our [open roles](https://angel.co/company/kubecost) or learn more at [Kubecost.com](https://kubecost.com/)!

<br/><br/>
Sources:  
[1] https://<span></span>forbes.com/sites/forbestechcouncil/2020/04/09/three-trends-that-will-affect-cloud-native-in-2020/?sh=19484a542327  
[2] https://<span></span>cncf.io/blog/2020/11/17/cloud-native-survey-2020-containers-in-production-jump-300-from-our-first-survey/


