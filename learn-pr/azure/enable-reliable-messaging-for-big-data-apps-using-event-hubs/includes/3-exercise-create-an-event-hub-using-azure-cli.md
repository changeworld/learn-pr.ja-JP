You're now ready to create a new event hub. After creating the event hub, you'll use the Azure portal to view your new hub.

You'll create an event hub using the Azure CLI. For this exercise, use the Azure CLI 2.0. 

## Create an Event Hubs namespace

Use the following steps to create an Event Hubs namespace using Bash shell supported by Azure Cloud shell:

1. Sign in to the Cloud Shell (Bash).  

1. Create an Azure resource group using the following command:

    ```azurecli
        az group create --name <resource group name> --location <location>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--name (required)      |Enter a new resource group name.|
    |--location (required)     |Enter the location of your nearest Azure datacenter, for example, westus.|

1. Create the Event Hubs namespace using the following command:

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--name (required)      |Enter a 6-50 characters-long unique name for your Event Hubs namespace. The name should contain only letters, numbers, and hyphens. It should start with a letter and end with a letter or number.|
    |--resource-group (required)  |Enter the resource group you created in step 1.
    |--l (optional)     |Enter the location of your nearest Azure datacenter, for example, westus.|

1. Fetch the connection string for your Event Hubs namespace using the following command. You'll need this to configure applications to send and receive messages using your event hub.

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--resource-group (required)  |Enter the resource group you created in step 1.|
    |--namespace-name (required)      |Enter the namespace you created in step 2.|

    This command returns the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications. Save the value of the following keys for later use.

    - **primaryConnectionString**
    - **primaryKey**

## Create an event hub

Use the following steps to create your new event hub:

1. Create a new event hub using the following command:

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--name (required)  |Enter a name for your event hub.|
    |--resource-group (required)  |Enter the resource group you created in the previous procedure.|
    |--namespace-name (required)      |Enter the namespace you created in the previous procedure.|

1. View the details of your event hub using the following command: 

    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--resource-group (required)  |Enter the resource group that you created in the previous procedure.|
    |--namespace-name (required)      |Enter the namespace you created in the previous procedure.|
    |--name  (required)|Enter the name of the event hub you created in step 1.|

## View the event hub in the Azure portal

Use the following steps view your event hub in the Azure portal.

1. Find your Event Hubs namespace using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Click your namespace to open it.

1. From **Event Hubs Namespace** > **ENTITIES**, click **Event Hubs**.
    Your event hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).

    ![Event Hub displayed in the Azure Portal](../media-draft/3-event-hub.png)

## Summary

You've now created a new event hub, and you've all the necessary information ready to configure your publisher and consumer applications.
