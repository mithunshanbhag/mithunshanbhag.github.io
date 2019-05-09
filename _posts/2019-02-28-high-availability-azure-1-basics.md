---
layout: post
title: "High Availability in Azure (Part 1): The basics"
comments: true
---
_Note: This blog post is part 1 of a series centered around the topic of high availability in Azure:_

* _**Part 1: The basics (this post)**_
* _Part 2: SLAs and the 9s (coming soon)_
* _[Part 3: Availability Sets]({{ site.baseurl }}{% post_url 2019-03-29-high-availability-azure-3-availability-sets %})_
* _[Part 4: Availability Zones]({{ site.baseurl }}{% post_url 2019-03-31-high-availability-azure-4-availability-zones %})_
* _[Part 5: Storage redundancies]({{ site.baseurl }}{% post_url 2019-03-02-high-availability-azure-5-storage %})_
* _Part 6: Load balancing (coming soon)_
* _Part 7: Application gateways (coming soon)_
* _[Part 8: Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _[Part 9: App Service, Function Apps]({{ site.baseurl }}{% post_url 2019-03-23-high-availability-azure-9-apps %})_
* _Part 10: SQL (coming soon)_
* _Part 11: CosmosDB (coming soon)_
* _Part 12: Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

-----

## High availability what now?

In order to understand high availability in Azure, we first need to dig into some underlying Azure concepts. To explain these, I've cobbled together a diagram (it's not 100% accurate, but it does make it simpler to explain things).

[![azure global infrastructure](../../../images/04-azure-global-infra.jpg)](../../../images/04-azure-global-infra.jpg)

#### Geography

The "highest-level" entity that exists to meet [data residency](https://azuredatacentermap.azurewebsites.net/), compliance and sovereignty requirements. Currently there are [4 Azure geographies](https://azure.microsoft.com/en-us/global-infrastructure/geographies/) - Americas, Europe, Asia Pacific and Middle East + Africa. An Azure geography contains two or more Azure regions within it.

#### Region

As of the time of writing this post, there are [42 Azure regions](https://azure.microsoft.com/en-us/global-infrastructure/regions/) (with 12 more announced) spread across 4 Azure geographies. Each Azure region contains a inter-connected set of datacenters (all datacenters within an azure region are connected via a dedicated regional low-latency network).

_[image attribution: [Azure website](https://azure.microsoft.com/en-us/global-infrastructure/regions/)]_
[![azure regions](../../../images/01-azure-regions.jpg)](https://azure.microsoft.com/en-us/global-infrastructure/regions/)

[Some Azure regions](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#regions-that-support-availability-zones) support availability zones (each such region contains 3 or more availability zones).

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)]_
[![azure regions](../../../images/02-azure-availability-zones.jpg)](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

#### Paired Regions

It is recommended that your redundancies span across a set of [paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) in order to meet data residency & compliance requirements even during planned platform maintenance & outages. Azure ensures that during planned platform maintenance, only one region in each pair is updated at a time. Also during multi-regional outages, azure ensures that at least one region in each pair will be prioritized for recovery.

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions)]_
[![azure regions](../../../images/03-azure-paired-regions.jpg)](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions)

#### Availability Zone

An [availability zone](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview) comprises of one or more datacenters. Each availability zone has its own autonomous, independent infrastructure for power, cooling, and networking.

The Azure resources that support availability zones are [listed here](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#services-that-support-availability-zones). Please note that these Azure resources can be categorized as follows:

* _**zone-specific (zonal) resources**_: Azure ensures that the resources are contained within a specific availability zone. VMs, managed disks and IP addresses fall in this category.
* _**zone-redundant resources**_: Azure automatically replicates the resources across multiple availability zones. Zone-redundant storage accounts and SQL databases fall in this category.
* _**non-zonal (regional) resources**_: Azure resource that are not supported by availability zones.

I'll talk about availability zones in detail in a future blog post in this series.

#### Datacenter

You can watch one of [Mark Russinovich](https://twitter.com/markrussinovich)'s excellent presentations ([link1](https://www.youtube.com/watch?v=D8hMu4jJAwo), [link2](https://www.youtube.com/watch?v=m7I8ANssACk), [link3](https://www.youtube.com/watch?v=t3Vo37V9oU8) and [link4](https://youtu.be/S2zguwKvlQk)) to peek into what an Azure datacenter comprises of. Also you can take a [virtual tour](https://cloud-platform-assets.azurewebsites.net/datacenter/index.html) of an Azure datacenter.

#### Fault domain (physical server rack)

A single physical rack is considered as a fault domain, since all servers in that rack are connected by common points of failure (common power source and common network switch).

#### Update domain

An update domain is a logical grouping of machines that Azure upgrades/patches simultaneously during planned platform maintenance.

#### Availability Set

It's always a bad idea to run a production workload on a single VM. Best to provision multiple VMs in an [availability set](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#availability-sets), which is a logical grouping of VMs within a datacenter across multiple [fault & update domains](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#fault-domains).

When you create multiple VMs within an availability set, Azure distributes them across these fault & update domains. This ensures that at least one VM is remains running in event of either a planned platform maintenance (only one update domain in an availability set is patched at a time) or in the event of a server rack facing hardware failure, network outage or power supply issues.

My next blog post will explore availability sets for VMs in detail.

## Preemptive FAQs

#### What about VM scale sets?

[VM scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview) exist for horizontal scaling under load. In _**my**_ opinion, they have almost nothing to do with redundancies for high availability. So I'll be excluding them from this particular blog series. Perhaps I'll address them in a future series on horizontal & vertical scaling for Azure resources.

Aside: Horizontal scaling & high availability address slightly different issues (performance & reliability respectively). The former adds additional instances when under load to ensure performant service. The latter adds redundant instances (irrespective of load) to prevent service disruption during outages.

#### Will I address high availability on Azure's government cloud?

No. I know very little about Azure's government cloud. You're welcome to read the [documentation](https://docs.microsoft.com/en-in/azure/azure-government/) yourself.

<br>
_Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._