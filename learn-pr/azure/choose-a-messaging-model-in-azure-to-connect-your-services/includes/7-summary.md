In this module, you have explored four different Azure services that allow you to create reliable and resilient distributed applications. Choosing between them is a matter of deciding the type of data that needs to be passed between components (messages or events), and then what features you need to deliver and process the data.

## Clean up
<!---TODO: Update for sandbox?--->

While a Storage Account contains data, it incurs a cost against your Azure subscription, although these are likely to be low for small queue with few messages. When you have finished with the queue, remember to remove it in order to avoid unnecessary charges. Because you created all the resources in the same resource group, the easiest way to cleanup your Azure subscription is to remove the resource group which will remove all its contents:

```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```

When you are asked to confirm the delete, answer **Yes**.

The command may take several minutes to complete as resources are deleted.