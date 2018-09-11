Azure provides Storage Service Encryption (SSE) and Azure Disk Encryption (ADE) to secure Azure VM disks. These technologies work together to provide strong 256-bit encryption, as part of a defense-in-depth approach for the protection of Azure VM disks. It's required that you complete the Azure Disk Encryption prerequisites to enable disk encryption. The Azure Disk Encryption prerequisites configuration script can automate this process. When enabling encryption on new VMs, you can use an Azure Resource Manager template. This ensures that your data is encrypted at the point of deployment, leaving no vulnerabilities.

## Clean up
<!---TODO: Update for sandbox?--->
Running Azure VMs, and the associated storage, incurs costs against your subscription. You'll want to remove unneeded resources to avoid unnecessary charges. The easiest way to clean up your Azure subscription is to remove the resource group; this will also delete all the resources in the group. When you are finished with this module, run the following Azure PowerShell cmdlet:

   ```powershell
   Remove-AzureRmResourceGroup -Name moneyapprg
   ```

When you are asked to confirm the deletion, answer **Yes**. The command may take several minutes to complete as resources are deleted. 

If you get a deletion failed message, you may need to remove locks on the key vault before you can delete the resource group.
