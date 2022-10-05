---
title: Application design of sustainable workloads on Azure
description: This design area explores application design considerations for sustainable workloads on Azure.
author: Zimmergren
ms.author: tozimmergren
ms.topic: conceptual
ms.date: 10/12/2022
ms.service: architecture-center
ms.subservice: well-architected
categories: 
  - networking
products: Azure
ms.custom:
  - sustainability
---

# Application design of sustainable workloads on Azure

When building new or updating existing applications, it's crucial to consider how the solution will impact the climate and if there are ways to improve and optimize. Learn about considerations and recommendations to optimize your code and applications for a more sustainable application design.

> [!IMPORTANT]
> This article is part of the [Azure Well-Architected sustainable workload](index.yml) series. If you aren't familiar with this series, we recommend you start with [what is a sustainable workload?](sustainability-get-started.md#what-is-a-sustainable-workload)

## Code efficiency

Demands on applications can vary, and it's essential to consider ways to stabilize the utilization to prevent over- or underutilization of resources, which can lead to unnecessary energy spills.

### Evaluate moving monoliths to a microservice architecture

Monolithic applications usually scale as a unit, leaving little room to scale only the individual components that may need it.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency), [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**
- A microservice architecture allows for scaling of only the necessary components during peak load; ensuring idle components are scaled down or in. Additionally, it may reduce the overhead and resources required for deploying monolithic applications.

- Evaluate the [microservice architecture on AKS](/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices-advanced) guidance.
- Then, Evaluate the [microservice advanced architecture on AKS](/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices-advanced) guidance.
- Build serverless Applications using [Keda](https://keda.sh/) ; Use it as an [AKS addon](/azure/aks/keda-about)
- Build Microservices Applications using [Dapr](https://dapr.io/) ; Use it as an [AKS addon](/azure/aks/dapr)
- Build [CNCF Projects on AKS](/azure/architecture/example-scenario/apps/build-cncf-incubated-graduated-projects-aks)

**Potential tradeoffs:**

- Consider this tradeoff: While reducing the compute resources required, you may increase the amount of traffic on the network, and the complexity of the application may increase significantly.
- Consider this other tradeoff: Moving to microservices can result in extra deployment overhead with numerous similarities in deployment pipelines. Carefully consider the required deployment resources for monolithic versus microservice architectures.
- Additionally, read about [containerizing monolithic applications](/dotnet/architecture/containerized-lifecycle/design-develop-containerized-apps/monolithic-applications).



### Leverage cloud native design patterns

Learning about cloud-native design patterns is helpful for building applications, whether they're hosted on Azure or running elsewhere. Optimizing the performance and cost of your cloud application will also reduce its resource utilization, hence its carbon emissions.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency), [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- Build serverless Applications using [Keda](https://keda.sh/) ; Use it as an [AKS addon](/azure/aks/keda-about)
- Build Microservices Applications using [Dapr](https://dapr.io/) ; Use it as an [AKS addon](/azure/aks/dapr)
- Build [CNCF Projects on AKS](/azure/architecture/example-scenario/apps/build-cncf-incubated-graduated-projects-aks)


### Consider using circuit breaker patterns

Consider evaluating and preventing applications from performing operations that are likely to fail. Repeated failures can lead to overhead and unnecessary processing that you can avoid with proper design patterns.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- A circuit breaker can act as a proxy for operations that might fail and should monitor the number of recent failures that have occurred and use that information to decide whether to proceed.
- Study the [Circuit Breaker pattern](/azure/architecture/patterns/circuit-breaker), and then consider how you can [implement the Circuit Breaker patterns](/dotnet/architecture/microservices/implement-resilient-applications/implement-circuit-breaker-pattern) to your applications.
- Consider using [Azure Monitor](/azure/azure-monitor/overview) to monitor failures and set up alerts.


### Optimize for async access patterns

Demands on applications can vary, and it's essential to consider ways to stabilize the utilization to prevent over- or underutilization of resources, which can lead to unnecessary energy spills.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Queue and buffer requests that don't require immediate processing, then process in batch. Designing your applications in this way helps achieve a stable utilization and helps flatten consumption to avoid spiky requests.
- Read about optimizing for [async access patterns](/azure/architecture/patterns/async-request-reply).



### Establish CPU and Memory thresholds in testing

Help build tests for testing sustainability in your application. Consider having a baseline CPU utilization measurement, and detect abnormal changes to the CPU utilization baseline when tests run. With a baseline, suboptimal decisions made in recent code changes can be discovered earlier.

Adding tests and quality gates into the deployment and testing pipeline helps avoid deploying non-sustainable solutions, contributing to lowered emissions.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Use [Vertical Pod Autoscaler](/azure/aks/vertical-pod-autoscaler) to automatically set resource requests and limits on containers per workload based on past usage.
- Use AKS [advanced scheduler features](azure/aks/operator-best-practices-advanced-scheduler) to optimize scheduling your applications (pods), to nodes
- Enforce Kubernetes [Resource Quotas](/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)
- Monitor CPU and memory allocations when running integration tests or unit tests.
- Configure alerts or test failures if surpassing the established baseline values, helping avoid deploying non-sustainable workloads.

## Profiling and measuring

Measuring, profiling, and testing workloads are imperative to understanding how to best use allocated resources.

### Assess where parallelization is possible

Without properly profiling and testing workloads, it's difficult to know if it's making the best use of the underlying platform and deployed resources.

_Green Software Foundation alignment: [Measuring sustainability](sustainability-design-principles.md#measuring-sustainability)_

**Recommendation:**

- Test your applications to understand concurrent requests, simultaneous processing, and more.
- If you're running Machine Learning (ML) for tests, consider machines with a GPU for better efficiency gains.
- Identify if the workload is performance intensive and work toward optimization.
- _Consider this tradeoff:_ Running GPU-based machines for ML tests may increase the cost.


### Assess with chaos engineering

Running integration, performance and load tests increase the reliability of a workload. However, the introduction of chaos engineering can significantly help improve reliability and resilience and how the applications react to failures. In doing so, the workload can be optimized to handle failures gracefully and with less wasted resources.

_Green Software Foundation alignment: [Measuring sustainability](sustainability-design-principles.md#measuring-sustainability)_

**Recommendation:**

- Use [load testing](/azure/load-testing/tutorial-identify-performance-regression-with-cicd) and [chaos engineering](/azure/architecture/framework/resiliency/chaos-engineering) to assess how the workload handles platform outages and traffic spikes or dips. This helps increase service resilience and the ability to react to failures, allowing for a more optimized fault handling.


## Storage efficiency

Build solutions with efficient storage to increase performance, lower the required bandwidth, and minimize unnecessary storage design climate impact.

### 


### Use the best suited storage access tier

The carbon impact of data retrieved from hot storage can be higher than data from cold- or archive storage. Designing solutions with the correct data access pattern can enhance the application's carbon efficiency.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Use [storage best suited for the application's data access patterns](/azure/architecture/guide/design-principles/use-best-data-store).
- Choose [the appropriate storage type](/azure/aks/operator-best-practices-storage#choose-the-appropriate-storage-type).
- Use [Storage Classes to define application needs](/azure/aks/operator-best-practices-storage#create-and-use-storage-classes-to-define-application-needs)
- [Dynamically provision volumes](/azure/aks/operator-best-practices-storage#dynamically-provision-volumes).

### Only store what is relevant

Backup is a crucial part of reliability. However, storing backups indefinitely can quickly allocate much unnecessary disk space. Consider how you plan backup storage retention.

+ define storage class ; disk delete ; reclaim

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- Use [Storage Classes to define application needs](/azure/aks/operator-best-practices-storage#create-and-use-storage-classes-to-define-application-needs)
- Implement policies to streamline the process of storing and keeping relevant information. [Microsoft Purview](/azure/purview/overview) can help label data and add time-based purging to delete it after a retention period automatically. Additionally, this lets you stay in control of your data and reduces the amount of data to process and transfer.
- Workloads integrated with Azure Monitor can rely on [Data Collection Rules (DCR)](/azure/azure-monitor/essentials/data-collection-rule-overview) to specify what data should be collected, how to transform that data, and where to send the data.

### Determine the most suitable access tier for blob data (lifecycle)

Consider whether to store data in an online tier or an offline tier. Online tiers are optimized for storing data that is accessed or modified frequently. Offline tiers are optimized for storing data that is rarely accessed.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Read [Hot, Cool, and Archive access tiers for blob data](/azure/storage/blobs/access-tiers-overview).
  
### Reduce the number of recovery points for VM backups

Recovery points aren't automatically cleaned up. Therefore, consider where [soft delete](/azure/backup/backup-azure-security-feature-cloud) is enabled for Azure Backup. The expired recovery points aren't cleaned up automatically.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**


- (if possible), Aim for [Stateless Design](/azure/aks/operator-best-practices-multi-region#remove-service-state-from-inside-containers)
- Backup & restore [your persistent volumes](/azure/aks/operator-best-practices-storage#secure-and-back-up-your-data) 

### Revise backup and retention policies

Consider reviewing backup policies and retention periods for backups to avoid storing unnecessary data.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- Review and revise backup and retention policies to minimize storage overhead.
- Actively review and delete backups that are no longer needed.

### Optimize the collection of logs

Continuously collecting logs across workloads can quickly aggregate and store lots of unused data.

_Green Software Foundation alignment: [Energy efficiency](sustainability-design-principles.md#energy-efficiency)_

**Recommendation:**

- Make sure you are logging and retaining only data that is relevant to your needs.
- Read more about the [Cost optimization and Log Analytics](/azure/architecture/framework/services/monitoring/log-analytics/cost-optimization).
- Read more about [Monitoring AKS Data Reference](/azure/aks/monitor-aks-reference)

## Next step

Review the design considerations for the application platform.

> [!div class="nextstepaction"]
> [Application platform](sustainability-application-platform.md)
