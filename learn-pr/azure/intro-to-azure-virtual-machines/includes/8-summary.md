In this module, you looked at the decisions you need to make before creating a virtual machine. These decisions include aspects such as the VM size, types of disks used, operating system image selected, and the types of resources created.

You also looked at the options to create and manage virtual machines in Azure. You saw how easy it is to create and manage VMs using the portal and when to use Resource Manager templates, PowerShell, the Azure CLI, and the Azure Client SDK.

Finally, you looked at the extensions and services available to more easily administer your VMs.

## Clean up
<!---TODO: Update for sandbox?--->

Remember that virtual machines continue to generate a monthly charge as long as you have hardware reserved (even if the OS is shut down!) and space used for disks. You can stop the VM in the portal to stop billing for compute services, but the storage will continue to generate a bill. If you create resources for testing purposes, an easy way to get rid of them all is to delete the **resource group** they are part of.

> [!TIP]
> Make sure to place all your test resources into the same resource group for easy management.

To delete a resource group, locate it through the **Resource Groups** panel (click on **Resource Groups** in the sidebar). Then click the `...` ellipse menu next to the resource group, and select **Delete resource group** from the pop-up menu as shown here.

![Delete a resource group](../media-draft/7-delete-rgs.png)
