Suppose you are developing a financial management application for new business startups. You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application. As part of your compliance requirements, you also need to be responsible for your own encryption key management.

In this unit, you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.

> [!IMPORTANT] 
> This exercise assumes that Azure PowerShell is installed on your computer. Go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.

## Prepare the environment

We'll start by deploying a Windows VM to a new resource group, and then add a data disk to the VM.

### Deploy Windows VM using the Azure portal

Here you'll use the Azure portal to create and deploy a Windows VM. Start by defining the basic VM information:

1. In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.

1. In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.

1. On the Compute blade, in the **Recommended** section, click **Windows Server**.

1. In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.

1. In the **Windows Server 2016 Datacenter** blade, click **Create**.

1. In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**

1. In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.

1. In the **Subscription** box, select your Azure subscription.

1. Under **Resource Group**, select **Create new**. In the box, type **moneyapprg**.

1. In the **Location** drop-down list, select a region near you.

1. Click **OK**.

### Choose a size for the VM, and start the deployment

> [!IMPORTANT]
> Remember that basic tier VMs do not support ADE.

1. On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**. Then click **Select**.

1. On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**. Then scroll down and click **OK**.

1. On the **Create** blade, click **Create**.

1. Wait until the VM has deployed before continuing with the exercise.

### Add a data disk to the VM

1. In the left menu, click **All resources**, and then click **moneyappsvr01**.

1. On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.

1. On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.

1. Click in the **Name** list, and then click **Create disk**.

1. In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.

1. Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.

1. Click **Create**.

1. Wait until the disk has been created before continuing.

1. On the **Disks** blade, click **Save**. Note that the data disk encryption status is currently **Not enabled**.

## Configure disk encryption prerequisites

You'll now use the Azure Disk Encryption prerequisites configuration script to configure all the disk encryption prerequisites. This script will create and prepare a key vault in the same region as your VM.

### Prepare the Azure Disk Encryption prerequisite setup script

1. Go to the [Azure Disk Encryption prerequisite setup script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1) GitHub page.

1. On the GibHub page, click **Raw**.

1. Use Ctrl-A to select all the text on the page, and then use Ctrl-C to copy all the text on the page to the clipboard.

1. On your computer, click **Start**, and then browse to **Windows PowerShell ISE**.

1. Right-click **Windows PowerShell ISE**, and click **Run as administrator**.

1. In the Administrator: Windows PowerShell ISE window, click **View**, and then click **Show Script Pane**.

1. Paste the copied text into the script pane.

1. In the script pane, locate the following block of code:

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. In the code block, change `$false` to `$true`.

1. Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.

1. In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.

### Run the Azure Disk Encryption prerequisite setup script

1. In the  PowerShell ISE console pane, type the following command, and press **Enter**:

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. In the PowerShell ISE console pane, type the following command, and press **Enter**:

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).

1. In the  PowerShell ISE console pane, type the following command, and press **Enter**:

   ```powershell
   Login-AzureRmAccount
   ```

1. Enter your Azure credentials.

1. Select your **SubscriptionId** string, and copy it to the clipboard.

1. In the PowerShell ISE, click **File**, and then click **Run**.

1. In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**. Then press **Enter**.

1. In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**. Then press **Enter**.

1. In the console pane, at the **location:** prompt, type the location you used when creating your VM.

1. In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.

1. The **moneyappkv** key vault will now be created. When this has completed, select the summary text (in green), and copy it to Notepad.

1. Press **Enter** to continue.

1. In the console pane, at the **aadAppName:**  prompt, type **moneyapp**. Then press **Enter**.

1. The **moneyapp** Azure AD application will now be created. When this has completed, select the summary text (in green), and copy it to Notepad.

1. Press **Enter** to continue.

### Encrypt your VM disks with PowerShell

Verify the encryption status of the OS and data disks:

1. In the PowerShell ISE console pane, type the following command, and press **Enter**:

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > The VM name must be enclosed in single quotes.

1. In the PowerShell ISE script pane, enter the following command, and press **Enter**:

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.

>[!IMPORTANT]
> Wait until the command has completed before continuing with this exercise.

### Verify the encryption status of your VM disks

Switch to the Azure portal. On the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.
