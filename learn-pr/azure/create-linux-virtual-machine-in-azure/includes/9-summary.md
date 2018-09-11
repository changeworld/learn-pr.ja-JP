In this module, you learned how to create a Linux VM using the Azure portal. You then connected to the public IP address of the VM and managed it with an SSH connection. 

You learned that while SSH allows us to interact with the operating system and software of the virtual machine, the portal will enable us to configure the virtual hardware and connectivity. We also could have used PowerShell or the Azure CLI, if a command-line or scriptable environment were preferred.

## Clean up
<!---TODO: Update for sandbox?--->

You are charged for VMs while they run and for the storage based on how much you use. Always stop and deallocate VMs when you aren't using them, and when you no longer need the resources, it's a good idea to delete them. To remove all the resources that you created, you can delete them one by one or delete the resource group:

1. Sign in to the Azure portal.

1. On the left menu, select **All Services**.

1. Select **Resource Groups**.

1. Find the resource group that you created in the first exercise. Click the ellipsis (...) on the right side of the list view.

1. Select **Delete resource group**.

1. On the next screen, enter the resource group name to confirm the deletion.

1. Click **Delete**.
