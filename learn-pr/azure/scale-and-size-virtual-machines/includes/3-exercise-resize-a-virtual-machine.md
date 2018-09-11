In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.

## Create a VM

1. In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.

1. In the Azure portal, create a new resource. In the **New** blade, type **virtual machine** in the search box, and then press ENTER.

1. In the **Everything** blade, under **Results**, click **Windows Server 2016 Datacenter**.

1. In the **Windows Server 2016 Datacenter** blade, click **Create**.

1. In the **Basics** blade, complete the details using the following information, and then click **OK**.

    |Setting|Value|
    |---|---|
    |Name|DB01|
    |Username|LocalAdmin|
    |Password and Confirm password|Adm1nPa$$word|
    |Resource group|ExerciseRG|
    |Location|Central US|

1. On the **Choose a size** blade, select **D2s_v3**, and then click **Select**.

1. On the **Settings** blade, under **Select public inbound ports** select **HTTP**, **HTTPS**, and **RDP (3389)**, under **Boot diagnostics** click **Disabled**, leave all other settings at the default value, and then click **OK**.

1. On the **Create** blade, click **Create**.

1. Wait until the deployment is complete before continuing the exercise.

## Resize using the portal

1. In the Azure portal, browse to the ExerciseRG resource group, and in the **ExerciseRG** blade, click the **DB01** virtual machine object.

1. On the **DB01** blade, click **Size**. Note the currently highlighted size is the size you selected when creating the virtual machine.

1. On the **Choose a size** blade, try to find the **F2s_v2** size - it should not be available in the list of sizes because the VM is currently running and that size is from a different family. Close the **Choose a size** blade.

1. In the **DB01** blade, click **Stop**. In the **Stop this virtual machine** dialog box, click **Yes**, and wait for the virtual machine status to show **Stopped (deallocated)**.

1. On the **DB01** blade, click **Size**. On the **Choose a size** blade, select **F2s_v2** and then click **Select**. Notice the notification about resizing the virtual machine.

1. On the **DB01** blade, click **Overview**, then click **Start**.

## Resize using PowerShell

1. In the Azure portal, open the Cloud Shell.

1. Use the following cmdlet to get the list of available virtual machine sizes.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. Use the following cmdlet to resize the virtual machine to an F4s_v2 size.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. Click the Refresh button in the DB01 blade while you are waiting for the PowerShell command to complete - you should notice that the virtual machine is restarting to accommodate the change in size.

In this exercise, you created a virtual machine and resized it with two different tools. A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.