---
layout: post
title: "High Availability in Azure (Part 3): Availability Sets"
comments: true
---
_Note: This blog post is part 6 of a series centered around the topic of high availability in Azure:_

* _[Part 1: The basics]({{ site.baseurl }}{% post_url 2019-02-28-high-availability-azure-1-basics %})_
* _Part 2: SLAs and the 9s (coming soon)_
* _**Part 3: Availability Sets (this post)**_
* _Part 4: Availability Zones (coming soon)_
* _[Part 5: Storage redundancies]({{ site.baseurl }}{% post_url 2019-03-02-high-availability-azure-5-storage %})_
* _Part 6: Load balancing (coming soon)_
* _Part 7: Application gateways (coming soon)_
* _[Part 8: Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _[Part 9: App Service, Function Apps]({{ site.baseurl }}{% post_url 2019-03-23-high-availability-azure-9-apps %})_
* _Part 10: SQL (coming soon)_
* _Part 11: CosmosDB (coming soon)_
* _Part 12: Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

---

## Azure Availability Sets

![azure availability sets](../../../images/19-azure-availability-set.png)

We've already discussed the concepts of [fault domains](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#fault-domains), [update domains](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#update-domains) and [availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#availability-sets) in the [first post of this series](../../../2019/02/28/high-availability-azure-1-basics.html#fault-domain-physical-server-rack). Visually, you can represent an availability set with a table as follow:

-------|FD0|FD1|FD2
-------|---|---|---
**UD0**|VM1|   |VM6
**UD1**|   |VM2|
**UD2**|   |   |VM3
**UD3**|VM4|   |
**UD4**|   |VM5|

No two VMs in an availability set share the same fault & update domain. This ensures that there will be at least one available VM in the event of a planned maintenance (where an entire update domain is affected) or hardware failure (where an entire fault domain is affected). The [SLA for Azure VMs](https://azure.microsoft.com/en-in/support/legal/sla/virtual-machines/v1_8/) guarantees that if an availability set has two or more VMs, then at least one VM will be available 99.95% of the time.

Availability sets are free (you're only charged for the VMs placed in the availability sets).

## Caveats, restrictions, gotchas & tidbits

* A VM must be placed in an availability set at the time of creation. Once created, it can't be moved into an availability set. Also it's [not possible to change an existing VM's availability set](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/change-availability-set).

* An availability set forces all its associated VMs to:
  * Be in the same resource group and region (technically they all reside in the same data center actually).
  * Have their network interfaces associated with the same VNET.

* For HA, a VM can be placed in an availability set or in an availability zone. But NOT both. The former offers HA within a datacenter, the latter offers HA within a region.
![availability set vs availability zone](../../../images/20-azure-avset-vs-avzone.jpg)

* All VMs in an availability set need not be identical, but there are hardware size constraints. Use the [Get-AzVmSize](https://docs.microsoft.com/en-us/powershell/module/az.compute/get-azvmsize?view=azps-1.6.0) powershell cmdlet to list all the VM sizes available for a particular availability set ([more details](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-availability-sets#check-for-available-vm-sizes)).

* For an availability set with (say) 3 FDs and 5 UDs, the [placement of the VMs will generally be as follows](https://blogs.msdn.microsoft.com/plankytronixx/2015/05/01/azure-exam-prep-fault-domains-and-update-domains/):
  * 1st VM: FD0, UD0
  * 2nd VM: FD1, UD1
  * 3rd VM: FD2, UD2
  * 4th VM: FD0, UD3
  * 5th VM: FD1, UD4
  * 6th VM: FD2, UD0
  * and so on...

* Generally an [availability set is paired with a load balancer](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#combine-a-load-balancer-with-availability-sets) for traffic equi-distribution amongst the VMs in that availability set.

* Pro tip: To ensure redundancies in all tiers of your n-tier application, [each tier should be placed in a separate availability set](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#configure-each-application-tier-into-separate-availability-sets).

* Pro tip: Use managed disks & managed availability sets for higher availability. Read more below.

## Managed disks and managed availability sets

#### The issue with unmanaged disks in an availability set

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)]_
[![unmanaged availability set](../../../images/21-azure-av-set-unmanaged-disks.jpg)](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability?#use-managed-disks-for-vms-in-an-availability-set)

The storage accounts associated with unmanaged disks in an availability set are all placed in a single storage scale unit (stamp), which then becomes a single point of failure.

#### Benefits of managed disks

With [Azure managed disks](https://docs.microsoft.com/en-gb/azure/virtual-machines/windows/managed-disks-overview), you no longer have to explicitly provision storage accounts to back your disks. Managed disks provide a convenient abstraction over storage accounts, blob containers and page blobs. Internally, managed disks use [LRS storage](../../../2019/03/02/high-availability-azure-5-storage.html#lrs-locally-redundant-storage) (3 redundant copies within a storage scale unit inside a single datacenter).

#### Managed disks go in managed availability sets

If you plan to use managed disks, please ensure you select the "aligned" option while creating the availability set. This effectively creates a managed availability set.

![creating managed availability set](../../../images/20-azure-managed-availability-set.jpg)

To [migrate VMs in an existing availability set to managed disks](https://docs.microsoft.com/en-gb/azure/virtual-machines/windows/migrate-to-managed-disks), the availability set itself needs to be [converted to a managed availability set](https://docs.microsoft.com/en-gb/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks). This can be done via the Azure portal or via the [Update-AzAvailabilitySet](https://docs.microsoft.com/en-us/powershell/module/az.compute/update-azavailabilityset?view=azps-1.6.0) powershell cmdlet. Once converted, only VMs with managed disks can be added to the availability set (existing VMs with unmanaged disks in the availability set will continue to operate as before).

Please note that the [max number of managed FDs will depend on the availability set's region](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#number-of-fault-domains-per-region).

#### Managed availability sets get it right

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)]_
[![unmanaged availability set](../../../images/22-azure-av-set-managed-disks.jpg)](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability?#use-managed-disks-for-vms-in-an-availability-set)

The managed disks in an availability set are all placed in a multiple storage scale units (stamps), aligned with VM FDs, avoiding a single point of failure. In the event of a storage scale unit failing, only VMs with managed disks in that storage scale unit will fail (other VMs will be unaffected). This increases the overall availability of the VMs in that availability set.

<br>
_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._
