---
layout: post
title: "High Availability in Azure (Part 10): SQL"
comments: true
---
_Note: This blog post is part 10 of a series centered around the topic of high availability in Azure:_

* _[Part 1: The basics]({{ site.baseurl }}{% post_url 2019-02-28-high-availability-azure-1-basics %})_
* _Part 2: SLAs and the 9s (coming soon)_
* _[Part 3: Availability Sets]({{ site.baseurl }}{% post_url 2019-03-29-high-availability-azure-3-availability-sets %})_
* _[Part 4: Availability Zones]({{ site.baseurl }}{% post_url 2019-03-31-high-availability-azure-4-availability-zones %})_
* _[Part 5: Storage redundancies]({{ site.baseurl }}{% post_url 2019-03-02-high-availability-azure-5-storage %})_
* _Part 6: Load balancing (coming soon)_
* _Part 7: Application gateways (coming soon)_
* _[Part 8: Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _[Part 9: App Service, Function Apps]({{ site.baseurl }}{% post_url 2019-03-23-high-availability-azure-9-apps %})_
* _**Part 10: SQL (this post)**_
* _Part 11: CosmosDB (coming soon)_
* _Part 12: Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

---

## Azure SQL

![azure storage account](../../../images/25-azure-sql.png)

## About
 - sla
 - single db vs elastic pool vs managed instance vs reserved cores vs vcore?
 - Tiers
    - (dtu-based) basic
    - (dtu-based) standard
    - (dtu-based) premium (read scale out enabled|disabled)
    - (vcore-based) general purpose
    - (vcore-based) hyperscale
    - (vcore-based) business critical (read scale out enabled|disabled)


## internal replication
- Built-in high availability in Azure SQL Database guarantees that database will never be single point of failure in your software architecture.

tenant ring: an azure cluster, where each node runs sqlservice engine??
control ring: ???
LS (local storage) vs RS (remote storage)
- basic/standard tier: separated compute & storage nodes. 
Standard availability refers to 99.99% SLA that is applied in Basic, Standard, and General Purpose service tiers. High availability in this architectural model is achieved by separation of compute and storage layers and the replication of data in the storage tier.
A stateless compute layer that is running the sqlserver.exe process and contains only transient and cached data (for example – plan cache, buffer pool, column store pool). This stateless SQL Server node is operated by Azure Service Fabric that initializes process, controls health of the node, and performs failover to another place if necessary.
A stateful data layer with database files (.mdf/.ldf) that are stored in Azure Blob storage. Azure Blob storage guarantees that there will be no data loss of any record that is placed in any database file. Azure blog storage has built-in data availability/redundancy that ensures that every record in log file or page in data file will be preserved even if SQL Server process crashes.
Whenever database engine or operating system is upgraded, some part of underlying infrastructure fails, or if some critical issue is detected in Sql Server process, Azure Service Fabric will move the stateless SQL Server process to another stateless compute node. There is a set of spare nodes that is waiting to run new compute service in case of failover in order to minimize failover time. Data in Azure Blob storage is not affected, and data/log files are attached to newly initialized SQL Server process. This process guarantees 99.99% availability, but it might have some performance impacts on heavy workload that is running due to transition time and the fact the new SQL Server node starts with cold cache.

- premium tier
In the premium model, Azure SQL database integrates compute and storage on the single node. High availability in this architectural model is achieved by replication of compute (SQL Server Database Engine process) and storage (locally attached SSD) deployed in 4-node cluster, using technology similar to SQL Server Always On Availability Groups.
Both the SQL database engine process and underlying mdf/ldf files are placed on the same node with locally attached SSD storage providing low latency to your workload. High availability is implemented using technology similar to SQL Server Always On Availability Groups. Every database is a cluster of database nodes with one primary database that is accessible for customer workload, and a three secondary processes containing copies of data. The primary node constantly pushes the changes to secondary nodes in order to ensure that the data is available on secondary replicas if the primary node crashes for any reason. Failover is handled by the Azure Service Fabric – one secondary replica becomes the primary node and a new secondary replica is created to ensure enough nodes in the cluster. The workload is automatically redirected to the new primary node.

- hyperscale (vCore model only)

geo-replication vs active geo-replication?

[Mithun] DB Replication can be done in one of two ways:
1: Active geo-replication: Proactively create read-only secondaries for a db
2: Failover group: When a db is in a failover group, it's read-only secondary is automatically created in the failover's secondary region.

## geo-replication

The secondary database has the same name as the primary database and has, by default, the same service tier and compute size. The secondary database can be a single database or a pooled database. For more information, see DTU-based purchasing model and vCore-based purchasing model. After the secondary is created and seeded, data begins replicating from the primary database to the new secondary database.

Active geo-replication is Azure SQL Database feature that allows you to create readable secondary databases of individual databases on a SQL Database server in the same or different data center (region).

Up to four secondaries are supported in the same or different regions, and the secondaries can also be used for read-only access queries.

Active geo-replication asynchronously replicates committed transactions on the primary database to a secondary database using snapshot isolation(?). AWhile at any given point, the secondary database might be slightly behind the primary database, the secondary data is guaranteed to never have partial transactions. 

In addition to disaster recovery active geo-replication can be used in the following scenarios:

Database migration: You can use active geo-replication to migrate a database from one server to another online with minimum downtime.
Application upgrades: You can create an extra secondary as a fail back copy during application upgrades.

the secondary database is populated with the data copied from the primary database. This process is known as seeding. After secondary database has been created and seeded, updates to the primary database are asynchronously replicated to the secondary database automatically. Asynchronous replication means that transactions are committed on the primary database before they are replicated to the secondary database.

An application can access a secondary database for read-only operations using the same or different security principals used for accessing the primary database. The secondary databases operate in snapshot isolation mode to ensure replication of the updates of the primary (log replay) is not delayed by queries executed on the secondary.
Note: The log replay is delayed on the secondary database if there are schema updates on the Primary. The latter requires a schema lock on the secondary database.


size restrictions: Both primary and secondary databases are required to have the same service tier. 

User-controlled failover and failback


## forced(manual) failover??

Planned failover

Planned failover performs full synchronization between primary and secondary databases before the secondary switches to the primary role. This guarantees no data loss. Planned failover is used the following scenarios: 
(a) to perform DR drills in production when the data loss is not acceptable; 
(b) to relocate the database to a different region; and 
(c) to return the database to the primary region after the outage has been mitigated (failback).

Unplanned (forced, manual) failover

Unplanned or forced failover immediately switches the secondary to the primary role without any synchronization with the primary. This operation will result in data loss. Unplanned failover is used as a recovery method during outages when the primary is not accessible. When the original primary is back online, it will automatically re-connect without synchronization and become a new secondary.

Selected secondary becomes new primary.
Previous primary becomes new secondary.
All other previous secondaries remain secondaries.

planned vs unplanned failover
A secondary database can explicitly be switched to the primary role at any time by the application or the user. During a real outage the “unplanned” option should be used, which immediately promotes a secondary to be the primary. When the failed primary recovers and is available again, the system automatically marks the recovered primary as a secondary and bring it up-to-date with the new primary. Due to the asynchronous nature of replication, a small amount of data can be lost during unplanned failovers if a primary fails before it replicates the most recent changes to the secondary. When a primary with multiple secondaries fails over, the system automatically reconfigures the replication relationships and links the remaining secondaries to the newly promoted primary without requiring any user intervention. After the outage that caused the failover is mitigated, it may be desirable to return the application to the primary region. To do that, the failover command should be invoked with the “planned” option.


## auto failover groups

auto-failover groups provide the group semantics on top of active geo-replication but the same asynchronous replication mechanism is used. 
If you are using auto-failover groups, your secondary database is always created in a different region.

If you add a single database to the failover group, it automatically creates a secondary database using the same edition and compute size on secondary server. You specified that server when the failover group was created. If you add a database that already has a secondary database in the secondary server, that geo-replication link is inherited by the group. When you add a database that already has a secondary database in a server that is not part of the failover group, a new secondary is created in the secondary server.

## data sync / sync groups???

<br>
_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._


## open questions

chaining (secondary of secondaries)??? If you are using active geo-replication to build a globally distributed application and need to provide read-only access to data in more than four regions, you can create secondary of a secondary (a process known as chaining). This way you can achieve virtually unlimited scale of database replication. In addition, chaining reduces the overhead of replication from the primary database. The trade-off is the increased replication lag on the leaf-most secondary databases.
https://blogs.msdn.microsoft.com/azuresqldbsupport/2018/11/20/azure-sql-db-more-than-4-readable-secondaries/


scalability
scale up/down: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-scale-resources
read scale out: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-read-scale-out

high availability: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-high-availability

geo-replication: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-active-geo-replication
geo-replication-portal: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-active-geo-replication-portal

failover
auto-failover groups: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auto-failover-group
business continuity: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-business-continuity
outage recovery: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-disaster-recovery

info on tenant rings: https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-sql-database-for-gaming-industry-workloads/Azure%20Sql%20DB%20for%20Gaming%20industry.pdf

https://books.google.co.in/books?id=jpHlDAAAQBAJ&pg=PA129&lpg=PA129&dq=sql+azure+control+ring+tenant+ring&source=bl&ots=_iQniYP917&sig=ACfU3U2fzcPDGNpTnrdSVMe332b4wBGtmQ&hl=en&sa=X&ved=2ahUKEwidoYjmvMDhAhVr6nMBHb_vDfMQ6AEwAXoECAgQAQ#v=onepage&q&f=true

https://slideplayer.com/slide/10844302/

https://sqlbits.com/Sessions/Event12/Azure_SQL_DB-running_a_cloud_database_service_at_scale

http://www.azuredlake.com/2017/12/20/design-a-hybrid-sql-server-solution/
Azure SQL Database PaaS offering has made the promise that High Availability (HA) is built in to the service and the customers are not be required to operate, add special logic to, or make decisions around HA. Azure SQL DB uses Local Storage (LS) on direct attached disks/VHDs and Remote Storage (RS) based on Azure premium storage page blobs.

Local storage is used in premium databases and pools, which are designed for OLTP applications with high IOPS requirements
Remote storage is used for Basic and Standard service tiers. Designed for database that require storage and compute power to scale independently.
In both the cases, the replication, failure detection, and failover mechanisms of SQL database are fully automated and operated without human intervention. This architecture is designed to ensure that committed data is never lost and that data durability takes precedence over all else.

The high availability solution in SQL Database is based on AlwaysON technology from SQL Server and makes it work for both LS and RS databases with minimal differences. In LS configuration, AlwaysON is used for persistence while in RS is it used for availability (low RTO). Alwayson availability group works based on Windows server failover clustering.

In case of Local Storage one Primary replica and at least two secondary replicas located within a tenant ring, Which is three independent physical syb systems on WSFC. All the read and writes are sent to Primary replica through Gateway (GW). Writes are asynchronously sent to secondary replica. SQL Database uses a quorum-based commit scheme where data is written to the primary and at least one secondary replica before the transaction commits.

For Remote Storage, one copy of database is maintained at remote blob store. During planned downtime, a SQL instance is spun off in advance, then the remote replica is restored. During unplanned downtime, Azure fabric manages the instance availability and, as a last step of recovery, attaches the remote database file.