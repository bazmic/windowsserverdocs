---
title: refs-overview
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 12/9/2016
ms.assetid: 
---
# Resilient File System
>Applies To: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

The Resilient File System (ReFS) is Microsoft's newest file system, designed to maximize data availibility, scale efficiently to large data sets across diverse workloads, and provide data integrity by means of resiliency to corruption. It seeks to address an expanding set of storage scenarios and establish a foundation for future innovations. 

## Key benefits

### **Resiliency**
ReFS introduces new features that can precisely detect corruptions and also fix those corruptions while remaining online, helping provide increased integrity and availibility for your data: 

- **Online repair** - Repair processes are both localized to the area of corruption and performed online, requiring no volume downtime. 
- **Storage Spaces integration** - When used in conjunction with a mirror or parity space, ReFS can automatically repair detected corruptions using the alternate copy of the data provided by Storage Spaces. 
- **Salvaging data** - If a volume becomes corrupted and an alternate copy of the corrupted data doesn't exist, ReFS removes the corrupt data from the namespace on a live volume. In other words, ReFS keeps the volume online while it handles non-correctable corruptions.
- **Integrity-streams** - ReFS introduces checksums for metadata and optionally for file data, giving ReFS the ability to reliably detect corruptions. 
- **Proactive error correction** - In addition to validating data before reads and writes, ReFS introduces a data integrity scanner, known as a <i>scrubber</i>. This scrubber periodically scans the volume, identifying latent corruptions and proactively triggering a repair of corrupt data. 

### **Performance**
In addition to providing resiliency improvements, ReFS introduces new features for performance-sensitive and virtualized workloads. Real-time tier optimization, block cloning, and sparse VDL are good examples of the evolving capabilities of ReFS, which are designed to support dynamic and diverse workloads:

- **Real-time tier optimization** - By dividing a volume into a performance tier and a capacity tier, ReFS can leverage real-time tier optimization to deliver both high performance and also capacity efficient storage for your data.
    - All writes will occur in the performance tier, and large chunks of data that remain in the performance tier will be efficiently moved to the capacity tier in real-time. 
    - If using a [hybrid deployment](../storage-spaces/choosing-drives-and-resiliency-types.md) (mixing flash and HDD devices), [the cache in Storage Spaces Direct](../storage-spaces/understand-the-cache.md) will help accelerate reads, reducing the effect of data fragmentation characteristic of virtualized workloads. Otherwise, if using an all-flash deployment, reads will also occur in the performance tier. 
    
- **Tiers with multiple resiliency types** - ReFS allows the performance tier and the capacity tier to have different resiliency types: for example, the performance tier and the capacity tier can use mirror and parity resiliency types respectively, helping to deliver fast storage for hot data and capacity-efficient storage for cold data. 
    
- **Block cloning** - Block cloning is central to accelerating VM operations, such as quick, low-impact VM checkpoint merge operations.  

- **Sparse VDL** - Sparse VDL also improves the efficiency of VM operations, reducing the time needed to create fixed VHDs from 10s of minutes to mere seconds. 

- **Variable cluster sizes** - ReFS supports both 4K and 64K cluster sizes. 4K is the recommended cluster size for most deployments, but 64K clusters are appropriate for large, sequential IO workloads.  

### **Scalability**
ReFS is designed to support extremely large data sets--petabytes and larger--without negatively impacting performance, achieving greater scale than prior file systems. 

## Supported deployments

**ReFS on Storage Spaces Direct** <br>
Deploying ReFS on Storage Spaces Direct is the recommended setup for virtualized workloads: 
- Real-time tier optimization and [the cache in Storage Spaces Direct](../storage-spaces/understand-the-cache.md) deliver high performance and capacity-efficient storage. 
- The introduction of block clone and sparse VDL dramatically accelerates .vhdx file operations, such as creation, merge, and expansion.
- Built-in checksums, online repair, and alternate data copies enable ReFS and Storage Spaces Direct to jointly to detect and correct corruptions within both metadata and data. 
- ReFS provides the functionality to efficiently scale and support massive data sets. 

**ReFS on Storage Spaces with SAS drive enclosures** <br>
Deploying ReFS on Storage Spaces with SAS drive enclosures is suitable for hosting archival data and storing user documents:
- Built-in checksums, online repair, and alternate data copies enable ReFS and Classic Storage Spaces to jointly to detect and correct corruptions within both metadata and data. 
- ReFS provides the functionality to efficiently scale and support massive data sets. 
- Data will be stored on a non-tiered volume. 

**ReFS on basic disks** <br>
Deploying ReFS on basic disks is suitable for applications that provide their own resiliency and availibility solutions. 
- For example, Exchange introduces its own concepts of resiliency and availability suited for its own workloads, and it benefits from the scalability offered in ReFS. 

## Feature comparison

### Limits

| Feature       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Maximum file name length | 32K Unicode characters  | 32K Unicode characters               |
| Maximum path name length |32K Unicode characters | 32K Unicode characters                |
| Maximum file size | 18 EB (exabytes)  | 18 EB (exabytes)                |
| Maximum volume size | 4.7 ZB (zettabytes)                           | 256 TB                |


### Functionality

| Functionality       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Object IDs | No | Yes |
| File system compression | No | Yes |
| File system encryption | No | Yes |
| BitLocker encryption | Yes | Yes |
| Transactions | No | Yes |
| Hard links | No | Yes |
| Soft links | Yes | Yes |
| Short names | No | Yes |
| Extended attributes | No | Yes |
| Disk quotas | No | Yes |
| Bootable | No | Yes |
| Supported on removable media | No | Yes |
| NTFS storage tiers | No | Yes |
| Data Deduplication | No | Yes |
| Failover cluster support | Yes | Yes |
| Cluster Shared Volume (CSV) support | Yes | Yes |
| Access-control lists | Yes | Yes |
| USN journal | Yes | Yes |
| Changes notifications | Yes | Yes |
| Junction points | Yes | Yes |
| Mount points | Yes | Yes |
| Reparse points | Yes | Yes |
| Volume snapshots | Yes | Yes |
| File IDs | Yes | Yes |
| Oplocks | Yes | Yes |
| Sparse files | Yes | Yes |
| Named streams | Yes | Yes |
| Block clone | Yes | No |
| Sparse VDL | Yes | No |
| Real-time tier optimization| Yes (on Storage Spaces Direct) | No |

## See also
[Storage Spaces Direct in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
