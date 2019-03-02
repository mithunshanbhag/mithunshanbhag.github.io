---
layout: post
title: "High Availability in Azure (Part 3): Availability Sets for VMs"
sitemap: false
---

Higher SLA with av sets

open-questions
- relation of av sets to resource groups, regions etc 
- powershell cmdlets?
- vms need not be identical, but there are hardware size constraints
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-availability-sets#check-for-available-vm-sizes
- azure advisor
- av sets are free (no cost, you only pay for VMs used)
- use of managed disks
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#storage-availability
- fault domains for managed disks
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#managed-disk-fault-domains
- managed availability sets?
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability?#use-managed-disks-for-vms-in-an-availability-set
- changing availability sets
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/change-availability-set
- vm reboots & maintenance & downtime
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#understand-vm-reboots---maintenance-vs-downtime
- scheduled events for maintenance windows
https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-scheduled-events-to-proactively-respond-to-vm-impacting-events

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability


https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability

https://www.youtube.com/watch?v=ilXx0cmmGz0
https://www.youtube.com/watch?v=hS2ZURILI50
https://www.youtube.com/watch?v=7o5NeWjNoFQ
https://www.youtube.com/watch?v=eyZELEeYA6o
https://www.youtube.com/watch?v=PP02QxplC2E

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/n-tier-sql-server#availability-considerations

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server

https://azure.microsoft.com/en-in/solutions/architecture/build-high-availability-into-your-bcdr-strategy/

https://stackoverflow.com/a/49444606 (scale set vs availability sets)


https://blogs.msdn.microsoft.com/plankytronixx/2015/05/01/azure-exam-prep-fault-domains-and-update-domains/

https://kvaes.wordpress.com/2016/06/01/azure-availability-patterns-for-iaas-can-i-do-multiple-regions/