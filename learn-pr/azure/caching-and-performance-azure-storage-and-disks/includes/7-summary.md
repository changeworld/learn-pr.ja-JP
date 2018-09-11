In this module, you learned about Azure disk caching and how it potentially improves performance. We used the Azure portal and Azure PowerShell to manage disk caching for our VM. 

Once you have an Azure VM disk caching strategy in place, you can then quickly and easily deploy new VMs and disks with the optimum disk cache settings by using scripts and templates.

## Further reading

- [Azure Premium Storage: Design for High Performance](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [Get started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [Azure Computer Cmdlets Reference](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## Cleanup
<!---TODO: Update for sandbox?--->

Running Azure VMs incurs costs against your subscription. Remove unneeded resources to avoid unnecessary charges. The easiest way to clean up your Azure subscription is to remove the resource group. Deleting a resource group deletes all the resources in that group. When you're finished with this module, run the following Azure PowerShell cmdlet:

```powershell
Remove-AzureRmResourceGroup -Name "fotoshare-rg"
```

When asked to confirm the deletion, answer **Yes**. The command may take several minutes to complete as resources are deleted.
