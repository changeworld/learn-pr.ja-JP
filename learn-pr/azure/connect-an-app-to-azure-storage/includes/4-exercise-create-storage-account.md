Now that we have an app, we need an Azure storage account to work with. We created one using the Azure portal in the **Create an Azure storage account** module. Let's use the Azure CLI this time.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## Use the Azure CLI to create an Azure storage account

We will use the `az storage account create` command to create a new storage account. It takes several parameters which we either need to supply (or should) to configure it the way we want.

> [!div class="mx-tableFixed"]
> | Option | Description |
> |--------|-------------|
> | `--name` | A **Storage account name**. The name will be used to generate the public URL used to access the data in the account. It must be unique across all existing storage account names in Azure. It must be 3 to 24 characters long and can contain only lowercase letters and numbers. |
> | `--resource-group` | Use <rgn>[Sandbox resource group name]</rgn> to place the storage account into the free sandbox. |
> | `--location` | Select a location near you. |
> | `--kind` | This determines the storage account _type_. Options include BlobStorage, Storage, and StorageV2. |
> | `--sku` | This decides the storage account performance and replication model. Options include Premium_LRS, Standard_GRS, Standard_LRS, Standard_RAGRS, and Standard_ZRS. |
> | `--access-tier` | The **Access tier** is only used for Blob storage, available options are Cool and Hot. The **Hot Access Tier** is ideal for frequently accessed data, and the **Cool Access Tier** is better for infrequently accessed data. Note that this only sets the _default_ value - when you create a Blob, you can set a different value for the data. |
    
Use the above table to craft a command line in the Cloud Shell on the right to create the account.
- Use a unique name, we recommend something like "photostore" with your initials and a random number. You will get an error if it's not unique.
- Normally you would create a new resource group to hold your app resources, in this case use the Sandbox resource group.
- Use "Standard_LRS" for the **sku**, this will use standard storage with local replication which is fine for this example.
- Use "Cold" for the **Access Tier**.

### Selecting a location
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### Example command

```bash
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cold
```

> [!TIP]
> If you are interested in exploring the options for the storage account, make sure to go through the **Create an Azure storage account** where we go through them in depth.

It will take a few minutes to deploy the account. While Azure is working on that, let's explore the APIs we'll use with this account.