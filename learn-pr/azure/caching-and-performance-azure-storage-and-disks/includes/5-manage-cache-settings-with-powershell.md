Creating administration scripts is a powerful way to optimize your work flow. You can automate common, repetitive tasks. Once a script has been verified, it will run consistently, which will likely reduce errors. In the previous exercise, we created a VM, added a data disk, and changed cache settings, all through the Azure portal. What if we needed to repeat these tasks across many VMs, in many regions? Let's use Azure PowerShell!

## What is Azure PowerShell?
Azure PowerShell is a module that you add to Windows PowerShell to let you connect to your Azure subscription and manage resources. Azure PowerShell requires Windows PowerShell to function. Windows PowerShell provides services like the shell window, command parsing, and so on. Azure PowerShell adds the Azure-specific commands.

For example, Azure PowerShell provides the **New-AzureRmVM** command that creates a virtual machine for you inside your Azure subscription. To use it, you would launch the PowerShell application and then issue a command like this example:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell is available two ways: inside a browser via the Azure Cloud Shell or with a local install on Linux, Mac, or Windows.

## What are PowerShell cmdlets?

A PowerShell command is called a **cmdlet** (pronounced "command-let"). A cmdlet is a command that manipulates a single feature. The term **cmdlet** is intended to imply "small command." By convention, cmdlet authors are encouraged to keep cmdlets simple and single-purpose.

The base PowerShell product ships with cmdlets that work with features such as sessions and background jobs. You add modules to your PowerShell installation to get cmdlets that manipulate other features. For example, there are third-party modules to work with ftp, administer your operating system, access the file system, and so on.

Cmdlets follow a verb-noun naming convention; for example, **Get-Process**, **Format-Table**, and **Start-Service**. There's also a convention for verb choice: "get" to retrieve data, "set" to insert or update data, "format" to format data, "out" to direct output to a destination, and so on.

Cmdlet authors are encouraged to include a help file for each cmdlet. The **Get-Help** cmdlet displays the help file for any cmdlet:

```
Get-Help <cmdlet-name> -detailed
```
## What is AzureRM?

**AzureRM** is the formal name for the Azure PowerShell module that has a set of cmdlets to work with Azure features (the **RM** in the name stands for **Resource Manager**). It contains hundreds of cmdlets that let you control nearly every aspect of every Azure resource. You can work with resource groups, storage, virtual machines, Azure Active Directory, containers, machine learning, and so on.

## PowerShell cmdlets for managing Azure disk caching

Azure PowerShell has specific cmdlets to help manage VMs and disks. 

The following table lists some of the cmdlets we'll use in the next exercise:

|Command  |Description  |
|---------|---------|
|`Get-AzureRmVM`     |  Gets the properties of a virtual machine.       | 
|`Update-AzureRmVM`     |  Updates the state of an Azure virtual machine.       |        
|`New-AzureRmDiskConfig`     |  Creates a configurable disk object.       |        
|`Add-AzureRmVMDataDisk`     |  Adds a data disk to a virtual machine.   |      


Let's use these cmdlets in the next exercise to manage caching on our VM.
