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


## Next step

Review the design considerations for the application platform.

> [!div class="nextstepaction"]
> [Application platform](sustainability-application-platform.md)
