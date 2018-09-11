> [!NOTE]
> If you are continuing on from **Create an Azure Cosmos DB database built to scale** and you did not delete the Cosmos DB database + collection that you created, then you can skip this unit and move on to adding data with the Data Explorer.

The first thing we need to do is create an empty Cosmos DB database and collection to work with. We want it to match the one you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**. Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.

# Create a Cosmos DB account + database with the Azure CLI

1. Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.

    ```azurecli
    az account list --output table
    ```

1. Make sure you see "sandbox" in the subscription list and set it as the current one to use: <!-- TODO: get official name here -->

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Get the Resource Group that has been created for you. If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below. Take note of the Resource Group name. This is where we will create our database. <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. To make this a bit easier, set a few environment variables so you don't have to type the common values each time. 

    > [!IMPORTANT]
    > Make sure to change these values to ones appropriate for your session. For example, replace the `<resource group>` value with the Resource Group name you identified above.

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. Next, set a variable for the database name. Name it "Users" so it matches the database we created in the last module.

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group. **Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step. Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. Next, create the Cosmos DB account. This will take a few minutes to complete.

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. Create the `Products` database in the account.

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. Finally, create the `Clothing` collection.

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

Now that we have our Cosmos DB account, database, and collection, let's go add some data!