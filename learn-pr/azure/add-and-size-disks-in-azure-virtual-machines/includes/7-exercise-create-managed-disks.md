In this short exercise, we'll learn how to migrate an existing, unmanaged VHD to a managed VHD. 

## Sign in to Azure
<!---TODO: Update for sandbox?--->

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

## Migrate our disks to managed disks

1. In the Azure portal, in the navigation on the left, select **Virtual machines**.

1. In the list of virtual machines, select our virtual machine,  **MailSenderVM**.

1. In the **MailSenderVM** pane, under **SETTINGS**, select **Disks**.

1. In the **MailSenderVM - Disks** page, select **Migrate to managed disks**.

1. In the **Migrate to managed disks** page, select **Migrate**. Azure stops the VM, migrates the disks, and then restarts the VM. This process may take several minutes.

We migrated our disks to managed disks in this exercise. By using managed disks, you don't have to configure storage accounts for those disks because Azure manages them for you.
