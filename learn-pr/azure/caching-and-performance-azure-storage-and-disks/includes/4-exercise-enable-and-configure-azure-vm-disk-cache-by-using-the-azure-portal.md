Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications. You want to make the following adjustments:

- You need to change the disk cache settings on a VM.
- You want to add a new data disk to the VM with caching enabled.

You've decided to make these changes through the Azure portal.

In this exercise, we'll walk through making the changes to a VM that we described above. First, let's sign in to the portal and create a VM.

## Sign in to the Azure portal
<!---TODO: Update for sandbox?--->

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

## Create a virtual machine

In this step, we're going to create a VM with the following properties:

|Property  |Value  |
|---------|---------|
|Image     |   **Windows Server 2016 Datacenter**      |
|Name     |   **fotoshareVM**     |
|Resource group     |   **fotoshare-rg**      |


1. In the left menu of the portal, select **Virtual machines**.

1. Now select **+ Add** in the top left of the **Virtual machines** screen. This action starts the creation process.

1. On the **Compute** panel that lists available VM images, type *Windows Server 2016 Datacenter* into the search box.

1. Select **Windows Server 2016 Datacenter** from the search results, and then select **Create** to start the VM creation process.

1. In the **Basics** panel, in the **Name** box, enter **fotoshareVM**.

1. In the **Username** and **Password**  boxes, enter a name and password for an administrator account on this server.

1. In the **Subscription** drop-down list, select your Azure subscription.

1. Under **Resource Group**, select **Create new**, and in the box, type **fotoshare-rg**.

1. In the **Location** drop-down list, select a region near you.

    The following is an example of what the **Basics** configuration looks like when filled out:

    ![Screenshot of VM Basics config filled out.](../media-draft/vm-basics-settings.PNG)

1. Select **OK** to move on to the next step.

You now need to choose a size for the VM, and then start the deployment:

> [!IMPORTANT]
> Remember that disk caching can't be changed for L-Series and B-series virtual machines. We'll select a different size.

1. In the **Choose a size** section, select a **Standard** SKU, such as **F1s**, and then choose **Select**.

1. Scroll down the **Settings** pane and observe that there is no option to configure disk caching. Let's accept the defaults on this step and choose **OK** at the bottom of the pane.

1. On the **Create** panel, review the summary, and then choose **Create**.

1. VM creation can take a while. Wait until the VM has deployed before continuing with the exercise. You'll get a message in the notification hub when the process is complete.

> [!IMPORTANT]
> We'll use this VM in the next lesson, so keep it around for a while!

## View OS disk cache status in the portal

Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps:

1. In the left menu, click **All resources**, and then select your VM,  **fotoshareVM**.

1. On the **Virtual machine** screen, under **SETTINGS**, select **Disks**.

1. On the **Disks** pane, the VM has one disk, the OS disk. Its cache type is currently set to the default value of **Read/write**.

![Screenshot of our OS and data disks, both set to Read-only caching.](../media-draft/os-disk-rw.PNG)

## Change the cache settings of the OS disk in the portal

1. On the **Disks** pane, select **Edit** in the upper left of the screen.

1. Change the **HOST CACHING** value for the OS disk to **Read-only** using the drop-down list, and then select **Save** in the upper left of the screen.

1. This update takes a while. The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk. If it's the operating system disk, the VM is also restarted. You'll get a message similar to the following notification when the update has finished:

    ![Example of notification you receive when the cache setting update has completed.](../media-draft/vm-disk-update-complete.PNG)

4. Once complete, the OS disk cache type is set to **Read-only**.

Let's move on to data disk cache configuration. To configure a disk, we'll need to first create one.

## Add a data disk to the VM and set caching type

1. Back on the **Disks** view of our VM in the portal, go ahead and select **Add data disk**. An error immediately appears in the **Name** field, telling us that the field can't be empty. We don't have a data disk yet, so let's create one.

1. Click in the **Name** list, and then click **Create disk**.

1. In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.

1. Under **Resource Group**, select **Use existing**, and select **fotoshare-rg** from the drop-down menu.

1. Select **Create** at the bottom of the screen.

1. Wait until the disk has been created before continuing.

1. Change the **HOST CACHING** value for our new data disk to **Read-only** using the drop-down list, and then select **Save** in the upper left of the screen.

1. Wait for the VM to update. Updating takes a while because Azure detaches and reattaches the data disk to change this setting.

1. Once complete, the data disk cache type is set to **Read-only**.

In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and configure caching on a new data disk. The following screenshot shows the final configuration: 

![Screenshot of our OS and data disks, both set to Read-only caching.](../media-draft/disks-final-config-portal.PNG)