Setting environment variables in your container instances allows you to provide dynamic configuration of the application or script run by the container. Environment variables are set using the Azure CLI, PowerShell, or the Azure portal at container creation time. Secured environment variables are used to prevent sensitive information from being displayed in container operations output.

In this unit, an Azure Cosmos DB instance is created, and then an Azure container instance runs with the connection information of the Azure Cosmos DB instance stored as environment variables. An application in the container uses the variables to write and read data from Azure Cosmos DB. In this unit, you will create both an environment variable and a secured environment variable.

## Deploy Azure Cosmos DB

Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command. This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.

This command can take a few minutes to complete:

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group myResourceGroup --name aci-cosmos --query documentEndpoint -o tsv)
```

Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group myResourceGroup --name aci-cosmos --query primaryMasterKey -o tsv)
```

## Deploy a container instance

Create an Azure container instance using the `az container create` command. Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`. These variables hold the values needed to connect to the Azure Cosmos DB instance:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

Once the container has been created, get the IP address with the `az container show` command:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query ipAddress.ip --output tsv
```

Open up a browser and navigate to the IP address of the container. You should see the following application. When casing a vote, the vote is stored in the Azure Cosmos DB instance.

![Azure voting application with two choices, cats or dogs.](../media-draft/azure-vote.png)

## Secured environment variables

In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables. By default, environment variables are displayed in the Azure portal and command-line tools in plain text.

For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query containers[0].environmentVariables
```

Example output:

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

Secure environment variables prevent clear text output. To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.

Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

Now, when the container is returned with the `az container show` command, the environment variables are not displayed:

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-secure --query containers[0].environmentVariables
```

Example output:

```bash
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```

## Summary

In this unit, you created an Azure Cosmos DB instance and an Azure container instance. Environment variables were set in the container instance such that the application running inside of the container was able to connect to the Azure Cosmos DB instance.

In the next unit, you will mount data volumes to an Azure container instance for data persistence.
