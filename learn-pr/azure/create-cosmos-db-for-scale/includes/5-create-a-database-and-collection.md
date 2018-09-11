Now that you understand how request units are used to determine database throughput and how the partition key creates the scale-out strategy for your database, you're ready to create your database and collection.

## Creating your database and collection

1. In the Azure Portal, select **Data Explorer** from your Cosmos DB resource and then click the **New Collection** button in the toolbar.
    
    The **Add Collection** area is displayed on the far right. You may need to scroll right to see it.

    ![The Azure portal Data Explorer, Add Collection blade](../media-draft/5-azure-cosmosdb-data-explorer.png)

1. In the **Add collection** page, enter the settings for the new collection.

    Setting | Suggested value | Description
    --------|-----------------|-------------
    Database id      | Products         | Enter *Products* as the name for the new database. Database names must contain from 1 through 255 characters, and they cannot contain /, \\, #, ?, or a trailing space.
    Collection id    | Clothing  | Enter *Clothing* as the name for your new collection. Collection ids have the same character requirements as database names.
    Storage capacity | Unlimited     | Use the default value of **Unlimited**. This value is the storage capacity of the database, and it enables your database to scale out as needed.
    Partition key    | productId        | productId is a good partition key for an online retail scenario, as so many queries are based around the product ID.
    Throughput       |1000 RU        | Change the throughput to 1000 request units per second (RU/s). 1000 is the minimum RU/s value you can set to enable automatic scaling.
    
    For now, don't check the **Provision database throughput** option, and don't add any unique keys to the collection.
    
1. Click **OK**. The Data Explorer will display the new database and collection.

    ![The Azure portal Data Explorer, showing the new database and collection](../media-draft/5-azure-cosmos-db-new-collection.png)

## Summary

In this unit, you used your knowledge of partition keys and request units to create a database and collection with throughput and scaling settings appropriate for your business needs.