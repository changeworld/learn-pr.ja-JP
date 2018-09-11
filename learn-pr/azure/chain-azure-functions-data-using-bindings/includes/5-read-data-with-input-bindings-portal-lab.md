Imagine we want to create a simple bookmark lookup service. Our service is readonly initially. If a user wants to find an entry, they send a request with the ID of the entry and we return the URL. The following diagram explains the flow.

![Flow diagram showing process of finding a bookmark in our Cosmos DB back-end](../media-draft/find-bookmark-flow-small.png)

When a user sends us a request with some text, we try to find an entry in our back-end database that contains this text as a key or Id. We return a result that indicates whether we found the entry or not.

We need to store data somewhere. In this flowchart, our data store is shown as an Azure Cosmos DB. But, how do we connect to that database from a function and read data? In the world of functions, we configure an *input binding* for that job.  Configuring a binding through the Azure Portal is straightforward. As we'll see shortly, You don't have to write code for tasks such as opening a storage connection. The Azure Functions runtime and binding take care of those tasks for you.

> [!IMPORTANT]
> We're going to refer to some resources (database, function, bindings, code) that we create here in our next lab. Please keep these resources around at least until you finish this course.

As was the case for the preceding module, we'll do everything in the Azure portal.

## Create an Azure Cosmos DB 

Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.

### Create a database account

A database account is a container for managing one of more databases. Before we can create a database, we need to create a database account.

1. Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Databases** > **Azure Cosmos DB**.

2. In the **New account** page, enter the settings for the new Azure Cosmos DB account.
 
    Setting|Value|Description
    ---|---|---
    ID|*Enter a unique name*|Enter a unique name to identify this Azure Cosmos DB account. Because *documents.azure.com* is appended to the ID that you provide to create your URI, use a unique but identifiable ID.<br><br>The ID can contain only lowercase letters, numbers, and the hyphen (-) character, and it must contain 3 to 50 characters.
    API|SQL|The API determines the type of account to create. Azure Cosmos DB provides five APIs to suit the needs of your application: SQL (document database), Gremlin (graph database), MongoDB (document database), Azure Table, and Cassandra, each which currently require a separate account. <br><br>Select **SQL**. At this time, the Azure Cosmos DB trigger, input bindings, and output bindings only work with SQL API and Graph API accounts. 
    Subscription|*Your subscription*|Select Azure subscription that you want to use for this Azure Cosmos DB account.
    Resource Group|Use existing<br><br>*Then enter [!INCLUDE [resource-group-name](./rg-name.md)], the resource group we created in an earlier unit for this module's resources.*| We're selecting **Use existing**, because we want to group all resources created for this module under the same resource group.
    Location|Auto-filled once **Use existing** is set. | Select the geographic location in which to host your Azure Cosmos DB account. Use the location that's closest to your users to give them the fastest access to the data. In this lab,  the location is pre-determined for us as the location set for the existing resource group.
    
Leave all other fields in the **New account** blade at their default values because we're using them in this module.  That includes **Enable geo-redundancy**, **Enable Multi Master**, **Virtual networks**.

3. Select **Create** to provision and deploy the database account.

4. Deployment can take some time. So, wait for a **Deployment succeeded** message similar to the following message before proceeding.

<!-- TODO figure out how to center these image -->

![Notification that database account deployment has completed](../media-draft/db-deploy-success.PNG)

5. Congratulations! You've created and deployed your database account!

6. Select **Go to resource** to navigate to the database account in the portal. We'll add a collection to the database next.

### Add a collection

In Cosmos DB, a *container* holds arbitrary user-generated entities. In a database the supports SQL API, a document-oriented API, a container is a *collection*. Inside a collection, we store documents. Hopefully all will be clearer once we create a collection and add some documents.

Let's use the Data Explorer tool in the Azure portal to create a database and collection.

1. Click **Data Explorer** > **New Collection**.

2. In the **Add collection**, enter the settings for the new collection.

    >[!TIP]
    >The **Add Collection** area is displayed on the far right, you may need to scroll right to see it.

    Setting|Suggested value|Description
    ---|---|---
    Database ID|[!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]| Database names must contain from 1 through 255 characters, and they cannot contain /, \\, #, ?, or a trailing space.<br><br>You are free to enter whatever you want here, but we suggest [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] as the name for the new database, and that's what we'll refer to in this unit. |
    Collection ID|[!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)]|Enter [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] as the name for our new collection. Collection IDs have the same character requirements as database names.
    Storage capacity| Fixed (10 GB)|Use the default value of **Fixed (10 GB)**. This value is the storage capacity of the database.
    Throughput|400 RU|Change the throughput to 400 request units per second (RU/s). Storage capacity must be set to **Fixed (10 GB)** in order to set throughput to 400 RU/s. If you want to reduce latency, you can scale up the throughput later.
    
3. Click **OK**. The Data Explorer displays the new database and collection. So, now we have a database. Inside the database, we've defined a collection. Next we'll add some data, also known as documents.

### Add test data

We've defined a collection in our database called [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)], so what are we intending to store in the collection? Well, the idea is to store a URL and ID in each document, like a list of web page bookmarks. 

We'll add data to our new collection using Data Explorer.

1. In Data Explorer, the new database appears in the Collections pane. Expand the [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)] database, expand the [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] collection, click **Documents**, and then click **New Document**.
  
2. Now add a document to the collection with the following structure. Each document is schema-less JSON file.

     ```json
     {
         "id": "docs",
         "URL": "https://docs.microsoft.com/azure"
     }
     ```

3. Once you've added the json to the **Documents** tab, click **Save**.

When the document is saved, notice that there are more properties than the ones we added. They all begin with an underline (_rid, _self, _etag, _attachments, _ts). These are properties generated by the system to help manage the document. The following table explains briefly what they are.


|Property  |Description  |
|---------|---------|
|_rid     |     The resource ID (_rid) is a unique identifier that is also hierarchical per the resource stack on the resource model. It is used internally for placement and navigation of the document resource.    |
|_self     |   The unique addressable URI for the resource.      |
|_etag     |   Required for optimistic concurrency control.     |
|_attachments     |  The addressable path for the attachments resource.       |
|_ts     |    The timestamp of the last update of this resource.    |
 

4. Let's add a few more documents into our collection. For each of the following, use the **New Document** command again to create an entry for each. Don't forget to click ##Save** to capture your additions.

|id  |value  |
|---------|---------|
|portal     |  https://portal.azure.com       |
|learn     |   https://docs.microsoft.com/learn |
|marketplace     |    https://azuremarketplace.microsoft.com/marketplace/apps  |
|blog | https://azure.microsoft.com/blog |

When you've finished, your collection should look like the following screenshot.

![Screenshot of the SQL API UI in the portal that shows the list of entries we added to our bookmarks collection.](../media-draft/db-bookmark-coll.PNG)

We now have a few entries in our bookmark collection. Our scenario will work as follows. If a request arrives with, for example, "id=docs", we'll look up that ID in our bookmarks collection and return the URL https://docs.microsoft.com/azure. Let's make an Azure function that looks up values in this collection.

## Create our function

1. Navigate to the function app you created in the preceding unit.

2. Expand your function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**. This action starts the function creation process. The following animation illustrates this action.

![Animation of the plus sign appearing when the user hovers over the functions menu item.](../media-draft/func-app-plus-hover-small.gif)

3. The page shows us the complete set of supported triggers. Select **HTTP trigger**, which is the first entry in the following screenshot.

![Screenshot of part of the trigger template selection UI, with the TTP trigger displayed first, in the top left of the image.](../media-draft/trigger-templates-small.PNG)

4. Fill out the **New Function** dialog that appears to the right  using the following values.

|Field  |Value  |
|---------|---------|
|Language     | **JavaScript**        |
|Name     |   [!INCLUDE [func-name-find](./func-name-find.md)]     |
| Authorization level | **Function** |

5. Select **Create** to create our function, which opens the index.js file in the code editor and displays a default implementation of the HTTP-triggered function.

You can verify what we have done so far by testing our new function as follows:

1. In your new function, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy**.

2. Paste the function URL you copied into your browser's address bar. Add the query string value `&name=<yourname>` to the end of this URL and press the `Enter` key on your keyboard to execute the request. You should see a response similar to the following response returned by the function displayed in your browser.  

Now that we have our bare-bones function working, let's turn our attention to reading data from our Azure Cosmos DB, or in our scenario, our [!INCLUDE [cosmos-coll-name](./cosmos-coll-name.md)] collection.

## Add a Cosmos DB input binding

We want to read data from the database we created, so enter input bindings. As you'll see, we can configure a binding that can talk to our database in just a few steps.

1. Select **Integrate** in the function menu on the left to open the integration tab.

The template we used created an HTTP trigger and an HTTP output binding for us. Let's add our new Azure Cosmos DB input binding. 

2. Select **+ New Input** under the **Inputs** column. A list of all possible input binding types is displayed.

3. Click on **Azure Cosmos DB** from the list and then the **Select** button. This action opens the Azure Cosmos DB input configuration page.

Next, we'll set up a connection to our database.

4. In the field named **Azure Cosmos DB account connection** on this page, click on *new* to the right of the empty field. This action opens the **Connection** dialog, which already has **Azure Cosmos DB account** and your Azure subscription selected. The only thing left to do is to select a database account id.

5. In the section, **Create a database account**, you had to supply an ID value. Now find that value in the  *Database Account* dropdown and then click **Select**.

A new connection to the database is configured and is shown in the **Azure Cosmos DB account connection** field. If you're curious about what is actually behind this abstract name, just click *show value* to reveal the connection string.

We want to look up a bookmark with a specific ID, so let's tie the ID we receive to the binding.

7. In the **Document ID (optional)** field, enter `{id}`. This is known as a *binding expression*. The function is triggered by an HTTP request that uses a query string to specify the ID to look up. Since IDs are unique in our collection, the binding will return either 0 (not found) or 1 (found) documents.

8. Carefully fill out the remaining fields on this page using the values in the following table. At any time, you can click on the information icon to the right of each field name to learn more about the purpose of each field.


|Setting  |Value  |Description  |
|---------|---------|---------|
|Document parameter name     |  **bookmark**       |  The name used to identify this binding in your code.      |
|Database name     |  [!INCLUDE [cosmos-db-name](./cosmos-db-name.md)]       | The database where data will be read. This is the database name we set earlier in this lesson.        |
|Collection Name     |  [!INCLUDE [cosmos-db-name](./cosmos-coll-name.md)]        | The collection from which we'll read data. This setting was defined earlier in the lesson. |
|SQL Query (optional)    |   leave blank       |   We are only retrieving one document at a time based on the ID. So, filtering with the Document ID field is a better than using a SQL Query in this instance. We could craft a SQL Query to return one entry (`SELECT * from b where b.ID = {id}`). That query would indeed return a document, but it would return it in a document collection. Our code would have to manipulate a collection unnecessarily. Use the SQL Query approach when you want to get multiple documents.   |
|Partition key (optional)     |   leave blank      |  We can accept the default here.       |

9. Click **Save** to save all changes to this binding configuration. Now that we have our binding defined, it's time to use it in our function.

## Update function implementation

1. Click on our function, [!INCLUDE [func-name-find](./func-name-find.md)], to open up *index.js* in the code editor. We've added an input binding to read from our database, so let's update the logic to use this binding.

2. Replace all code in index.js with the code from the following snippet.

[!code-javascript[](../code/find-bookmark-single.js)]

When an HTTP request causes our function to trigger, the `id` query parameter is passed to our Cosmos DB input binding. If it found a document that matches this ID, then the `bookmark` parameter will be set to it. In that case, we construct a response that contains the URL value found in the bookmark document. If no document was found matching this key, we respond with a payload and status code that tells the user the bad news.

## Try it out

1. As usual, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy** to copy the function's URL.

2. Paste the function URL you copied into your browser's address bar. Add the query string value `&id=docs` to the end of this URL and press the `Enter` key on your keyboard to execute the request. All going well, you should see a response that includes a URL to that resource.

3. Replace `&id=docs` with `&id=missing` and observe the response.

4. Replace the previous query string with `&id=` and observe the response.

>[!TIP]
>You can also test the function using the **Test** tab in the function portal UI. You can add a query parameter or just supply a request body to get the same results as described in te preceding steps.

In this unit, we created our first input binding  manually to read from an Azure Cosmos DB database. The amount of code we wrote to search our database and read data was minimal, thanks to bindings. We did most of our work configuring the binding declaratively and the platform took care of the rest.  

In the next unit, we'll add more data to our bookmark collection through an Azure Cosmos DB output binding.

> [!TIP]
> This unit is not intended to be a tutorial on Azure Cosmos DB. If you would like to dive deeper, here are a few resources to get you started:
>
>* [Introduction to Azure Cosmos DB: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
>
>* [A technical overview of Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
>
>* [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/cosmos-db/)