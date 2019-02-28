---
layout: post
title: "High Availability in Azure (Part 3): Availability Zones"
sitemap: false
---

https://dzone.com/refcardz/scalability?chapter=1

An Availability Zone in an Azure region is a combination of a fault domain and an update domain. For example, if you create three or more VMs across three zones in an Azure region, your VMs are effectively distributed across three fault domains and three update domains. The Azure platform recognizes this distribution across update domains to make sure that VMs in different zones are not updated at the same time.

To achieve comprehensive business continuity on Azure, build your application architecture using the combination of Availability Zones with Azure region pairs. You can synchronously replicate your applications and data using Availability Zones within an Azure region for high-availability and asynchronously replicate across Azure regions for disaster recovery protection.


scale sets (and placement groups)
site recovery

https://azure.microsoft.com/en-in/features/resiliency/

# Storage
storage redundancy
storage stamp
https://azure.microsoft.com/en-us/global-infrastructure/availability-zones/

https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
https://docs.microsoft.com/en-us/azure/storage/common/storage-initiate-account-failover?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
https://docs.microsoft.com/en-us/azure/storage/common/storage-performance-checklist?toc=%2fazure%2fstorage%2fblobs%2ftoc.json


https://cloud.netapp.com/blog/azure-storage-behind-the-scenes


# databases
failover
replicas


# misc / web apps

azure application gateways
azure load balancers
azure traffic managers
azure front door
horizontal scaled instances (auto load-balanced)

https://docs.microsoft.com/en-us/azure/architecture/example-scenario/infrastructure/multi-tier-app-disaster-recovery
https://docs.microsoft.com/en-us/azure/architecture/example-scenario/infrastructure/wordpress

https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-load-balancing-azure

# misc links

RTO
RPO
active/active vs active/passive

https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency
https://docs.microsoft.com/en-us/azure/architecture/checklist/resiliency-per-service
https://docs.microsoft.com/en-us/azure/architecture/checklist/availability
https://docs.microsoft.com/en-us/azure/architecture/resiliency/

https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles/redundancy

https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions

https://docs.microsoft.com/en-us/azure/architecture/resiliency/disaster-recovery-azure-applications