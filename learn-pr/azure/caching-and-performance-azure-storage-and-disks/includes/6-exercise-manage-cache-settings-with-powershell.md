
In the previous exercise, we performed the following tasks using the Azure portal:

- View OS disk cache status
- Change the cache settings of the OS disk
- Add a data disk to the VM
- Change caching type on a new data disk

Let's practice these operations using Azure PowerShell. We're going to use the VM we created in the previous exercise. The operations in this lab assume:

- Our VM exists and is called **fotoshareVM**
- Our VM lives in a resource group called **fotoshare-rg**

If you've gone with a different set of names, just replace these values with yours. 

Here's the current state of our VM disks from the last exercise:

![Screenshot of our OS and data disks, both set to Read-only caching.](../media-draft/disks-final-config-portal.PNG)

We used the portal to set the **HOST CACHING** field for both the OS and data disks. Keep this initial state in mind as we work through the following steps. 

### Set up some variables
First, let's  store some resource names so we can use them later.

Use the Azure Cloud Shell terminal on the right to run the following PowerShell commands:

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session. 

### Get info about our VM

Run the following command to get back the properties of our VM:
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
We store the response in our `$myVM` variable. We can run the following command to just show us the properties we specify here:

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

As the following screenshot shows, this VM is indeed the VM we're after. So, let's move on. 

![PowerShell console showing results of last 4 commands that we ran.](../media-draft/ps-commands-1.PNG)

### View OS disk cache status

We can check the caching  setting through  the `StorageProfile` object, as follows:

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
In this example, the current value is `None`. Let's change it back to the default for an OS disk.

![PowerShell console showing our OS disk having a caching value of "None".](../media-draft/ps-oscaching-none.PNG)

### Change the cache settings of the OS disk

We can set the value for the cache type using the same `StorageProfile` object, as follows:
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

This command runs fast, which should tell you it's doing something locally. The command just changes the property on the `myVM` object. As the following screenshot shows, if you refresh the `$myVM` variable,  the caching value won't have changed on the VM:

![PowerShell console showing that refreshing our "myVM" object resets the caching to "none" because we didn't actually update the VM.](../media-draft/ps-commands-2.PNG)

To  make the change on the VM itself, call `Update-AzureRmVM`, as follows:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Notice that this call takes a while to complete. That's because we're updating the actual VM, and Azure restarts the VM  to make the change.

![PowerShell console showing our OS disk having a caching value of "None".](../media-draft/ps-oscaching-rw.PNG)

If you refresh the `$myVM` variable again, you'll see the change on the object. Looking at the disk in the portal, you'd also see the change there. Let's move on to creating a new data disk.  

### List data disk info

To see what data disks we have on our VM, run the following command: 

```powershell
$myVM.StorageProfile.DataDisks
```

We have only one data disk at the moment. The `Lun` field is important. It's the unique **L**ogical **U**nit **N**umber. When we add another data disk, we'll give it a unique `Lun` value. 

### Add a new data disk to our VM 

For convenience, we'll store our new disk name:

```powershell
$newDiskName = "fotoshareVM-data2"
```

Run the following `Add-AzureRmVMDataDisk` command to define a new disk:

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

We've given this disk a `Lun` value of `1` because it's not taken. We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change: 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Let's look at our data disk info again:

```powershell
$myVM.StorageProfile.DataDisks
```

![PowerShell console showing our two data disks.](../media-draft/2-data-disks-part1.png)

We now have two disks. Our new disk has a `Lun` of `1` and the default value for `Caching` is `None`. Let's change that value.

### Change cache settings of new data disk

We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet, as follows:

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

As always, commit the changes with `Update-AzureRmVM`:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Here's a view from the portal of what we've accomplished in this exercise. Our VM now has two data disks, and we've adjusted all **HOST CACHING** settings. We did all of that with just a few commands. That's the power of Azure PowerShell.

![Azure portal showing our two data disks.](../media-draft/disks-final-config-portal2.png)
