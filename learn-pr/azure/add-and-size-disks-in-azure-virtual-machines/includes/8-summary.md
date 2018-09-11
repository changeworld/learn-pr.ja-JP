It's essential to understand VHDs and their different configurations if you want to optimize how your VMs run in Azure.

Here's a summary of what we learned in this module.

[!INCLUDE [summary table](./summary-table.md)]

## Further reading

- [About disks  storage for Azure Windows VMs](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Azure Managed Disks Overview](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)
- [Azure Storage replication](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

## Cleanup
<!---TODO: Update for sandbox?--->

Remove unneeded VMs to avoid unnecessary charges in your Azure subscription. The easiest way to clean up your Azure subscription is to remove the resource group we used in this module. Removing the resource group deletes all resources in that group, including the VMs. When you're finished with this module, follow these steps:

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

1. In the navigation on the left, select **Resource groups**.

1. Select the **MailInfrastructure** resource group.

1. On the **MailInfrastructure** pane, click **Delete resource group**.

1. In the **TYPE THE RESOURCE GROUP NAME** textbox, type **MailInfrastructure** and then select **Delete**. Azure removes the resource group and all the resources it contains. The process may take several minutes to complete.
