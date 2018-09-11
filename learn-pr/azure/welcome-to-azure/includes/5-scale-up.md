Your web server is up and running, but you realize you need more computing power to make the experience great for your users. How can you make your VM run faster?

In your data center, you might move your web server to more powerful hardware to solve performance problems. The problem is you need to buy, rack, and power your new system. With Azure, the answer is much simpler.

Now you'll scale up your VM to a more powerful size. Before you change your VM's size, you must shut it down, or deallocate it.

First, let's define what scale means and what happens when you deallocate your VM.

## What is scale?

_Scale_ refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.  

You may have heard the terms _scale up_ and _scale out_.

Scaling up, or vertical scaling, means to increase the memory, storage, or compute power on an existing virtual machine. For example, you can add additional memory to a web or database server to make it run faster.

> [!TIP]
> You can also _scale down_ your system if you needed to scale up only temporarily.

Scaling out, or horizontal scaling, means to additional virtual machines to power your application. For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.

## What is deallocation?

Deallocation is the process that shuts down your VM and releases its compute resources.

When you shut down your PC at work or at home, the operating system closes your programs and then notifies the power management hardware to turn off power.

Deallocating a virtual machine is similar. After your VM shuts down, Azure reclaims the hardware used to power it. Your data disks and storage remain intact. When you start your VM back up, you'll pick up where you left off, just like with your PC.

When deallocated, you are not billed for the compute and network resources that your virtual machine uses. You still pay for any associated disks to sit in storage, but the overall cost is much lower than it would be if the VM were running.

Here, you'll deallocate your VM briefly so that you can resize it. But you can also deallocate your VMs for a longer period to save cost. Say you have a bank of VMs that you use for testing during work hours. You can schedule your VMs to be automatically deallocated on nights and weekends. If you need to stay late, you can manually restart them.

## Scale up your VM

Recall that you specified the size **Standard_DS2_v2** when you created your VM. Your VM currently has two virtual CPUs and 7 GB of memory.

Let's bump up to the next size, **Standard_DS3_v2**. Your VM will then have four virtual CPUs and 14 GB of memory.

::: zone pivot="windows-cloud"

1. From Cloud Shell, deallocate your VM.

    ```powershell
    Stop-AzureRmVM `
      -ResourceGroupName "myResourceGroup" `
      -Name "myVM" `
      -Force
    ```
    The process takes a couple minutes to complete.
1. Run these commands to specify your VM's new size.
    ```powershell
    $vm = Get-AzureRmVM `
      -ResourceGroupName "myResourceGroup" `
      -VMName "myVM"
    $vm.HardwareProfile.VmSize = "Standard_DS3_v2"
    Update-AzureRmVM `
      -VM $vm `
      -ResourceGroupName "myResourceGroup"
    ```
    The update process takes about a minute.
1. Restart your VM.

    ```powershell
    Start-AzureRmVM `
      -ResourceGroupName "myResourceGroup" `
      -Name "myVM"
    ```
    The process takes about a minute.
1. Verify that your VM is running the new size.

    ```powershell
    $vm = Get-AzureRmVM `
      -ResourceGroupName "myResourceGroup" `
      -VMName "myVM"
    $vm.HardwareProfile
    ```
    You see your new VM size, **Standard_DS3_v2**.
    ```console
    VmSize
    ------
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. From Cloud Shell, run `az vm deallocate` to deallocate, or stop, your VM.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```
    The process takes a couple of minutes to complete.
1. Run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.

    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
    The update process takes about a minute.
1. Run `az vm start` to restart your VM.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```
    The process takes about a minute.
1. Run `az vm show` to verify that your VM is running the new size.

    ```azurecli
    az vm show -n myVM -g myResourceGroup --query "hardwareProfile" -o tsv
    ```
    You see your new VM size, **Standard_DS3_v2**.
    ```console
    Standard_DS3_v2
    ```

::: zone-end

> [!NOTE]
> By default, Azure assigns a new public IP address to your VM when you stop and restart it. This is OK for learning purposes. In practice, you can reserve a public IP address that stays with your VM even when your VM is restarted.

## Summary

Nice job! With just a few commands, your VM is now twice as powerful.

Scaling up and scaling out are two ways to increase performance. Here you scaled up your VM to increase its compute power.

You deallocate a VM before you can resize it. You can also deallocate your VMs when they're not in use to save costs.