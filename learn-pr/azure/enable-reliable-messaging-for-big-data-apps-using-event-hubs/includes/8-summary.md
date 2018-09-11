Azure Event Hubs provides big data applications the capability to process large volume of data. It also has the ability to scale out during exceptionally high- demand periods as and when required. Azure Event Hubs decouple the sending and receiving messages to manage the data processing. This helps eliminate the risk of overwhelming consumer application and data loss due to any unplanned interruptions.

In this module, you've seen how to deploy Azure Event Hubs as part of an event processing solution. 
You learned how to:

- Use the Azure CLI commands to create an Event Hubs namespace and an event hub in that namespace. 
- Configure sender and receiver applications to send and receive messages through the event hub.
- Use the Azure portal to view your event hub status and performance.

## Clean up 
<!---TODO: Update for sandbox?--->

The resources you've used for your event hub testing will incur costs against your subscription. When you've have finished with the event hub, remember to remove these resources in order to avoid unnecessary charges.

Because you create the hub, namespace, and storage in the same resource group, the easiest way to clean up your Azure subscription is to remove the resource group, which will remove all its contents. 

Run the following command to remove the resource group, namespace, storage account, and all related resources. Replace `myResourceGroup` with the name of the resource group you created:

```azurecli
az group delete --resource-group myResourceGroup
```

When you are asked to confirm the delete, answer **Yes**.

The command may take several minutes to complete as resources are deleted.
