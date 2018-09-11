You saw two ways to meet demand using Virtual Machines. If your load is predictable you can use the portal or scripts to manually resize your VMs. For unpredictable demand patterns, a better approach is to use scale sets with autoscale to automatically add and remove instances as demand changes. Multiple VMs have the added benefit of increasing the availability of your system since a failed VM will not disrupt your service.

## Cleanup

While a VM is provisioned and running, it incurs costs against your subscription. Because you create all the VMs in the same resource group, the easiest way to clean up your Azure subscription is to remove the resource group, which will remove all its contents. Please run the following PowerShell cmdlet when you are finished with the exercise:

   ```powershell
   Remove-AzureRmResourceGroup -Name ExerciseRG
   ```

When you are asked to confirm the delete, answer **Yes**. The command may take several minutes to complete as resources are deleted.