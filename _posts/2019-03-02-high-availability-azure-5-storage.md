---
layout: post
title: "High Availability in Azure: Storage redundancies"
comments: true
---
_Note: This blog post is part of a series centered around the topic of high availability in Azure:_

* _[The basics]({{ site.baseurl }}{% post_url 2019-02-28-high-availability-azure-1-basics %})_
* _SLAs and the 9s (coming soon)_
* _[Availability Sets]({{ site.baseurl }}{% post_url 2019-03-29-high-availability-azure-3-availability-sets %})_
* _[Availability Zones]({{ site.baseurl }}{% post_url 2019-03-31-high-availability-azure-4-availability-zones %})_
* _**Storage redundancies (this post)**_
* _Load balancing (coming soon)_
* _Application gateways (coming soon)_
* _[Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _[App Service, Function Apps]({{ site.baseurl }}{% post_url 2019-03-23-high-availability-azure-9-apps %})_
* _SQL (coming soon)_
* _CosmosDB (coming soon)_
* _Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

-----

## Azure Storage Account

![azure storage account](https://assets.cloudskew.com/assets/blog/images/05-azure-storage-account.jpg)

In Azure, the following entities are backed by Azure storage accounts: [blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview), [file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction), [queues](https://docs.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction), [NoSQL table storages](https://docs.microsoft.com/en-us/azure/storage/tables/table-storage-overview), [Data Lake Storage (gen2)](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) and unmanaged disks. In this blog post, we'll go over the [various redundancy options](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy) available for these storage accounts. We'll compare & contrast them based on the following parameters:

* Replication latency: How soon before all replicas are in full-sync?
* Disaster scenarios: Are you looking at partial data loss or fully unrecoverable data? How easy (or difficult) is it to get back on track once things have hit rock bottom?
* SLAs: How many 9s?

Hopefully this blog post will serve as a cheat-sheet and help you choose the right Azure storage redundancy options for your use cases.

<!-- markdownlint-disable-next-line MD033 -->
<br>
## LRS (locally-redundant storage)

With [LRS](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), your data is replicated thrice across multiple [fault domains](../../../2019/02/28/high-availability-azure-1-basics/#fault-domain-physical-server-rack) & [update domains](../../../2019/02/28/high-availability-azure-1-basics/#update-domain) within a single storage scale unit (all within a single datacenter). Note that all three replicas are addressed by a single endpoint (i.e. you can't target individual replicas for read/write operations).

**Replication latency**: No replication latency, data is synchronously written to all three replicas on every write request.

**Disaster scenarios**:

| disaster type                                                                                                                     | service interruption? | data loss?     | recovery possible? |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------- | -------------- | ------------------ |
| hardware failure in [physical rack](../../../2019/02/28/high-availability-azure-1-basics/#fault-domain-physical-server-rack)/node | NO                    | NO<sup>1</sup> | N/A                |
| [datacenter](../../../2019/02/28/high-availability-azure-1-basics/#datacenter) disaster                                           | YES                   | YES            | NO<sup>2</sup>     |
| [availability zone](../../../2019/02/28/high-availability-azure-1-basics/#availability-zone) disaster                             | "                     | "              | "                  |
| [regional](../../../2019/02/28/high-availability-azure-1-basics/#region) disaster                                                 | "                     | "              | "                  |
| [geographic](../../../2019/02/28/high-availability-azure-1-basics/#geography) disaster                                            | "                     | "              | "                  |
| worldwide disaster                                                                                                                | "                     | "              | "                  |

1. Since the replicas are spread across multiple fault domains.
2. Assuming all three replicas within the storage scale unit are affected, your data is permanently lost & unrecoverable.

**SLAs**:

object storage|>= 99.999999999% (11 nines)
read requests (hot tier)|>= 99.9% (3 nines)
read requests (cool tier)|>= 99% (2 nines)
write requests (hot tier)|>= 99.9% (3 nines)
write requests (cool tier)|>= 99% (2 nines)

<!-- markdownlint-disable-next-line MD033 -->
<br>
## ZRS (zone-redundant storage)

With [ZRS](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy-zrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), your data is replicated across three availability zones within the same region (please note that currently not all regions support availability zones). As in the earlier case with LRS, all three replicas are addressed by a single endpoint.

**Replication latency**: Very low latency, data is synchronously written to all three replicas on every write request.

**Disaster scenarios**:

| disaster type                                                                                                                     | service interruption? | data loss?     | recovery possible? |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------- | -------------- | ------------------ |
| hardware failure in [physical rack](../../../2019/02/28/high-availability-azure-1-basics/#fault-domain-physical-server-rack)/node | NO                    | NO<sup>1</sup> | N/A                |
| [datacenter](../../../2019/02/28/high-availability-azure-1-basics/#datacenter) disaster                                           | "                     | "              | "                  |
| [availability zone](../../../2019/02/28/high-availability-azure-1-basics/#availability-zone) disaster                             | YES<sup>2</sup>       | NO             | N/A                |
| [regional](../../../2019/02/28/high-availability-azure-1-basics/#region) disaster                                                 | YES                   | YES            | NO<sup>3</sup>     |
| [geographic](../../../2019/02/28/high-availability-azure-1-basics/#geography) disaster                                            | "                     | "              | "                  |
| worldwide disaster                                                                                                                | "                     | "              | "                  |

1. Only one replica will be affected, since the replicas are spread across different availability zones.
2. Temporary service interruption until Azure finishes DNS updates (not entirely sure how long these updates take, [the official docs](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy-zrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#what-happens-when-a-zone-becomes-unavailable) do not mention this). To mitigate this, best to use [transient fault-handling patterns](https://docs.microsoft.com/en-us/azure/architecture/best-practices/transient-faults) ([retries](https://docs.microsoft.com/en-us/azure/architecture/patterns/retry) with [back-offs](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy?view=azure-dotnet) and [circuit breakers](https://docs.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker)) for all reads/writes on the storage account. More details can be found [here](https://docs.microsoft.com/en-us/azure/architecture/best-practices/retry-service-specific#azure-storage) and [here](https://docs.microsoft.com/en-us/azure/storage/common/storage-designing-ha-apps-with-ragrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#handling-retries).
3. Assuming all three replicas across the availability zones are affected, your data is permanently lost & unrecoverable.

**SLAs**:

object storage|>= 99.9999999999% (12 nines)
read requests (hot tier)|>= 99.9% (3 nines)
read requests (cool tier)|>= 99% (2 nines)
write requests (hot tier)|>= 99.9% (3 nines)
write requests (cool tier)|>= 99% (2 nines)

<!-- markdownlint-disable-next-line MD033 -->
<br>
## GRS (geo-redundant storage)

With [GRS](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), your data is replicated across two [paired-regions](../../../2019/02/28/high-availability-azure-1-basics/#paired-regions) (within the same Azure geography) in a primary region + secondary region setup. This ensures that one regional replica will be available in the event of a regional disaster.

The primary region & the secondary regions are addressed by separate endpoints. The secondary endpoint is generally inaccessible. However in case of a [fail-over](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#understand-the-account-failover-process), the secondary is promoted to primary and read + write access is enabled for this endpoint. Fail-overs are automatically initiated by Azure in the event of a regional disaster. Azure is also introducing [user-initiated fail-overs](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#initiate-an-account-failover), which is currently in preview mode as of the time of writing this post.

_Note: Both GRS (geo-redundant storage) and RA-GRS (read-access geo-redundant storage) are misnomers. They don't create redundant copies across Azure geographies, only across paired-regions within the same Azure geography._

![azure storage GRS](https://assets.cloudskew.com/assets/blog/images/06-azure-storage-grs.jpg)

**Replication latency**: Your data is first replicated synchronously within the primary region via LRS. The data is then replicated asynchronously to the secondary region (eventually consistent). Within the secondary region, it is replicated synchronously using LRS. The [official SLA for Azure storage](https://azure.microsoft.com/en-us/support/legal/sla/storage/v1_3/) does not make any guarantees about the time needed for geo-replication.  

**Disaster scenarios**:

| disaster type                                                                                                                     | service interruption? | data loss?           | recovery possible? |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------- | -------------------- | ------------------ |
| hardware failure in [physical rack](../../../2019/02/28/high-availability-azure-1-basics/#fault-domain-physical-server-rack)/node | NO                    | NO                   | N/A                |
| [datacenter](../../../2019/02/28/high-availability-azure-1-basics/#datacenter) disaster                                           | YES<sup>1</sup>       | POSSIBLE<sup>2</sup> | YES<sup>3</sup>    |
| [availability zone](../../../2019/02/28/high-availability-azure-1-basics/#availability-zone) disaster                             | "                     | "                    | "                  |
| [regional](../../../2019/02/28/high-availability-azure-1-basics/#region) disaster                                                 | "                     | "                    | "                  |
| [geographic](../../../2019/02/28/high-availability-azure-1-basics/#geography) disaster                                            | YES                   | YES                  | NO                 |
| worldwide disaster                                                                                                                | "                     | "                    | "                  |

1. Within the primary & secondary regions itself, the data is replicated via LRS. In the event of a datacenter disaster in the primary region, it is possible that all replicas within the storage scale unit are affected and the primary endpoint will now be both inaccessible & unrecoverable. Although the secondary region has replica data, its endpoint will be inaccessible until a fail-over is initiated (the data will be inaccessible until the fail-over is complete).
2. With GRS, the replication from primary to secondary regions is asynchronous. In the event of the primary being destroyed before it has completely replicated the data to secondary, the secondary will have a stale copy and un-replicated writes will be permanently lost.
3. Only when a fail-over has completed, the secondary endpoint becomes the new primary, accessible for read + write operations, with LRS replication.

**SLAs**:

object storage|>= 99.99999999999999% (16 nines)
read requests (hot tier)|>= 99.9% (3 nines)
read requests (cool tier)|>= 99% (2 nines)
write requests (hot tier)|>= 99.9% (3 nines)
write requests (cool tier)|>= 99% (2 nines)

<!-- markdownlint-disable-next-line MD033 -->
<br>
## RA-GRS (read-access geo-redundant storage)

Same as GRS, but you always have read-only access to the secondary replica.

**Replication latency**: Same as GRS.

**Disaster scenarios**:

| disaster type                                                                                                                     | service interruption? | data loss?           | recovery possible? |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------- | -------------------- | ------------------ |
| hardware failure in [physical rack](../../../2019/02/28/high-availability-azure-1-basics/#fault-domain-physical-server-rack)/node | NO                    | NO                   | N/A                |
| [datacenter](../../../2019/02/28/high-availability-azure-1-basics/#datacenter) disaster                                           | YES<sup>1</sup>       | POSSIBLE<sup>2</sup> | YES<sup>3</sup>    |
| [availability zone](../../../2019/02/28/high-availability-azure-1-basics/#availability-zone) disaster                             | "                     | "                    | "                  |
| [regional](../../../2019/02/28/high-availability-azure-1-basics/#region) disaster                                                 | "                     | "                    | "                  |
| [geographic](../../../2019/02/28/high-availability-azure-1-basics/#geography) disaster                                            | YES                   | YES                  | NO                 |
| worldwide disaster                                                                                                                | "                     | "                    | "                  |

1. In the event of primary replica being destroyed, the secondary region will still have read-only access, even without a failover being initiated (unlike GRS where the secondary is inaccessible until a fail-over has been completed).
2. In the event of the primary being destroyed before it has completely replicated the data to secondary, the secondary will have a stale copy and un-replicated writes will be permanently lost (same as GRS).
3. Prior to fail-over, the secondary will have read-access. After fail-over, the secondary becomes the new primary, with read + write access and LRS replication.

**SLAs**:

object storage|>= 99.99999999999999% (16 nines)
read requests (hot tier)|>= 99.99% (4 nines)
read requests (cool tier)|>= 99.9% (3 nines)
write requests (hot tier)|>= 99.9% (3 nines)
write requests (cool tier)|>= 99% (2 nines)

<!-- markdownlint-disable-next-line MD033 -->
<br>
_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._
