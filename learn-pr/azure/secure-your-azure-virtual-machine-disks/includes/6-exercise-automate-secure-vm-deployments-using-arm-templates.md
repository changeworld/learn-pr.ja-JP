Your banking company handles highly sensitive customer information and wants you to ensure your disks are encrypted at all times, including VM disk deployment. You've been tasked to automate a secure VM deployment to safeguard your company's data.

In this unit, you'll use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.

## Configure and deploy a new VM using an Azure Resource Manager template

1. Go to the [Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), and then click **Deploy to Azure**.
1. In the Azure portal, on the **Azure quickstart template** blade, under **Resource Group**, select **Use existing**. In the list, select **moneyapprg**.
1. In the **SETTINGS** section, enter the following information:

   - Vm Name: **moneyappsvr02**
   - **Admin Username**: Same as you used in the previous exercise.
   - **Admin Password**: Same as you used in the previous exercise.
   - **New Storage Account Name**: Enter a unique name.
   - **Vm Size**: Replace with the same size that you used in the previous exercise, such as **Standard_B1s** (as you are using the same Azure region, ensuring that size is available in your current region).
   - **Virtual Network Name**: **moneyapprg-vnet**
   - **Subnet Name**: **default**
   - **AAD Client ID**: Copy from the information you pasted to Notepad.
   - **AAD Client Secret**: Copy from the information you pasted to Notepad.
   - **Key Vault Name**: **moneyappkv**
   - **Key Vault Resource Group**: **moneyapprg**
   - **Key Encryption Key URL**: Copy from the information you pasted to Notepad.
1. Select the **I agree to the terms and conditions** check box, and then click **Purchase**.

The deployment may take 5-10 minutes to complete.

## Verify encryption status of new VM

1. In the sidebar of the Azure portal, click **Virtual machines**.

1. On the **Virtual machines** blade, click **moneyappsvr02**.

1. On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.

1. On the **Disks** blade, notice the OS disk encryption status is **Enabled**.
