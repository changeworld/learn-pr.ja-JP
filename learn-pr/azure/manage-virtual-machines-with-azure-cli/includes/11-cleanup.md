You've created a new Linux virtual machine, changed its size, stopped and started it, and updated the configuration with the Azure CLI.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

When you are working in your own subscription, you can execute the following Azure CLI command to delete the resource group and all associated resources.

```azurecli
az group delete --name [resource-group-name] --no-wait
```

Answer "yes" to the prompt when shown, or use the `--yes` parameter to auto-answer the prompt.

This command deletes all of the resources associated with the resource group, and is guaranteed to deallocate them in the correct order. The `--no-wait` parameter keeps the Azure CLI from blocking while the deletion takes place. Leave it off to wait for the resources to be deleted. Or use the `az group wait -n [resource-group-name] --deleted` command later in your script to wait for the deletion to finish.


## Further reading

- [Azure CLI overview](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure CLI command reference](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
