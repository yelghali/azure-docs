---
title: Concepts - Sustainable software engineering in Azure Kubernetes Services (AKS)
description: Learn about sustainable software engineering in Azure Kubernetes Service (AKS).
services: container-service
ms.topic: conceptual
ms.date: 03/29/2021
---

# Sustainability considerations for Azure Kubernetes Service (AKS) Clusters

The following considerations are aligned with the [Application platform](/azure/architecture/framework/sustainability/sustainability-application-platform.md) Sustainability Design Area, and provide more details on designing and operating sustainable AKS Clusters.

## Platform and service updates

Keep platform and services up to date to leverage the latest performance improvements and energy optimizations.

### Review platform and service updates regularly 

Platform updates enable you to use the latest functionality and features to help increase efficiency. Running on outdated software can result in running a suboptimal workload with unnecessary performance issues. New software tends to be more efficient in general.

_sustainability design principles: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_


**Recommendations:**
 - Configure [Automatic **Cluster Ugrade**](/azure/aks/auto-upgrade-cluster) 
 - Configure [Automatic **Linux node updates**](/azure/aks/node-updates-kured)


**Potential tradeoffs**
   - Consider backward compatibility and hardware reusability. An upgrade may not be the most efficient solution if the software or the OS isn't supported.

## Regional differences

The Microsoft Azure data centers are geographically spread across the planet and powered using different energy sources. Making decisions around where to deploy your workloads can significantly impact the emissions your solutions produce.

Learn more about [sustainability from the data center to the cloud with Azure](https://www.microsoft.com/sustainability/azure).

### Deploy to low-carbon regions

Learn about what Azure regions have a lower carbon footprint than others to make better-informed decisions about where and how our workloads process data.

_sustainability design principles: [Carbon efficiency](sustainability-design-principles.md#carbon-efficiency)_



**Recommendations:**
 - Deploy your workloads to Regions powered by renewable and low-carbon energy sources 

**Potential tradeoffs**
   - For choosing the right region, Evaluate carbon efficiency, cost, latency, and compliance requirements.
   - Migrating data between data centers may not be carbon efficient.
   - Consider the cost for new regions, including low-carbon regions, which may be more expensive.
   - If the workloads are latency sensitive, moving to a lower carbon region may not be an option.


### Process when the carbon intensity is low

Carbon Intensity contained In Energy varies during the day. Therefore it's essential to build applications that maximize compute when Carbon Intensity is Low.

_sustainability design principles: [Carbon efficiency](sustainability-design-principles.md#carbon-efficiency), [Carbon awareness](sustainability-design-principles.md#carbon-awareness)_

**Recommendations:**

- Where you have the data available, consider optimizing workloads when knowing that the energy mix comes mostly from renewable energy sources.
- If your application(s) allow it, consider scheduling & scaling workloads dynamically when the energy conditions change. For example:
  - Scaling down Deployments _when enegry mix is high in carbon_, and scaling up when it is low
  - Pausing Jobs _when enegry mix is high in carbon_, and resuming execution when it is low  

**Potential tradeoffs**
   - Consider Time Scheduling constraints, for when workloads execution needs to be finished.
   - Target workloads need to have a resilient design and tolerate interruptions

### Choose data centers close to the customer

Deploying cloud workloads to data centers is easy. However, consider the distance from a data center to the customer. Network traversal increases if the data center is a greater distance from the consumer.

_sustainability design principles: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendations:**

- Consider deploying to data centers close to the consumer.

**Potential tradeoffs**
   - For choosing the right region, Evaluate carbon efficiency, cost, latency, and compliance requirements.

### Schedule batch workloads during low-carbon intensity periods

Proactively designing batch processing of workloads can help with scheduling intensive work during low-carbon periods.

_sustainability design principles: [Carbon awareness](sustainability-design-principles.md#carbon-awareness)_

**Recommendations:**

- Where you have the data available to you, plan your deployments to maximize compute utilization for running batch workloads during low-carbon intensity periods.

 - For example : Time scheduling recurrent workloads (CronJobs) at night may be more beneficial when renewable sources are at their peak

**Potential tradeoffs**
   - Time Scheduling constraints for workloads having several dependencies.

## Modernization

Consider these platform design decisions when choosing how to operate workloads. Leveraging managed services and highly optimized platforms in Azure helps build cloud-native applications that inherently contribute to a better sustainability posture.

### Containerize workloads where applicable

Consider options for containerizing workloads to reduce unnecessary resource allocation and to utilize the deployed resources better.

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

- Use [Draft](/azure/aks/draft) to simplify containzerizing an application by generating its Dockerfiles and Kubernetes manifests.


**Potential tradeoffs**

  - The benefit of containerization will only realize if the utilization is high. Additionally, provisioning an orchestrator such as Kubernetes for only a few containers would likely lead to higher emissions overall.

  - Containerizing an Monolith application, will help optimize its operations (at the platform level) ; but the application itself maybe not be energy efficient. Consider Modernizing the application as part of your migrations / containerizations efforts



### Evaluate moving to PaaS and serverless workloads

Managed services are highly optimized and operate on more efficient hardware than other options, contributing to a lower carbon impact.

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendations:**

- Build cloud native Apps, and leverage Cloud Platforms that optimize scaling, availability, and performance, ultimately optimizing the hardware efficiency.
- Build serverless Applications using [Keda](https://keda.sh/) ; Use it as an [AKS addon](/azure/aks/keda-about)
- Build Microservices Applications using [Dapr](https://dapr.io/) ; Use it as an [AKS addon](/azure/aks/dapr)
- Build [CNCF Projects on AKS](/azure/architecture/example-scenario/apps/build-cncf-incubated-graduated-projects-aks)
- Leverage [Virtual node pools](/aks/virtual-nodes) to optimize infrastructure usage, and ultimately hardware efficiency and costs.

**Potential tradeoffs**
 - [Virtual node pools limitations](/azure/aks/virtual-nodes#known-limitations)


### Use SPOT VMs where possible

Think about the unused capacity in Azure data centers. Utilizing the otherwise wasted capacity—at significantly reduced prices—the workload contributes to a more sustainable platform design.

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

- Use [SPOT Node pools](/azure/aks/spot-node-pool), to take advantage of unused capacity in Azure data centers while getting a significant discount on the VM.

**Potential tradeoffs**

- Consider the tradeoff: When Azure needs the capacity back, the VMs get evicted. Learn more about the SPOT VM [eviction policy](/azure/virtual-machines/spot-vms#eviction-policy).
- [Spot node pools limitations](/azure/aks/spot-node-pool#limitations) 

## Right sizing

Ensuring workloads use all the allocated resources helps deliver a more sustainable workload. Oversized services are a common cause of more carbon emissions.

### Turn off workloads outside of business hours

Ensuring workloads use all the allocated resources helps deliver a more sustainable workload. Oversized services are a common cause of more carbon emissions.

_sustainability design principles: [Energy efficiency](sustainability-design-principles.md#energy-efficiency), [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

- Use [cluster stop / start](/azure/aks/start-stop-cluster) and [node pool stop / start](/azure/aks/start-stop-nodepools), for shutting them down outside regular business hours.
- Use [Keda Cron scaler](https://keda.sh/docs/2.7/scalers/cron/), to turn off applications (scale pods to zero), outside regular business hours.

**Potential tradeoffs**

- Keda Cron scaler, helps scale applications based on time. It is best to design your applications to scale based on demand (traffic, queue length, etc.).



### Utilize auto-scaling and bursting capabilities

It's not uncommon with oversized compute workloads where much of the capacity is never utilized, ultimately leading to a waste of energy. 

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

https://learn.microsoft.com/en-us/azure/aks/scale-cluster?tabs=azure-cli#scale-user-node-pools-to-0
- Use [Keda](https://keda.sh/) to Auto-scale your applications based on demand.
- Use [Cluster Auto-scaler](azure/aks/cluster-autoscaler) to scale your cluster based on Demand.
- Leverage [Scaling **User node pools** to 0](/azure/aks/scale-cluster#scale-user-node-pools-to-0)
- Use [Virtual Nodes](/azure/aks/virtual-nodes) to rapidly burst to Serverless Nodes (that scale to zero when there is no demand)
- Review the [B-series burstable virtual machine sizes](https://azure.microsoft.com/en-in/blog/introducing-burstable-vm-support-in-aks/).

**Potential tradeoffs**

- Consider that it may require tuning to prevent unnecessary scaling during short bursts of high demand, as opposed to a static increase in demand.
- Consider the application architecture as part of scaling considerations. For example, [logical components should scale independently](/azure/architecture/framework/sustainability/sustainability-application-design.md#evaluate-moving-monoliths-to-a-microservice-architecture) to match the demand of that component, as opposed to scaling the entire application if only a portion of the components needs scaling.


### Match the scalability needs

Consider the platform and whether it meets the scalability needs of the solution. For example, having provisioned resources with a dedicated allocation may lead to unused or underutilized compute resources.

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

- Separate applications into different node pools allowing independent scalling.
- Align node SKU selection and managed disk size with applications requirements.
- [Size the nodes for storage need](/azure/aks/operator-best-practices-storage#size-the-nodes-for-storage-needs)
- [Resize node pools](/azure/aks/resize-node-pool) to maximize your applications density (and maximize your nodes usage).
- Use [Vertical Pod Auto-scaler](/azure/aks/vertical-pod-autoscaler) to automatically set resource requests and limits on containers per workload based on past usage
- Use AKS [advanced scheduler features](azure/aks/operator-best-practices-advanced-scheduler) to optimize scheduling your applications (pods), to nodes
- Perform [ongoing load testing activities](/azure/load-testing/overview-what-is-azure-load-testing) that exercise both the pod and cluster autoscaler.
- Enforce Kubernetes [Resource Quotas](/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)
- [Monitor & Optimize](/azure/azure-monitor/containers/container-insights-overview)

**Potential tradeoffs**
- Some services require a higher tier to access certain features and capabilities regardless of resource utilization.
- Consider and prefer services that allow dynamic tier scaling where possible.

### Evaluate Ampere Altra Arm-based processors for Virtual Machines

The Arm-based VMs represent a cost-effective and power-efficient option that doesn't compromise on the required performance.

_sustainability design principles: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendations:**

- Evaluate if the Ampere Altra Arm-based VMs is a good option for your workloads.
- Read more about [Azure Virtual Machines with Ampere Altra Arm–based processors](https://azure.microsoft.com/blog/azure-virtual-machines-with-ampere-altra-arm-based-processors-generally-available/) on Azure.
- Monitor your AKS clusters having Arm nodes [with container insights](https://azure.microsoft.com/en-us/updates/public-preview-monitoring-for-ampere-altra-arm-based-vms-and-aks-clusters/)

### Delete zombie workloads

_sustainability design principles: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Use [ImageCleaner](/azure/aks/image-cleaner) to clean up stale images on your Azure Kubernetes Service cluster

# Networking considerations for sustainable workloads on Azure

Most workloads in the cloud rely heavily on networking to operate. Whether internal networking or public-facing workloads, the components and services used in provisioned solutions must consider the impact of carbon emissions. Consider that network equipment consumes electricity, including traffic between the data centers and end consumers. Learn about considerations and recommendations to enhance and optimize network efficiency to reduce unnecessary carbon emissions.

Internet traversal between data centers and end consumers is a significant [Scope 3 emission](sustainability-design-methodology.md#briefly-about-emission-scopes). Therefore, recommendations in this section are aligned with the Principles of Green Software [Networking](https://principles.green/principles/networking/) area to improve networking efficiency.

> [!IMPORTANT]
> This article is part of the [Azure Well-Architected sustainable workload](index.yml) series. If you aren't familiar with this series, we recommend you start with [what is a sustainable workload?](sustainability-get-started.md#what-is-a-sustainable-workload)

## Network efficiency

Reduce unnecessary network traffic and lower bandwidth requirements where possible, allowing for a more optimized network efficiency with less carbon emission.


### Select Azure regions based on where the customer resides

The location of an application's consumers can be disparate, and it can be challenging to serve requests with good performance and energy efficiency if the distance is too great.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Deploy or [move Azure resources across regions](/azure/architecture/solution-ideas/articles/move-azure-resources-across-regions) to better serve the applications from where most consumers reside.


### Maximize network utilization within the same cloud and region

Operating solutions in multiple regions have a networking impact. Network traversals between components in Azure are optimized to stay within the Azure infrastructure. However, any network traffic destined for the internet or a component in another cloud involves the public internet's router resources, which you have no control over regarding resource impact measurement or utilization.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Reduce Latency using [Proximity Placement Groups](/azure/aks/reduce-latency-ppg)
- Maximize network utilization within the same cloud and, if possible, within the same region.
- Since the cost can be a proxy for sustainability, review the [Azure regions](/azure/architecture/framework/cost/design-regions) documentation in the Cost Optimization pillar of the Azure Well-Architected Framework.

## Security monitoring

Use cloud native security monitoring solutions to optimize for sustainability.

### Use cloud native log collection methods where applicable

Traditionally, log collection methods for ingestion to a Security Information and Event Management (SIEM) solution required the use of an intermediary resource to collect, parse, filter and transmit logs onward to the central collection system. Using this design can carry an overhead with more infrastructure and associated financial and carbon-related costs.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Use [Best Practices for Monitoring Cloud Applications](/azure/architecture/framework/devops/monitor-collection-data-storage)
- USe [Best Practices for Monitoring Microservices Application on AKS](/azure/architecture/microservices/logging-monitoring)

**Potential Tradeoffs:**
- Consider this tradeoff: Deploying more monitoring agents will increase the overhead in processing as it needs more compute resources. Carefully design and plan for how much information is needed to cover the security requirements of the solution and find a suitable level of information to store and keep.


### Avoid transferring large unfiltered data sets from one cloud service provider to another

Conventional SIEM solutions required all log data to be ingested and stored in a centralized location. In a multicloud environment, this solution can lead to a large amount of data being transferred out of a cloud service provide and into another, causing increased burden on the network and storage infrastructure.

_Green Software Foundation alignment: [Carbon efficiency](sustainability-design-principles.md#carbon-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Cloud native security services can perform localized analysis on relevant security data source. This analysis allows the bulk of log data to remain within the source cloud service provider environment. Cloud native SIEM solutions can be [connected via an API or connector](/azure/sentinel/connect-aws) to these security services to transmit only the relevant security incident or event data. This solution can greatly reduce the amount of data transferred while maintaining a high level of security information to respond to an incident.

In time, using the described approach helps reduce data egress and storage costs, which inherently help reduce emissions.

### Filter or exclude log sources before transmission or ingestion into a SIEM

Consider the complexity and cost of storing all logs from all possible sources. For instance, applications, servers, diagnostics and platform activity.

_Green Software Foundation alignment: [Carbon efficiency](sustainability-design-principles.md#carbon-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- When designing a log collection strategy for cloud native SIEM solutions, consider the use cases based on the [Microsoft Sentinel analytics rules](/azure/sentinel/detect-threats-built-in) required for your environment and match up the required log sources to support those rules.
- This option can help remove the unnecessary transmission and storage of log data, reducing the carbon emissions on the  environment.

### Archive log data to long-term storage

Many customers have a requirement to store log data for an extended period due to regulatory compliance reasons. In these cases, storing log data in the primary storage location of the SIEM system is a costly solution.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Log data can be [moved out to a cheaper long-term storage option](https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/move-your-microsoft-sentinel-logs-to-long-term-storage-with-ease/ba-p/1407153) which respects the retention policies of the customer, but lowers the cost by utilizing separate storage locations.

## Network architecture

Increase the efficiency and avoid unnecessary traffic by following good practices for network security architectures.

### Use cloud native network security controls to eliminate unnecessary network traffic

When you use a centralized routing- and firewall design, all network traffic is sent to the hub for inspection, filtering, and onward routing. While this approach centralizes policy enforcement, it can create an overhead on the network of unnecessary traffic from the source resources.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency), [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Use [Network security groups](/azure/virtual-network/network-security-groups-overview) 
- Use [Network Policies](/azure/aks/use-network-policies)
- Filter [Ingress traffic](/azure/application-gateway/ingress-controller-overview)
- Filter [egress traffic](/azure/aks/limit-egress-traffic)

### Minimize routing from endpoints to the destination

In many customer environments, especially in hybrid deployments, all end user device network traffic is routed through on-premises systems before being allowed to reach the internet. Usually, this happens due to the requirement to inspect all internet traffic. Often, this requires higher capacity network security appliances within the on-premises environment, or more appliances within the cloud environment.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Minimize routing from endpoints to the destination.
  - Where possible, end user devices should be optimized to [split out known traffic directly to cloud services](/microsoft-365/enterprise/microsoft-365-vpn-implement-split-tunnel) while continuing to route and inspect traffic for all other destinations. Bringing these capabilities and policies closer to the end user device prevents unnecessary network traffic and its associated overhead.

### Use network security tools with auto-scaling capabilities

Based on network traffic, there will be times when demand of the security appliance will be high, and other times where it will be lower. Many network security appliances are deployed to a scale to cope with the highest expected demand, leading to inefficiencies. Additionally, reconfiguration of these tools often requires a reboot leading to unacceptable downtime and management overhead.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- Making use of auto-scaling allows the rightsizing of the backend resources to meet demand without manual intervention.
- This approach will vastly reduce the time to react to network traffic changes, resulting in a reduced waste of unnecessary resources, and increases your sustainability effect.
- Learn more about relevant services by reading [how to enable a Web Application Firewall (WAF) on an Application Gateway](/azure/web-application-firewall/ag/application-gateway-web-application-firewall-portal), and [deploy and configure Azure Firewall Premium](/azure/firewall/premium-deploy).

### Evaluate whether to use TLS termination

Terminating and re-establishing TLS is CPU consumption that might be unnecessary in certain architectures.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Consider if you can terminate TLS at your border gateway and continue with non-TLS to your workload load balancer and onwards to your workload.
- Review the information on [TLS termination](/azure/application-gateway/ssl-overview#tls-termination) to better understand the performance and utilization impact it offers.
- Consider if you (really) need a [service mesh](/azure/aks/servicemesh-about)
- Consider [when to use Dapr with Or without a service mesh](https://docs.dapr.io/concepts/service-mesh/#when-to-use-dapr-or-a-service-mesh-or-both)

**Potential tradeoffs:**

- Consider the tradeoff: A balanced level of security can offer a more sustainable and energy efficient workload while a higher level of security may increase the requirements on compute resources.

### Use DDoS protection

Distributed Denial of Service (DDoS) attacks aim to disrupt operational systems by overwhelming them, creating a significant impact on the resources in the cloud. Successful attacks flood network and compute resources, leading to an unnecessary spike in usage and cost.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency), [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- DDoS protection seeks to [mitigate attacks at an abstracted layer](/azure/ddos-protection/types-of-attacks), so the attack is mitigated before reaching any customer operated services.
  - Mitigating any malicious usage of compute and network services will ultimately help reduce unnecessary carbon emissions.

## Endpoint security

It's imperative that we secure our workloads and solutions in the cloud. Understanding how we can optimize our mitigation tactics all the way down to the client devices can have a positive outcome for reducing emissions.

### Integrate Microsoft Defender for Endpoint

Many attacks on cloud infrastructure seek to misuse deployed resources for the attacker's direct gain. Two such misuse cases are botnets and crypto mining.

Both of these cases involve taking control of customer-operated compute resources and use them to either create new cryptocurrency coins, or as a network of resources from which to launch a secondary action like a DDoS attack, or mass e-mail spam campaigns.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendations:**

- [Enable Microsoft Defender for Containers](/azure/defender-for-cloud/defender-for-containers-introduction)
- [Identify vulnerable container images](/azure/defender-for-cloud/defender-for-containers-cicd)
- The EDR capabilities provide advanced attack detections and are able to take response actions to remediate those threats. The unnecessary resource usage created by these common attacks can quickly be discovered and remediated, often without the intervention of a security analyst.

## Reporting

Getting the right information and insights at the right time is important for producing reports around emissions from your security appliances.

### Tag security resources

It can be a challenge to quickly find and report on all security appliances in your tenant. Identifying the security resources can help when designing a strategy for a more sustainable operating model for your business.

_Green Software Foundation alignment: [Measuring sustainability](sustainability-design-principles.md#measuring-sustainability)_

**Recommendation:**

- [Use Tags](/azure/aks/use-tags).



## Next step

Review the design considerations for deployment and testing.

> [!div class="nextstepaction"]
> [Deployment and testing](/azure/architecture/framework/sustainability/sustainability-testing.md)
