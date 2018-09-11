In this unit, you will create an Azure container registry using the Azure CLI.

## Create an Azure container registry

Before you create your Azure container registry, you need a *resource group* to deploy it to. A resource group is a logical collection into which all Azure resources are deployed and managed.

Create a resource group with the `az group create` command. In the following example, a resource group named *myResourceGroup* is created in the *eastus* region:

```azurecli
az group create --name myResourceGroup --location eastus
```

Once you've created the resource group, create an Azure container registry with the `az acr create` command. The container registry name must be unique within Azure, and contain between 5 and 50 alphanumeric characters. Replace `<acrName>` with a unique name for your registry.

For this example, a premium registry SKU is deployed. The premium SKU is required for geo-replication. For more information on Container Registry SKUs, see, [Azure Container Registry SKUs](https://docs.microsoft.com/azure/container-registry/container-registry-skus)

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

Here's example output for a new Azure container registry:

```console
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.

## Summary

In this unit, you created an Azure container registry using the Azure CLI.