Our goal is to create a new Azure virtual machine. We'll need to supply several pieces of information to identify the resource location, OS to use, and the hardware configuration needed for the VM. Let's start with the **resource group**.

## Create a resource group

Azure uses _resource groups_ to group related resources such as virtual machines and databases together. The resource group also identifies a specific location (called a "region") which will decide what data center the resource is placed into.

> [!NOTE]
> The Azure sandbox provides a pre-created resource group named <rgn>[Sandbox resource group name]</rgn>. You do not need to execute these steps. However, when you are creating your _own_ resources for real projects, these will be the commands you will need to perform. The Azure sandbox does not allow you to create resource groups directly.

As an example, you could type the following Azure CLI command in Azure Cloud Shell to create a resource group in the **East US** region. You would replace **[resource-group]** with a valid name that is unique within the active subscription.

```azurecli
az group create --name [resource-group] --location eastus
```

This would return a JSON block indicating the resource group has been created.

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/<resourcegroup>",
  "location": "eastus",
  "managedBy": null,
  "name": "<resourcegroup>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Notice that it returns the subscription unique identifier, location, and name as part of the response. You can use these to verify it got created in the proper subscription and location.

Now that we know how to create a resource group, let's create a new virtual machine.