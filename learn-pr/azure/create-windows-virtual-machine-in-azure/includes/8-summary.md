In this module, you learned how to create a Windows VM using the Azure portal. You then connected to the public IP address of the VM and managed it over RDP. You discovered how RDP in Azure provides a similar experience to logging on interactively to a physical computer.

You learned that while RDP allows us to interact with the operating system and software of the virtual machine, the portal allows us to configure the virtual hardware and connectivity. We also could have used PowerShell or the Azure CLI, if a command-line or scriptable environment were preferred.

## Clean up
<!---TODO: Update for sandbox?--->

You are charged for VMs while they run, and for the storage based on how much you use. Always stop and deallocate VMs when you aren't using them, and when you no longer need the resources it's a good idea to delete them. To remove all the resources that you created, you can delete them one-by-one, or just delete the resource group.

1. Sign in to the Azure portal.

1. On the left menu, select **All Services**.

1. Select **Resource Groups**.

1. Find the resource group that you created in the first exercise. Click the ellipsis (...) on the right side of the list view.

1. Select **Delete resource group**.

1. On the next screen, enter the resource group name to confirm the deletion.

1. Click **Delete**.
