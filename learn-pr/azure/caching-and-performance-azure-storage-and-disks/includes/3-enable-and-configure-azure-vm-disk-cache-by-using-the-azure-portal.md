You can configure virtual machine disk cache settings with any of the following tools:

- Azure portal
- Resource Manager templates
- Azure CLI
- Azure PowerShell

In the next exercise, we're going to use the portal to create a VM and configure caching on its disks. Here's some information to keep in mind. 

When you provision a new VM using the Azure portal, you can't change the default caching configuration for the OS disk from read/write until the VM is deployed.

When you add a data disk to an existing VM, you can configure the cache option before the disk is deployed to the VM.

Changing the cache setting of an Azure disk detaches and reattaches the target disk. If it's the operating system disk, the VM is restarted. Stop all applications/services that might be affected by this disruption before changing the disk cache setting.

Let's create a VM and change the cache settings using the Azure portal.
