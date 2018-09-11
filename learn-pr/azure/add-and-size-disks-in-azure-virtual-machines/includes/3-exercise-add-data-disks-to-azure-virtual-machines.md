In this exercise, let's assume your company runs a Simple Mail Transfer Protocol (SMTP) email server. You want to migrate this server into Azure. You want the SMTP server to store incoming messages for your own domain in a folder called "Drop" on a dedicated VHD.

The goal of the exercise is to create a Windows virtual machine (VM) and attach a new virtual hard disk (VHD) called "Incoming" to store the "Drop" directory.

## Sign in to Azure
<!---TODO: Need update for sanbox?--->

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

## Create a Windows VM in the Azure portal

To create a VM to host the SMTP server with its data drives, follow these steps:

1. Choose **Create a resource** in the upper left corner of the Azure portal.

1. In the search box above the list of Azure Marketplace resources, search for and select **Windows Server 2016 Datacenter**, and then choose **Create**.

1. In the **Basics** pane that opens to the right, enter the following property values. 


|Property  |Value  |Notes  |
|---------|---------|---------|
|Name     |   **MailSenderVM**      |         |
|VM disk type     |  **Standard HDD**       |   Select this value from the dropdown.      |
|User name     |  **mailmaster**       |         |
|Password     |  The password must be at least 12 characters long and meet the [defined complexity requirements](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).       | Make sure to remember this user name and password because we'll use them throughout the module.         |
|Subscription     |  Choose your subscription.       |  Select this value from the dropdown.       |
|Resource group     |  Select **Create new**, and then type **MailInfrastructure**.       |  We'll gather all resource used in this module into one resource group.       |
|Location     |   A location near you.      | Select this value from the dropdown.        |

4. Select **OK** at the bottom of the **Basics** page to continue to the **Size** configuration pane.

1. In the **Size** configuration pane, search for and select **B1ms**, and then click **Select**.

1. In the **Settings** pane, under **Use managed disks**, click **No**. We'll discuss managed disks later in this module.

1. In the **Select public inbound ports** dropdown list, select **RDP (3389)**. We'll use this port to remote into our VM after it's created.

1. Leave all the other settings at their default, and then click **OK**.

1. In the **Create** pane, review the configuration.

1. When you have reviewed the configuration,  select **Create**. Azure creates and starts the new VM.

> [!TIP]
> Creating your VM and deploying it in Azure can take a few minutes. You can watch the progress in the **Notifications** hub. Azure will display a notification dialog when it finishes.

## Add an empty data disk to our VM

We're going to name the disk stores the "Drop" directory for your SMTP server "Incoming". Let's add a new empty disk to the server using the following steps:

1. In the navigation on the left, under **FAVORITES**, select **Virtual machines**.

1. In the list of VMs, select **MailSenderVM**.

1. Under **SETTINGS** of the **MailSenderVM** configuration menu on the left, select **Disks**.

1. Under **Data disks**, select **Add data disk**.

1. In the **Attach unmanaged disks** pane, set the following properties.


|Property  |Value  |Notes  |
|---------|---------|---------|
|Name     |   **MailSenderVMIncoming**      |         |
|Source type     |  **New (empty disk)**       |   Select this value from the dropdown.       |
|Account type     |  **Standard HDD**       |  Select this value from the dropdown.        |


6. To the left of the **Storage container** field, select **Browse**.

1. In the list of storage accounts, search for the storage account whose name begins with **mailinfrastructure** and select it.

1. In the list of containers, click **vhds** and then choose **Select**.

1. Back on the **Attach unmanaged disk** screen, select **OK**.

1. Back on the **MailSenderVM - Disks** screen, select **Save**.

We've now defined a disk called **MainSenderVMIncoming**. To use the disk, we'll first need to partition and format it when we log into the VM. 

## Partition and format a data disk

As with physical disks, initiate and format a partition on a VHD before it can be used.

### Log into our Windows VM using RDP

1. In the **MailSenderVM** virtual machine main screen, select **Overview**.

1. Select **Connect** from the top left of the overview screen.

1. In the **Connect to virtual machine** dialog that opens on the right, select **Download to RDP File**.

   ![Screenshot of the "Connect to virtual machine" dialog with the "Download RDP file" button highlighted.](../media-draft/download-rdp.png)

4. A file called **MailSenderVM.rdp** is downloaded to your local `Downloads` folder. This file is the remote desktop configuration file for the MailSenderVM virtual machine. Open the file to start the connection process.

1. In the **Remote Desktop Connection** dialog, click **Connect**.

1. In the **Windows Security** dialog, click **Use another account**.

1. In the **Username** textbox, type **mailmaster**.

1. In the **Password** textbox, type the password you entered for this user name in this exercise. 

1. In the **Remote Desktop Connection** dialog, click **Yes**.

A remote desktop session to the virtual machine is now started. It might take a few moments to sign in for the first time. When sign-in is finished, the **Server Manager** tool will be displayed on the screen.

### Partition and format our data disk using Server Manager

1. When **Server Manager** is displayed, select **File and Storage Services** in the navigation on the left.

1. Under **Volumes**, select **Disks**.

1. In the list of disks, disk **0** is the operating system disk and disk **1** is the temporary disk. Select disk **2**, which is the new VHD you just added.

1. At the top of the **VOLUMES** pane, select **TASKS** followed by **New Volume...**. The menu is in the top right of the screen as follows.

   ![Screenshot of "TASKS" menu expanded to reveal three menu items. They are "New Volume...", "Rescan Storage", and "Refresh".](../media-draft/tasks-menu.png)


1. In the **New Volume** wizard, click **Next**.

1. In the **Select server and disk** page, select **MailSenderVM** and **Disk 2**, and then click **Next**.

1. In the **Offline or Uninitiated Disk** dialog, click **OK**.

1. In the **Specify the size of the volume** page, click **Next**.

1. In the **Assign a drive letter** page, select **F:** followed by **Next**.

1. In the **Select file system settings** page, in the **Volume label** textbox, type **Incoming** and then select **Next**.

1. In the **Confirm selections** page, select **Create**. Windows initiates the disk and formats the new partition.

1. In the **Completion** page, select **Close**.

Let's have a look at our new disk volume in File Explorer. 
1. Open **File Explorer**.

1. In the navigation in the left, click **This PC** and then double-click **Incoming (F:)**.

1. Select **Home**, and then **New Folder**.

1. Type **Drop** and then press Enter.

We now have a new volume created on our VHD called **Incoming**, and we've created a folder called **Drop** on that volume.  

1. Close Windows Explorer.

1. On the **Task Bar**, click the **Start** button, click the **Power** button, and then click **Disconnect**.

Congratulations! You've successfully created a Windows VM, attached a new VHD, created a volume on that VHD and added a folder to that volume. If you recall, the disk type we used for the new VHD was a **Standard HDD**. In the next unit, we'll learn the differences between standard and premium storage. 
