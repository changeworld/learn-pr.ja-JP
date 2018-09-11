If your virtual machine (VM) hosts a disk-intensive application, you should consider using premium storage for the virtual hard drives (VHDs).

For example, you decide to add a VHD to store outgoing mail on your SMTP server VM. To optimize disk performance, you decide on premium storage for outgoing mail. 

Let's add a premium SSD to the VM. 

## Sign in to Azure
<!---TODO: Update for sandbox?--->

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

## Create a premium storage account

All premium storage VHDs must be stored in a premium storage account. Follow these steps to create a premium storage account.

1. Select **Storage accounts** under **FAVORITES** in the left-hand menu of the portal.

1. Select **+ Add** at the top left of the **Storage accounts** screen.

1. In the **Create storage account** pane that opens, set the following properties.

|Property  |Value  |Notes  |
|---------|---------|---------|
|Name     |    *A unique name (see note)*     |   This name must be unique across all existing storage account names in Azure. It must be 3 to 24 characters long, and can contain only lowercase letters and numbers.      |
|Account kind     |  **Storage (general-purpose v1)**       |         |
|Location     |  *Select the same location as the VM you created earlier*       |         |
|Replication     |   **Locally redundant storage (LRS)**      |  Select this value from the dropdown. If you recall, we're creating a premium storage account, and premium storage supports only LRS replication.       |
|Performance     |  **Premium**       | Premium storage accounts are backed by solid-state drives and offer consistent, low-latency performance.        |
|Resource group     |  *Select **Use existing** and then  **MailInfrastructure***      |  We want to keep all resources together under the same resource group.       |

When you've filled out this dialog, it should look like the following screenshot. 

!["Create storage account" dialog showing all properties set as instructed.](../media-draft/create-premium-sa.png)

1. Select **Create** to start the storage account creation process. This process can take a few moments to complete. 

1. When you receive a notification that deployment of the new storage account finished successfully, select **Refresh** in the storage accounts list to display the premium storage account we created. Note the name of this account, as it will be used in the next step.

## Create VHD in the premium storage account

Now you can add a new VHD to the VM and specify the premium storage account as its location. Follow these steps:

1. In the navigation on the left, under **FAVORITES**, select **Virtual machines**.

1. In the list of VMs, select **MailSenderVM**.

1. Under **SETTINGS** of the **MailSenderVM** configuration menu on the left, select **Disks**.

1. Under **Data disks**, select **Add data disk**.

1. In the **Attach unmanaged disks** pane, set the following properties.


|Property  |Value  |Notes  |
|---------|---------|---------|
|Name     |   **MailSenderVMOutgoing**      |         |
|Source type     |  **New (empty disk)**       |   Select this value from the dropdown.       |
|Account type     |  **Premium SSD**       |  Select this value from the dropdown.        |

1. To the left of the **Storage container** field, select **Browse**.

1. In the list of storage accounts, find and select the premium storage account you created earlier in this unit. The type of the entry will be listed as **Premium-LRS**.

1. In the list of containers, select __+ Container__.

1. In the **New container** pane, in the **Name** textbox, type **vhds** and then select **OK**.

1. In the list of containers, select **vhds** and then choose **Select**.

1. Back on the **Attach unmanaged disk** pane, select **OK**.

1. Back on the **MailSenderVM - Disks** pane, select **Save**. Azure adds the new premium storage disk to the VM.

Our virtual machine has an operating system disk, a standard disk, and a premium SSD-based disk.

> [!NOTE]
> The new disk must be initialized, partitioned, and formatted before it can store data. To avoid repetition, these steps have been omitted from this exercise. If you want to complete these tasks, complete the steps in the [Partition and format a data disk](../3-exercise-add-data-disks-to-azure-virtual-machines.yml##partition-and-format-a-data-disk) section of the preceding exercise.
