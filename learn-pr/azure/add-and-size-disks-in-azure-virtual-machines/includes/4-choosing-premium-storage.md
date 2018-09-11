Some applications place greater demands on data storage than others. For example, Microsoft Exchange servers and SharePoint servers need high-performance disks to run at their best.

Let's revisit the scenario of migrating the case histories database for your law firm into Azure. You want to make sure that your lawyers and other staff can access case details as fast as possible. The firm is growing,  and you want your database to handle increased demand.

One way you can  maximize the performance of virtual machines (VMs) in Azure is to use premium storage for virtual hard drives (VHDs). Premium storage delivers faster input and output (IO) than standard storage. Faster disk IO results in better performance for disk-intensive applications.

Let's compare the performance tiers for storage and learn more about premium storage accounts.

## How premium storage differs from standard storage

In Azure, premium storage is implemented on solid-state drives (SSDs). SSDs have higher IO performance than hard disk drives (HDDs) and can be more reliable because they have no moving parts. A read or write head doesn't have to move to the correct location on a disk to find the data requested. 

Standard storage is implemented on HDDs. Standard storage a billed at a lower rate. Choose standard storage to control costs, and choose premium storage when fast IO performance is required.

You can also choose to use a mix of standard and premium storage per disk in a single VM.

> [!NOTE]
> If you're using managed disks, you have a third option: standard SSDs. Standard SSDs are intermediate in both performance and price between standard HDDs and premium SSDs but are not available for unmanaged disks. You will learn more about standard SSDs later in this module.

## Premium storage accounts

VHDs are stored in Azure general-purpose storage accounts as page blobs. To use premium storage for your VHDs, you must store VHDs in a **premium storage account**. You choose the performance tier of a storage account when you create it, and you cannot change this setting later.

> [!IMPORTANT]
> Premium storage may not be available in all Azure regions. To check the availability in your region, see [Products available by region](https://azure.microsoft.com/en-us/global-infrastructure/services/).

### Data replication and premium storage accounts

The data in your Microsoft Azure storage account is always replicated to ensure durability and high availability. Azure Storage replication copies your data so that it's protected from planned and unplanned events like transient hardware failures, network or power outages, massive natural disasters, and so on. You can choose to replicate your data within the same data center, across zonal data centers within the same region, and even across regions. There are four types of replication:

- **Locally redundant storage (LRS)** - Azure replicates the data within the same Azure data center. The data remains available if a node fails. However, if an entire data center fails, data may be unavailable.
- **Geo-redundant storage (GRS)** - Azure replicates your data to a secondary region that is hundreds of miles away from the primary region. If your storage account has GRS enabled, then your data is durable even if there's a complete regional outage or a disaster in which the primary region isn't recoverable.
- **Read-access geo-redundant storage (RA-GRS)** - Azure provides read-only access to the data in the secondary location, and geo-replication across two regions. If a data center fails, the data remains readable but can't be modified.
- **Zone-redundant storage (ZRS)** - Azure replicates your data synchronously across three storage clusters in a single region. Each storage cluster is physically separated from the others and resides in its own availability zone (AZ). With this type of replication, you can still access and manage your data in the event that a zone becomes unavailable.

Standard storage accounts support all replication types, but premium storage accounts only support locally redundant storage (LRS). Since VMs themselves run in a single region, this restriction isn't normally an issue for VHD storage.

## Migrating from standard storage to premium storage

It's best to create VHDs in the correct type of storage account in the first place. However, sometimes requirements change, demands increase, or you realize that you chose the wrong type. Consider a move to premium storage in these situations.

Before starting a migration, consider that it will involve some downtime during which the VM will be unavailable to users.

> [!NOTE]
> For full details of the migration from standard to premium storage, see [Migrating to Azure Premium Storage (Unmanaged Disks)](https://docs.microsoft.com/azure/storage/common/storage-migration-to-premium-storage).

When you create a VM in Azure, you need to find the right balance between cost and performance. Premium storage is a good choice for disk-intensive applications such as databases because it supports faster IO than standard storage.
