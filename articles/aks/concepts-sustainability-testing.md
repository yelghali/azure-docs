---
title: Testing considerations for sustainable workloads on Azure
description: This design area explores testing and DevOps considerations for sustainable workloads on Azure.
author: Zimmergren
ms.author: tozimmergren
ms.topic: conceptual
ms.date: 10/12/2022
ms.service: architecture-center
ms.subservice: well-architected
categories: 
  - devops
products: Azure
ms.custom:
  - sustainability
---

# Testing considerations for sustainable workloads on Azure

Organizations developing and deploying solutions to the cloud also need reliable testing. Learn about the considerations and recommendations for running workload tests and how to optimize for a more sustainable testing model.

> [!IMPORTANT]
> This article is part of the [Azure Well-Architected sustainable workload](index.yml) series. If you aren't familiar with this series, we recommend you start with [what is a sustainable workload?](sustainability-get-started.md#what-is-a-sustainable-workload)


### Automate CI/CD to scale worker agents as needed

Running underutilized or inactive CI/CD agents results in more emissions.

_Green Software Foundation alignment: [Hardware efficiency](sustainability-design-principles.md#hardware-efficiency)_

**Recommendation:**

- Keeps the compute utilization high, based on the current demand, avoiding unnecessary capacity allocation.
- Only scale out when necessary, and when not testing, scale in. Ultimately this ensures there's no idle compute resources in test environments.
- Consider optimized platform services like AKS over testing in a VM, utilizing the platform to reduce maintenance.
- Use [Gitops on AKS to automate cluster & application lifecycle](/azure/architecture/example-scenario/gitops-aks/gitops-blueprint-aks), including testing & compliance.

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

**Potential tradeoffs:**

- _ Injecting fault during chaos engineering and increasing the load on any system also increases the emissions used for the testing resources. Evaluate how and when you can utilize chaos engineering to increase the workload reliability while considering the climate impact of running unnecessary testing sessions.
- Another angle to this is using chaos engineering to test energy faults or moments with higher carbon emissions: consider setting up tests that will challenge your application to consume the minimum possible energy. Define how the application will react to such conditions with a specific "eco" version informing users that they're emitting the minimum possible carbon by sacrificing some features and possibly some performance. This can also be your benchmark application for scoring its sustainability.

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

**Potential tradeoffs:**

- Consider this tradeoff: As applications grow, the baseline may need to shift accordingly to avoid failing the tests when introducing new features.

## Next step

Review the design considerations for operational procedures.

> [!div class="nextstepaction"]
> [Operational procedures](sustainability-operational-procedures.md)
