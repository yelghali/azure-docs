---
title: Concepts - Sustainable software engineering in Azure Kubernetes Services (AKS)
description: Learn about sustainable software engineering in Azure Kubernetes Service (AKS).
services: container-service
ms.topic: conceptual
ms.date: 03/29/2021
---

# Sustainable software engineering practices in Azure Kubernetes Service (AKS)

The sustainable software engineering principles are a set of competencies to help you define, build, and run sustainable applications. The overall goal is to reduce the carbon footprint in every aspect of your application. [The Principles of Sustainable Software Engineering][principles-sse] has an overview of the principles of sustainable software engineering.

Sustainable software engineering is a shift in priorities and focus. In many cases, the way most software is designed and run highlights fast performance and low latency. Meanwhile, sustainable software engineering focuses on reducing as much carbon emission as possible. Consider:

* Applying sustainable software engineering principles can give you faster performance or lower latency, such as by lowering total network travel. 
* Reducing carbon emissions may cause slower performance or increased latency, such as delaying low-priority workloads. 

The [sustainability guidance in the Microsoft Azure Well-Architected Framework (WAF)](/azure/architecture/framework/sustainability/) aims to address the challenges of building sustainable workloads on Azure.

This article provides practical guidance for applying WAF sustainability guidance for AKS.

## Get started with WAF sustainability guidance

* [What is a sustainable workload](/azure/architecture/framework/sustainability/sustainability-get-started)
* [Design Methodology for building sustainable workloads](/azure/architecture/framework/sustainability/sustainability-design-methodology)
* [Design principles of a sustainable workload](/azure/architecture/framework/sustainability/sustainability-design-principles)


## Cloud efficiency
Making workloads more [sustainable and cloud efficient](/azure/architecture/framework/sustainability/sustainability-get-started#cloud-efficiency-overview), requires combining efforts around cost optimization, reducing carbon emissions, and optimizing energy consumption. Optimizing the application's cost is the initial step in making workloads more sustainable.


## Key sustainability Design Areas

Sustainable guidance in the Well Architected Framework series is composed of architectural considerations and recommendations oriented around these key design areas.

Decisions made in one design area can impact or influence decisions across the entire design. The focus is ultimately on building a sustainable solution to minimize the footprint and impact on the environment.

|Design area|Description|
|---|---|
|[Application design](/azure/architecture/framework/sustainability/sustainability-application-design.md)|Cloud application patterns that allow for designing sustainable workloads.|
|[Application platform](/azure/architecture/framework/sustainability/sustainability-application-platform.md)|Choices around hosting environment, dependencies, frameworks, and libraries.|
|[Testing](/azure/architecture/framework/sustainability/sustainability-testing.md)|Strategies for CI/CD pipelines and automation, and how to deliver more sustainable software testing.|
|[Operational procedures](/azure/architecture/framework/sustainability/sustainability-operational-procedures.md)|Processes related to sustainable operations.|
|[Storage](sustainability-storage.md)|Design choices for making the data storage options more sustainable.|
|[Network and connectivity](/azure/architecture/framework/sustainability/sustainability-networking.md)|Networking considerations that can help reduce traffic and amount of data transmitted to and from the application.|
|[Security](/azure/architecture/framework/sustainability/sustainability-security.md)|Relevant recommendations to design more efficient security solutions on Azure.|

We recommend that readers familiarize themselves with these design areas, reviewing provided considerations and recommendations to better understand the consequences of encompassed decisions.

## Sustainability Design considerations for AKS workloads
 -  Sustainability considerations for your AKS workloads (or applications), **should cover the All Key Sustainability Design Areas**
 -  The sustainability considerations for AKS clusters are aligned with Application Platform Area
 -  In practice, you should consider the hollistic lifecycle of your application, as Business requiements shape Workload design, which will inform cluster design
  
|Design area|Description|
|---|---|
|[Application design](concepts-sustainability-application-design.md)|Modernize workloads to allow independent optimization of their logical components|
|[Application platform](concepts-sustainability-application-platform.md)|**AKS cluster is the Platform** ; Design cluster for energy and hardware efficiency|
|[Testing](concepts-sustainability-testing.md)|Optimize Testing procedures for Cluster & workload development lifecycle|
|_Operational procedures_|Implement sustainability operations (not a technical consideration). Refer to the [WAF Operational procedures](/azure/architecture/framework/sustainability/sustainability-operational-procedures.md)|
|[Storage](concepts-sustainability-storage.md)| Consider _Stateless Vs Stateful Application_ Design ; Plan for storage classes & Backup retention policies.|
|[Network and connectivity](concepts-sustainability-networking.md)|Optimize network travel for workloads and clusters|
|[Security](concepts-sustainability-security.md)| Implement Security and Optimize log collection for Monitoring & SIEM.|



## For Product Teams: Sustainability Checklist for AKS workloads

The following checklist provides recommendations for designing sustainable workloads, hosted on AKS. 

As your workload End2End architecture would typically include several Azure services (or 3rd party integration) ; your workload design considerations should refer to [Sustainability Design considerations for AKS workloads](#sustainability-design-considerations-for-aks-workloads), for a more comprehensive approach.



Design:




reporting : tag resources

Optimize storage:





Optimize network:



Testing






**Optimize code for efficient resource usage**

**Containerize workloads where applicable**
_Consider options for containerizing workloads to reduce unnecessary resource allocation and to utilize the deployed resources better._

 - Use [Draft](/azure/aks/draft) to simplify containzerizing an application by generating its Dockerfiles and Kubernetes manifests.

**Evaluate moving monoliths to a microservice architecture to allow independent scaling of their logical components**
_Monolithic applications usually scale as a unit, leaving little room to scale only the individual components that may need it._
- Build Microservices Applications using [Dapr](https://dapr.io/) 
- Build [CNCF Projects on AKS](/azure/architecture/example-scenario/apps/build-cncf-incubated-graduated-projects-aks)

Design for Event Driven scaling, which allows to scale based on business metrics
- Build serverless Applications using [Keda](https://keda.sh/) 
 
Leverage cloud native design patterns

Consider using circuit breaker patterns

Optimize for async access patterns


Deploy to the Right Region:
Select Azure regions based on where the customer resides
Deploy to low-carbon regions

** Aim for [Stateless Design]**
- (if possible), Aim for [Stateless Design](/azure/aks/operator-best-practices-multi-region#remove-service-state-from-inside-containers)

choose-the-appropriate-storage-type
- Choose [the appropriate storage type](/azure/aks/operator-best-practices-storage#choose-the-appropriate-storage-type).


Optimize Storage Utilization
- Use [Storage Classes to define application needs](/azure/aks/operator-best-practices-storage#create-and-use-storage-classes-to-define-application-needs)
- [Dynamically provision volumes](/azure/aks/operator-best-practices-storage#dynamically-provision-volumes).

Set Storage Retention Policies
- Define retention policies for storage, backups, logs

Revise backup and retention policies
- Define retention policies for storage, backups, logs
- Backup & restore [your persistent volumes](/azure/aks/operator-best-practices-storage#secure-and-back-up-your-data) 



**Optimize networking**

Evaluate whether to use TLS termination
- Consider if you can terminate TLS at your border gateway and continue with non-TLS to your workload load balancer and onwards to your workload.
- Review the information on [TLS termination](/azure/application-gateway/ssl-overview#tls-termination) to better understand the performance and utilization impact it offers.



Reduce Transmitted Data
- Consider if you (really) need a [service mesh](/azure/aks/servicemesh-about)
- Consider [when to use Dapr with Or without a service mesh](https://docs.dapr.io/concepts/service-mesh/#when-to-use-dapr-or-a-service-mesh-or-both)

Optimize the collection of logs


 **Assess for Resilience and Performance**
- Use [load testing](/azure/load-testing/tutorial-identify-performance-regression-with-cicd) and [chaos engineering](/azure/architecture/framework/resiliency/chaos-engineering) to assess how the workload handles platform outages and traffic spikes or dips. This helps increase service resilience and the ability to react to failures, allowing for a more optimized fault handling.

**Optimize resource usage**
- Use [Keda Cron scaler](https://keda.sh/docs/2.7/scalers/cron/), to turn off applications (scale pods to zero), outside regular business hours.
 - Define workloads [resource requests and limits](/azure/aks/developer-best-practices-resource-management#define-pod-resource-requests-and-limits)
- [Use Tags](/azure/aks/use-tags) to enable recording of emissions impact.
- Use [Best Practices for Monitoring Cloud Applications](/azure/architecture/framework/devops/monitor-collection-data-storage)
- Use [Best Practices for Monitoring Microservices Application on AKS](/azure/architecture/microservices/logging-monitoring)

**Consider Carbon Awareness in your workload design**
 - Deploy your workloads to Regions powered by renewable and low-carbon energy sources
 - Consider deploying to data centers close to the consumer

Reduce waste:
Turn off workloads outside of business hours

## For Platform Teams: Sustainability Checklist for AKS clusters

Choose A Region That Is Closest To Users

Filter or exclude log sources before transmission or ingestion into a SIEM
Use DDoS protection






The following checklist provides recommendations for designing energy and hardware efficient AKS clusters, that operate as a "Green Platform". 
   
**Enable Modernization of Applications**
-  Use Keda as an [AKS addon](/azure/aks/keda-about)
-  Use Darp as an [AKS addon](/azure/aks/dapr)
 - Use [Gitops on AKS to automate cluster & application lifecycle](/azure/architecture/example-scenario/gitops-aks/gitops-blueprint-aks), including testing & compliance.

**Scale cluster resources, based on demand**
Utilize auto-scaling and bursting capabilities
Match the scalability needs

- Use [Cluster Auto-scaler](azure/aks/cluster-autoscaler) to scale your cluster based on Demand.
- Leverage [Scaling **User node pools** to 0](/azure/aks/scale-cluster#scale-user-node-pools-to-0)
- Use [Virtual Nodes](/azure/aks/virtual-nodes) to rapidly burst to Serverless Nodes (that scale to zero when there is no demand)
- Review the [B-series burstable virtual machine sizes](https://azure.microsoft.com/en-in/blog/introducing-burstable-vm-support-in-aks/).

 **Use Energy Efficient Hardware**
 Evaluate Ampere Altra Arm-based processors for Virtual Machines

 - Evaluate if [nodes with Ampere Altra Arm–based processors](https://azure.microsoft.com/blog/azure-virtual-machines-with-ampere-altra-arm-based-processors-generally-available/) are a good option for your workloads

**Maximize Hardware utilization**
- Separate applications into different node pools allowing independent sizing & scalling.

Establish CPU and Memory thresholds in testing
Match Utilization Requirements of Virtual Machines (VMs)

- Align node SKU selection and managed disk size with applications requirements.
- [Size the nodes for storage need](/azure/aks/operator-best-practices-storage#size-the-nodes-for-storage-needs)
- [Resize node pools](/azure/aks/resize-node-pool) to maximize your applications density (and maximize your nodes usage).
- Use [Vertical Pod Auto-scaler](/azure/aks/vertical-pod-autoscaler) to automatically set resource requests and limits on containers per workload based on past usage

Use SPOT VMs where possible
- Use [SPOT Node pools](/azure/aks/spot-node-pool), to take advantage of unused capacity in Azure data centers while getting a significant discount on the VM.
- Use AKS [advanced scheduler features](azure/aks/operator-best-practices-advanced-scheduler) to optimize scheduling your applications (pods), to nodes




**Reduce Network Latency**
Reduce Transmitted Data
Maximize network utilization within the same cloud and region


- Consider using [Proximity Placement Groups](/azure/aks/reduce-latency-ppg) to reduce network latency

**Secure endpoints and eliminate unnecessary network traffic**
Use cloud native network security controls to eliminate unnecessary network traffic

- Use [Network security groups](/azure/virtual-network/network-security-groups-overview) 
- Use [Network Policies](/azure/aks/use-network-policies)

Use network security tools with auto-scaling capabilities

- Filter [Ingress traffic](/azure/application-gateway/ingress-controller-overview)
- Filter [egress traffic](/azure/aks/limit-egress-traffic)

Integrate Microsoft Defender for Endpoint

- [Enable Microsoft Defender for Containers](/azure/defender-for-cloud/defender-for-containers-introduction)
- [Identify vulnerable container images](/azure/defender-for-cloud/defender-for-containers-cicd)

**Reduce Waste**
Delete zombie workloads
Revise backup and retention policies
Delete Unused Storage Resources

- Use [ImageCleaner](/azure/aks/image-cleaner) to clean up stale images on your Azure Kubernetes Service cluster
- Use [cluster stop / start](/azure/aks/start-stop-cluster) and [node pool stop / start](/azure/aks/start-stop-nodepools), for shutting them down outside regular business hours.
- Enforce Kubernetes [Resource Quotas](/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)

**Optimize operations**
Archive log data to long-term storage

 - Configure [Automatic **Cluster Ugrade**](/azure/aks/auto-upgrade-cluster)
 - Configure [Automatic **Linux node updates**](/azure/aks/node-updates-kured)
- Use [Best Practices for Monitoring Cloud Applications](/azure/architecture/framework/devops/monitor-collection-data-storage)
- Use [Best Practices for Monitoring Microservices Application on AKS](/azure/architecture/microservices/logging-monitoring)

**Consider Carbon Awareness in your workload orchestration**
 - Consider optimizing workloads when knowing that the energy mix comes mostly from renewable energy sources
 - Plan your deployments to maximize compute utilization for running batch workloads during low-carbon intensity periods.

Process when the carbon intensity is low
Filter or exclude log sources before transmission or ingestion into a SIEM

## Next step

Review the sustainability Operational procedures.

> [!div class="nextstepaction"]
> [Operational procedures](/azure/architecture/framework/sustainability/sustainability-operational-procedures.md)
