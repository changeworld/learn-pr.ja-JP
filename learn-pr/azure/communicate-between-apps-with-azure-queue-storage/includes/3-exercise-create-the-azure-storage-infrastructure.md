You've discovered that spikes in traffic can overwhelm our middle-tier. To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.

The first step in creating a queue is to create the Azure Storage Account that will store our data.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## Create a Storage Account with the Azure CLI

> [!TIP] 
> Normally, you'd start a new project by creating a _resource group_ to hold all the associated resources. In this case, we'll be using the Azure Sandbox which provides a resource group named <rgn>[Sandbox resource group name]</rgn>.

1. In the Cloud shell on the right, select Bash if you are given a choice.

1. Use the `az storage account create` command to create the storage account. You'll need to supply several parameters:

| Parameter | Value |
|-----------|-------|
| `--name`  | Sets the name. Remember that storage accounts use the name to generate a public URL - so it has to be unique. In addition, the account name must be between 3 and 24 characters, and be composed of numbers and lowercase letters only. We recommend you use the prefix **articles** with a random number suffix but you can use whatever you like. |
| `-g`        | Supplies the **Resource Group**, use <rgn>[Sandbox resource group name]</rgn> as the value. |
| `--kind`    | Sets the **Storage Account type** - use _StorageV2_ to create a general-purpose V2 account. |
| `-l`        | Sets the **Location** independent of the Resource Group owner. It's optional, but you can use it to place the queue in a different region than the Resource Group. |
| `--sku`     | Sets the **Replication and Storage type**, it defaults to _Standard_RAGRS_. Let's use _Standard_LRS_ which means it's only locally redundant within the data center. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Here's an example command line that uses the above parameters. Make sure to change the `--name` and `--location` parameters if you decide to copy/paste this command.

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 -l [location-name] --sku Standard_LRS
```