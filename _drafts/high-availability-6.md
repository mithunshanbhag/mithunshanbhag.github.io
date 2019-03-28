---
layout: post
title: "High Availability in Azure (Part 3): Availability Sets for VMs"
sitemap: false
---

Concepts
- storage stamp
- storage scale unit
  - A standard performance tier for storing blobs, files, tables, queues, and Azure virtual machine disks.
  - A premium performance tier for storing unmanaged virtual machine disks only.

<br>
## FAQs


layer-4 

IP address skus

Availability sets

Use of IP addresses (basic LB sku)
 * public IP | basic
 

Endpoints (basic sku)
	• VM
	• Availability Set
	• VM Scale Set

Endpoints (std sku)
	• VMs (from selected VNET)

HA ports?
multiple frontends
SLA

Standalone VMs, availability sets, and virtual machine scale sets can be connected to only one SKU, never both. 

When you use them with public IP addresses, both Load Balancer and the public IP address SKU must match. 

Load Balancer and public IP SKUs are not mutable.


Load Balancer instantly reconfigures itself when you scale instances up or down. Adding or removing VMs from the backend pool reconfigures the Load Balancer without additional operations on the Load Balancer resource.

The Load Balancer resource's functions are always expressed as a frontend, a rule, a health probe, and a backend pool definition. A resource can contain multiple rules. You can place virtual machines into the backend pool by specifying the backend pool from the virtual machine's NIC resource. This parameter is passed through the network profile and expanded when using virtual machine scale sets.

One key aspect is the scope of the virtual network for the resource. While Basic Load Balancer exists within the scope of an availability set, a Standard Load Balancer is fully integrated with the scope of a virtual network and all virtual network concepts apply.


What are the constraints related to Global VNet Peering and Load Balancers?
If the two virtual networks are in different region (Global VNet Peering), you cannot connect to resources that use Basic Load Balancer. You can connect to resources that use Standard Load Balancer. The following resources use Basic Load Balancers which means you cannot communicate to them across Global VNet Peering:
VMs behind Basic Load Balancers
VM Scale Sets with Basic Load Balancers
Redis Cache
Application Gateway (v1) SKU
Service Fabric
SQL Always-on
SQL MI
API Managemenet
ADDS
Logic Apps
HD Insight
Azure Batch
AKS
App Service Environment


**How to find the replication status for GRS and RA-GRS accounts?**
https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getservicestats?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetServiceStats_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_

https://webcache.googleusercontent.com/search?q=cache:XwJms1G3mSsJ:https://alexandrebrisebois.wordpress.com/2016/02/21/get-last-sync-time-for-read-access-geo-redundant-azure-storage/+&cd=1&hl=en&ct=clnk&gl=in

https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/

https://www.powershellgallery.com/packages/PSAzureToolkit/1.0.1/Content/Public%5CGet-AzureRmStorageGeoReplicationStats.ps1

Last sync time?

- FailOver
- FailBack

**Is it possible to switch between redundancy options?**

**Does the storage account's chosen access tier (hot, cool, archive) have any bearing on high availability?** 

- (no bearing on HA or SLA?) performance tiers: standard vs premium

Special patterns for RA-GRS?

====================


One key aspect is the scope of the virtual network for the resource. While Basic Load Balancer exists within the scope of an availability set, a Standard Load Balancer is fully integrated with the scope of a virtual network and all virtual network concepts apply.



Can a VNet span regions?
No. A VNet is limited to a single region. A virtual network does, however, span availability zones. To learn more about availability zones, see Availability zones overview. You can connect virtual networks in different regions with virtual network peering. For details, see Virtual network peering overview

VM in region1 cannot join VNET in region2

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/n-tier-sql-server#availability-considerations

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server

https://azure.microsoft.com/en-in/solutions/architecture/build-high-availability-into-your-bcdr-strategy/

things I still don't fully understand
- storage scale unit (stamps)
- hardware cluster (compute cluster. storage cluster etc)


- vm reboots & maintenance & downtime
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#understand-vm-reboots---maintenance-vs-downtime
- scheduled events for maintenance windows
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-scheduled-events-to-proactively-respond-to-vm-impacting-events


https://stackoverflow.com/a/49444606 (scale set vs availability sets)

Not sure if this link below is actually useful (this article was written before AV zones were available)
https://kvaes.wordpress.com/2016/06/01/azure-availability-patterns-for-iaas-can-i-do-multiple-regions/

Next two links are redundant
https://stackoverflow.com/a/46418623
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#managed-disk-fault-domains


youtube videos on availability sets
https://www.youtube.com/watch?v=ilXx0cmmGz0
https://www.youtube.com/watch?v=hS2ZURILI50
https://www.youtube.com/watch?v=7o5NeWjNoFQ
https://www.youtube.com/watch?v=eyZELEeYA6o
https://www.youtube.com/watch?v=PP02QxplC2E


SLA for AV zones is higher than that of AV Sets: https://azure.microsoft.com/en-in/support/legal/sla/virtual-machines/v1_8/