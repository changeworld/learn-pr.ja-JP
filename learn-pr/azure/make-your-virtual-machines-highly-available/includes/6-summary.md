Here, you learned how to use a load balancer as part of a solution for delivering high availability for virtual machines hosted in Azure. This included the load balancer itself, the associated virtual networks, rules to control the balancing algorithm, and health probes that identify which virtual machines are running correctly.

## Clean up
<!---TODO: Update for sandbox?--->

Running virtual machines and scale sets incurs costs against your subscription. You should remove unneeded resources to avoid unnecessary charges. The easiest way to clean up your Azure subscription is to remove the resource group; this will also delete all the resources in the group. When you are finished with this module, please run the following Azure PowerShell cmdlet:

```powershell
Remove-AzureRmResourceGroup -Name woodgrove-RG
```

When you are asked to confirm the delete, answer **Yes**. The command may take several minutes to complete as resources are deleted.