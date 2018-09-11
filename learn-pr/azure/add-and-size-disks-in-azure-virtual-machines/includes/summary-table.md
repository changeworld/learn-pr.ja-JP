
The following table shows what disks are available as managed, and unmanaged disks.

|Disk type  |Unmanaged  | Managed  | Storage account for unmanaged|
|---------|---------|---------|---------|
|[Standard HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   yes      |  yes       |  general storage account       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   no      |   yes      |  Not applicable       |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    yes     |   yes      |     premium storage account    |

Here's a summary of the tradeoffs you make when selecting a disk type for your VMs.

|Disk type  |Relative IOPS*  | Relative cost  | Scenarios|
|---------|---------|---------|---------|
|[Standard HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   lowest      |  lowest       |  Use for low-IOPs workloads that demand consistent performance and cost-effective development and testing       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   medium|   medium      |  Use for low-IOPs workloads that demand consistent performance and cost-effective development and testing |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    highest     |   highest      |     Use for production and performance-sensitive workloads where low latency and high throughput are critical    |

* IOPS - Input/output operations per second.