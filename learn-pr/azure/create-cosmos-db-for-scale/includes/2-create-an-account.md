Your company has chosen Azure Cosmos DB to meet the demands of their expanding customer and product base. You have been tasked with creating the database.

The first step is to create an Azure Cosmos DB account.

## What is an Azure Cosmos DB account?

Azure Cosmos DB account is an Azure resource that acts as an organizational entity for your databases. It connects your usage to your Azure subscription for billing purposes.

Each Azure Cosmos DB account is associated with one of the several data models Azure Cosmos DB supports, and you can create as many accounts as you need. 

SQL API is the preferred data model if you are creating a new application. If you're working with graphs or tables, or migrating your MongoDB or Cassandra data to Azure, create additional accounts and select relevant data models.

When creating an account, choose an ID that is meaningful to you; it is how you identify your account. Further, create the account in the Azure region that's closest to your users to minimize latency between the datacenter and your users.

You can optionally set up virtual networks and geo-redundancy during account creation, but this can also be done later. In this module we will not enable those settings.

## Creating an Azure Cosmos DB account in the portal

1. Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Click **Create a resource** > **Databases** > **Cosmos DB**.
   
   ![The Azure portal Databases pane](../media-draft/2-create-nosql-db-databases-json-tutorial.png)

1. On the **New account** page, enter the settings for the new Azure Cosmos DB account.
 
    Setting|Value|Description
    ---|---|---
    ID|*Enter a unique name*|Enter a unique name to identify this Azure Cosmos DB account. Because *documents.azure.com* is appended to the ID that you provide to create your URI, use a unique but identifiable ID.<br><br>The ID can contain only lowercase letters, numbers, and the hyphen (-) character, and it must contain 3 to 50 characters.
    API|SQL|The API determines the type of account to create. Azure Cosmos DB provides five APIs to suit the needs of your application: SQL (document database), Gremlin (graph database), MongoDB (document database), Azure Table, and Cassandra, each of which currently requires a separate account. <br><br>Select **SQL** because in this module you are creating a document database that is queryable using SQL syntax and accessible with the SQL API.|
    Subscription|*Your subscription*|Select the Azure subscription that you want to use for this Azure Cosmos DB account.
    Resource Group|Create new<br><br>*Then enter the same unique name as provided above in ID*|Select **Create New**, and then enter a new resource-group name for your account. For simplicity, the same name can be used as your ID. 
    Location|*Select the region closest to your users*|Select the geographic location in which to host your Azure Cosmos DB account. Use the location that's closest to your users to give them the fastest access to the data.
    Enable geo-redundancy| Leave blank | This setting creates a replicated version of your database in a second (paired) region. Leave this blank for now, as the database can be replicated later.
    Virtual networks|Disabled|Leave virtual networks disabled for now. This can be enabled later.

1. Click **Create**.

    ![The new account page for Azure Cosmos DB](../media-draft/2-azure-cosmos-db-create-new-account.png)

1. The account creation takes a few minutes. Wait for the portal to display the notification that the deployment succeeded and click the notification. 

    ![Notification alert](../media-draft/2-azure-cosmos-db-notification.png)

1. In the notification window, click **Go to resource**.

    ![Go to resource](../media-draft/2-azure-cosmos-db-go-to-resource.png)

    The portal displays the **Congratulations! Your Azure Cosmos DB account was created** page.

    ![The Azure portal Notifications pane](../media-draft/2-azure-cosmos-db-account-created.png)

## Summary

You have created an Azure Cosmos DB account, which is the first step in creating an Azure Cosmos DB database. You selected appropriate settings for your data types and set the account location to minimize latency for your users.
