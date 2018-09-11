In this module, you've seen how queues in Azure storage accounts are used to pass messages between components in a distributed application. Using queues in this way can help to make a distributed application more reliable and resilient to failures and periods of high demand. If you use the Microsoft Azure Storage Client Library for .NET, you can easily write C# or VB.NET code that creates queues, adds messages, or retrieves and removes messages from queues.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

When you are working in your own subscription, you can execute the following Azure CLI command to delete the resource group and all associated resources.

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

The optional `--no-wait` option tells the shell to not wait for Azure to complete the operation.