---
layout: post
title: "High Availability in Azure: Availability Zones"
comments: true
---
_Note: This blog post is part of a series centered around the topic of high availability in Azure:_

* _[The basics]({{ site.baseurl }}{% post_url 2019-02-28-high-availability-azure-1-basics %})_
* _SLAs and the 9s (coming soon)_
* _[Availability Sets]({{ site.baseurl }}{% post_url 2019-03-29-high-availability-azure-3-availability-sets %})_
* _**Availability Zones (this post)**_
* _[Storage redundancies]({{ site.baseurl }}{% post_url 2019-03-02-high-availability-azure-5-storage %})_
* _Load balancing (coming soon)_
* _Application gateways (coming soon)_
* _[Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _[App Service, Function Apps]({{ site.baseurl }}{% post_url 2019-03-23-high-availability-azure-9-apps %})_
* _SQL (coming soon)_
* _CosmosDB (coming soon)_
* _Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

---

## Azure Availability Zones

![azure availability zones](https://assets.cloudskew.com/assets/blog/images/02-azure-availability-zones.jpg)

In the [opening post of this blog series](../../../2019/02/28/high-availability-azure-1-basics/#availability-zone) we talked about availability zones and how resources can be classified as zone-redundant, zonal (zone-specific) or non-zonal (regional). If you haven't seen that post, please take a minute to do so.

Availability zones exist to shield your resources against a datacenter-level disaster.

As of the time of writing this blog post, [only a few Azure regions support availability zones](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#regions-that-support-availability-zones).

Availability zones are free (you're only charged for the VMs and resources placed in the availability zones).

## Supported Azure Resources

Only a few Azure resource types support availability zones (we're highlighting a couple of important ones below. The complete list is [available here](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#regions-that-support-availability-zones)).

* **Virtual Machines**: During creation, a VM can be configured as zonal. Its managed disk and public IP address (standard sku only) are then automatically placed in that same zone.

* **Managed Disks**: During creation, a managed disk can be configured as zonal or non-zonal. [Snapshots](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/snapshot-copy-managed-disk) of any managed disks (zonal or otherwise) can be be persisted to zone-redundant storage.

* **Public IPs**: During creation, a Public IP address (standard sku only) can be configured as zone-redundant (default) or zonal. Public IPs with basic sku are non-zonal.

* **Storage Accounts**: With zone-redundant storage, your data is replicated across three availability zones within the same region. We already covered ZRS storage in [part 5 of this blog series](../../../2019/03/02/high-availability-azure-5-storage/#zrs-zone-redundant-storage).

* **Load Balancers (standard sku only)**: During creation, load balancers (standard sku only) can be configured as zone-redundant or zonal. Load balancers with basic sku are non-zonal.

## Availability Sets vs Availability Zones

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/architecture/resiliency/)]_
[![availability zone vs availability set](https://assets.cloudskew.com/assets/blog/images/24-azure-avset-vs-avzone.jpg)](https://docs.microsoft.com/en-us/azure/architecture/resiliency/)

* Availability sets provide redundancies within a datacenter, while availability zones provide redundancies within a region. The former shields you against hardware failures in a physical rack, while the latter shields you against a datacenter-level disaster.

* SLA for VMs in availability zones is predictably higher (99.99% uptime guarantee) than that of VMs in availability sets (99.95% uptime guarantee). [Full SLA details here](https://azure.microsoft.com/en-in/support/legal/sla/virtual-machines/v1_8/).

* With an availability set, all VMs in it must belong to the same VNET and same resource group. However an availability zone imposes no such restrictions (zonal VMs can belong to any VNET and any resource group within the region).

* When placing a VM in an availability set, you cannot specify its placement (fault domain, update domain etc). However when placing a VM in an availability zone, you have to specify its zone.

## Caveats, restrictions, gotchas & tidbits

* Zonal resources, once created, cannot be moved to other availability zones within the region. It is however possible to [use Azure Site Recovery to move non-zonal VMs to availability zones in another region](https://docs.microsoft.com/en-us/azure/site-recovery/move-azure-vms-avset-azone).

* All VMs in an availability zone need not be identical

* To ensure redundancies in all tiers of your n-tier application, each tier should ideally be placed in a separate availability zone.

* Some additional caveats with zonal VMs:

  * A zonal VM can only attach to a public IP address that is zone-redundant or zonal (i.e. standard sku only. Basic skus don't have zonal support).
  * A zonal VM can only attach a managed disk from the same availability zone. A non-zonal VM can however attach any managed disk from the same region, irrespective of whether it's zonal or not.
  * It's not possible for a zonal VM to use unmanaged disks.
![vm in availability zone must use managed disks](https://assets.cloudskew.com/assets/blog/images/23-azure-availability-zone-managed-disk.jpg)

* Pro tip: [Pair zonal VMs with a zone-redundant load balancer (standard sku)](https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal) for traffic equi-distribution amongst the VMs in that availability zone. All the zonal VMs must be connected to the same VNET.

<br>
_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._
