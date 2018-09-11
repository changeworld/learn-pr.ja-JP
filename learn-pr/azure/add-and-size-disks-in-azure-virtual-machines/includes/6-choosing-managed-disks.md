Applications and services often need to scale with demand. For virtual machines, this means scaling out, or creating additional instances. Creating disks for new instances manually can place a burden on Azure administrators. Instead, we can use **managed disks** to reduce the administration required to create new disks.

Managed disks handle storage for you "behind the scenes". You specify the disk type and the size of disk you need, and Azure creates and manages the disk for you. 

When you're using managed disks, another disk type is available for you to use called **standard SSD**.

## Standard SSDs

If you recall, with unmanaged disks, you can choose from two types of disk: standard hard disk drives (HDDs) and premium solid-state drives (SSDs).

When you use managed disks, you can choose a third type of disk: standard SSDs. This disk type sits between standard HDDs and premium SSDs from a performance and cost perspective.

You can use standard SSDs with any VM size, including VM sizes that don't support premium storage. In fact, using standard SSDs is the only way to use SSDs with those VMs.

## Managed disks in availability sets

Sometimes you use several VMs to host your application to handle increased load.

You can put your VMs into an **availability set** to reduce the chances of customers experiencing an outage. Using an availability set, your VMs are spread across fault domains. A fault domain is a logical group of underlying hardware that shares a common power source and network switch, similar to a rack within an on-premises data center. As you create VMs within an availability set, the Azure platform automatically distributes your VMs across these fault domains. This separation in the Azure data center guarantees that there's no single point of failure for all the VMs in the availability set.

When you use managed disks, Azure automatically makes sure that each VHD is placed into the same fault domain as its VM. This guarantee removes the possibility that the storage account configuration may include a single point of failure. There's no way to guarantee this separation when you use unmanaged disks.

## Choosing managed disks

**Do you want to control storage accounts in your subscription?**

Managed disks are easy to administer, but you may feel that you can optimize storage or manage costs better by keeping control of storage accounts.

**Do you need to use standard SSDs?** 

If you want to use SSDs with a VM size that does not support premium storage, you have to choose managed disks.

**Are the VMs in an availability set?** 

It is highly recommended to use managed disks to optimize resilience if your VMs are members of an availability set.

## Migrating to managed disks

You can convert your unmanaged disks to managed disks at any time using one of these two methods:

- Using the Azure portal. On the Disks page of your virtual machine's settings, you'll find the **Migrate to managed disks** tool. This process will stop the VM, migrate the disks, and then restart the VM for you. You'll experience an interruption in service from the VM while the conversion takes place.
- Using Azure PowerShell. You can use the `ConvertTo-AzureRmVMManagedDisk` cmdlet to perform the migration. If you choose this method, you must stop and start the VM yourself.

Managed disks reduce the administrative workload and have some features, such as standard SSDs, that aren't available for unmanaged disks.
