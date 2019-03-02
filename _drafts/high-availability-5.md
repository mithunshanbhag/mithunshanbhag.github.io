---
layout: post
title: "High Availability in Azure (Part 5): Storage Redundancy"
sitemap: false
---

Concepts
- storage stamp
- storage scale unit
- (no bearing on HA or SLA?) access tiers: hot, cool, archive 
- (no bearing on HA or SLA?) performance tiers: standard vs premium
  - A standard performance tier for storing blobs, files, tables, queues, and Azure virtual machine disks.
  - A premium performance tier for storing unmanaged virtual machine disks only.




Do we have control over where (which region, data center etc) the redundant copies are located?


General purpose v2 accounts only


Standard storage accounts are backed by magnetic drives and provide the lowest cost per GB. They're best for applications that require bulk storage or where data is accessed infrequently. Premium storage accounts are backed by solid state drives and offer consistent, low-latency performance. They can only be used with Azure virtual machine disks, and are best for I/O-intensive applications, like databases. Additionally, virtual machines that use Premium storage for all disks qualify for a 99.9% SLA, even when running outside of an availability set. This setting can't be changed after the storage account is created. 



Replication options for a storage account include:

Locally-redundant storage (LRS): A simple, low-cost replication strategy. Data is replicated within a single storage scale unit.
Zone-redundant storage (ZRS): Replication for high availability and durability. Data is replicated synchronously across three availability zones.
Geo-redundant storage (GRS): Cross-regional replication to protect against region-wide unavailability.
Read-access geo-redundant storage (RA-GRS): Cross-regional replication with read access to the replica.


=======


- how many redunant copies
- consistency / latency
- Promised SLA



- endpoint uris for replicas
- can redundant copies be accessed on-demand?
- restore from replica? failover vs azcopy etc?
- active/active vs active/passive
- consistency models & latency
- failover process? msft initiated vs user initiated
- data loss scenarios (full vs partial)
