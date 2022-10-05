---
title: Data and storage design considerations for sustainable workloads on Azure
description: This design area explores considerations around storage design for sustainable workloads on Azure.
author: Zimmergren
ms.author: tozimmergren
ms.topic: conceptual
ms.date: 10/12/2022
ms.service: architecture-center
ms.subservice: well-architected
categories: 
  - storage
products: Azure
ms.custom:
  - sustainability
---

# Data and storage design considerations for sustainable workloads on Azure

Data storage in Azure is a crucial component of most provisioned workloads. Learn how to design for a more sustainable data storage architecture and optimize existing deployments.

> [!IMPORTANT]
> This article is part of the [Azure Well-Architected sustainable workload](index.yml) series. If you aren't familiar with this series, we recommend you start with [what is a sustainable workload?](sustainability-get-started.md#what-is-a-sustainable-workload)

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

Review the design considerations for security.

> [!div class="nextstepaction"]
> [Security](sustainability-security.md)
