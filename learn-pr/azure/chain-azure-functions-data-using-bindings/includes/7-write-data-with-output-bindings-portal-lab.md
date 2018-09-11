In our last exercise, we implemented a scenario to look up bookmarks in an Azure Cosmos DB database. We configured an input binding to read data from our bookmarks collection. But just reading data is boring, so let's do more. Let's expand the scenario to include writing. Consider the following flowchart.

![Flow diagram showing process of adding a bookmark in our Cosmos DB back-end](../media-draft/add-bookmark-flow-small.png)

In this scenario, we'll receive requests to add bookmarks to our list. The requests pass in the desired key, or ID, along with the bookmark URL. As you can see in the flow chart, we'll respond with an error if the key already exists in our back-end.

If the key that was passed to us is *not* found, we'll add the new bookmark to our database. We could stop there, but let's do a little more.

Notice another step in the flowchart? So far we haven't done much with the data that we receive in terms of processing. We move what we receive into a database. However, in a real solution, it is possible that we'd probably process the data in some fashion. We can decide to do all processing in the same function, but in this lab we'll show a pattern that offloads further processing to another component or piece of business logic.

What might be a good example of this offloading of work in our bookmarks scenario? Well what if we send the new bookmark to a QR code generation service? That service would, in turn, generate a QR code for the URL, store the image in blob storage, and add the address of the qr image back into the entry in our bookmarks collection. Calling a service to generate a qr image is time consuming so, rather than wait for the result, we hand it off to a function and let it take care of this asynchronously.

Just as Azure Functions supports input bindings for different integration sources, it also has a set of output bindings templates to make it easy for you to write data to data sources. Output bindings are also configured in the *function.json* file.  As we'll see in this exercise, we can configure our function to work with multiple data sources and services.


> [!IMPORTANT]
> This exercise builds on the exercise in the last unit, namely, it uses the same Azure Cosmos DB database and input binding. If you haven't worked through that unit, we recommend doing so before proceeding  with this lab.

## Create an HTTP_triggered Function

1. Make sure you are signed in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with the same Azure account you've used throughout this module.

2. In the Azure portal, navigate to the function app you created in this module.

3. Expand your function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**. This action starts the function creation process. The following animation illustrates this action.

![Animation of the plus sign appearing when the user hovers over the functions menu item.](../media-draft/func-app-plus-hover-small.gif)

4. The page shows us the current set of supported triggers. Select **HTTP trigger**, which is the first entry in the following screenshot.

![Screenshot of part of the trigger template selection UI, with the TTP trigger displayed first, in the top left of the image.](../media-draft/trigger-templates-small.PNG)

5. Fill out the **New Function** dialog that appears to the right  using the following values.

|Field  |Value  |
|---------|---------|
|Language     | **JavaScript**        |
|Name     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
| Authorization level | **Function** |

5. Select **Create** to create our function, which opens the index.js file in the code editor and displays a default implementation of the HTTP-triggered function.

In this exercise, we'll speed up things by using the *code* and *configuration* from the previous unit as a starting point.

6. Replace all code in index.js with the code from the following snippet and click **Save** to save this change. 

[!code-javascript[](../code/find-bookmark-single.js)]

If this code looks familiar, that's because it's the implementation of our [!INCLUDE [func-name-find](./func-name-find.md)] function. As you would expect, the function won't work until we define the same bindings.  

7. Open the *function.json* file from the [!INCLUDE [func-name-find](./func-name-find.md)] function. You'll find it by opening the **View files** menu to the right of the code editor.

8. Copy the entire contents of this file.

9. Open the *function.json* file from the [!INCLUDE [func-name-add](./func-name-add.md)] function.

10. Replace the contents of this file with the content you copied from the *function.json* file associated with the [!INCLUDE [func-name-find](./func-name-find.md)] function. When you're done, your function.json should contain the following JSON.

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "documentDB",
      "name": "bookmark",
      "databaseName": "func-io-learn-db",
      "collectionName": "Bookmarks",
      "connection": "unit3test_DOCUMENTDB",
      "direction": "in",
      "id": "{id}"
    }
  ],
  "disabled": false
}
```

11. Make sure to **Save** all changes.

In the preceding steps, we configured bindings for our new function by copying binding definitions from another. We could, of course, created a new binding through the UI, but it is good to understand that this alternative is available to you.

## Try it out

1. As usual, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy** to copy the function's URL.

2. Paste the function URL you copied into your browser's address bar. Add the query string value `&id=docs` to the end of this URL and press the `Enter` key on your keyboard to execute the request. All going well, you should see a response that includes a URL to that resource.

So, where are we at? Well, so far we've really just replicated what we did in the previous lab. But that's ok. We're copying what we did in the last lab to serve as a starting point for this one. We'll work on the new stuff next, namely, writing to our database. For that, we'll need an *output binding*.

## Define Azure Cosmos DB output binding

Rather than define a new output binding by going through the user interface, we'll create this binding by updating the configuration file, *function.json*, by hand. 

1. Open the **function.json** file for this function in the editor by selecting it in the **View files** list.

2. Copy the binding with the name `bookmark` in that file.

3. Place your cursor directly after the closing curly bracket, right before the closing square bracket. Add a comma `,` and then paste the copy of the binding here. Your *function.json* config should now look like the following.

[!code-json[](../code/config-new-entry.json?highlight=22-31)]

4. Edit the binding we pasted, with the following changes.


|Property   |Old value  |New value  |
|---------|---------|---------|
|name     |   bookmark      |  **newbookmark**       |
|direction     |   in      |   **out**      |
|id     |      {id}   |   **delete this property. It does not exist for the output binding.**      |

When you make these changes, you end up with a file that looks like the following JSON.

[!code-json[](../code/config-q-complete.json?highlight=22-30)]

That was just a demo of how you can also create bindings directly in the configuration file. In this example, it makes sense because we are reusing the properties from another binding, namely, the `databaseName`, `collectionName` and `connection` that we already configured for our Cosmos DB input binding.

> [!NOTE]
> The actual value of `connection` in the preceding JSON file will be whatever name your connection was given when it was created.

Before we update our code, let's add one more binding that will enable us to post messages to a queue.

## Define Azure Queue Storage output binding

Azure Queue storage is a service for storing messages that can be accessed from anywhere in the world. A single message can be up to 64 KB and a queue can contain millions of messages up to the total capacity limit of the storage account in which it is defined. The following diagram shows at a high level how a queue will be used in our scenario.

![Diagram showing concept of a storage queue and two functions pushing and popping messages onto the queue.](../media-draft/q-logical-small.png)

Here we can see that our new function, [!INCLUDE [func-name-add](./func-name-add.md)], adds messages to a queue. Another function, for example a fictitious function called *gen-qr-code*, will pop messages from the same queue and process the request.  Since we write, or *push*, messages to the queue from [!INCLUDE [func-name-add](./func-name-add.md)], we'll add a new  output binding to our solution. Let's create the binding through the UI this time.

1. Select **Integrate** in the function menu on the left to open the integration tab.

2. Select **+ New Output** under the **Outputs** column. A list of all possible output binding types is displayed.

3. Click on **Azure Queue Storage** from the list and then the **Select** button. This action opens the Azure Queue Storage output configuration page.

Next, we'll set up a storage account connection. This is where our queue will be hosted.

4. In the field named **Storage account connection** on this page, click on *new* to the right of the empty field. This action opens the **Storage Account** selection dialog. 

5. When we started this module and created our function app, a storage account was also created at that time. It will be listed in this dialog, so go ahead and select it. The **Storage account connection** field is populated with the name of a connection. If you want to see the connection string value, click on **show value**.

6. Although we could leave all other fields on this page with their default values, let's change the following to lend more meaning to the properties.


|Property  |Old value  |New value  | Description |
|---------|---------|---------|---------|
|Queue name     |    outqueue     |  **bookmarks-post-process**      | This is the name of the queue we are using to place bookmarks into so that they can be processed further by another function. |
| Message parameter name    |  outputQueueItem       |   **newmessage**      | This is the binding property we'll use in code. |


7. Remember to click **Save** to save your changes.

## Update function implementation

We now have all our bindings set up for the [!INCLUDE [func-name-add](./func-name-add.md)] function. It's time to use them in our function.

1.  Click on our function, [!INCLUDE [func-name-add](./func-name-add.md)], to open up *index.js* in the code editor.

2. Replace all code in index.js with the code from the following snippet.

[!code-javascript[](../code/add-bookmark.js)]

Let's breakdown what this code does.

* Since this function changes our data, we expect the HTTP request to be a POST and the bookmark data to be part of the request body.
* Our Cosmos DB input binding attempts to retrieve a document, or bookmark, using the `id` that we receive. If it finds an entry, the `bookmark` object will be set. The `if(bookmark)` condition checks whether an entry was found.
* Adding to the database is a simple as setting the `context.bindings.newbookmark` binding parameter to the new bookmark entry, which we have created as a JSON string.
* Posting a message to our queue is as simple as setting the  `context.bindings.newmessage parameter`.

> [!NOTE]
> The only task we performed was to create a queue binding. We never created the queue explicitly. You are witnessing the power of bindings! As the following callout says, the queue is automatically created for you if it doesn't exist!

![Screenshot calling out that the queue will be auto-created.](../media-draft/q-auto-create-small.png)

So, that's it - let's see our work in action in the next section.

## Try it out

Now that we have multiple output bindings, our testing becomes a little trickier. Whereas in previous labs we were content to test by sending an HTTP request and a query string, we'll want to perform an HTTP Post this time. We also need to check whether messages are making it into a queue.

1.  With our function, [!INCLUDE [func-name-add](./func-name-add.md)], selected in the Function Apps portal, click on the Test menu item on the far left to expand it.

2. Select the **Test** menu item and verify that you have the test panel open. The following screenshot shows what it should look like. 

![Screenshot showing the function Test Panel expanded.](../media-draft/test-panel-open-small.png)

> [!IMPORTANT]
> Make sure **POST** is selected in the HTTP method dropdown.

3. Replace the content of the request body with the following JSON payload.

```json
  {
      "id": "docs",
      "url": "https://docs.microsoft.com/azure"
  }
  ```

4. Click **Run** at the bottom of the test panel. 

5. Verify that the *Output* window displays the "Bookmark already exists." message as shown in the following diagram. 

![Screenshot showing Test Panel and result of a failed test.](../media-draft/test-exists-small.png)

6. Now replace the Request body with the following payload. 

```json
  {
      "id": "github",
      "URL": "https://www.github.com"
  }
  ```
7. Click **Run** at the bottom of the test panel.

8. Verify the that *Output* box displays the "bookmark added" message as shown in the following diagram.

![Screenshot showing Test Panel and result of a successful test.](../media-draft/test-success-small.png)

Congratulations! The [!INCLUDE [func-name-add](./func-name-add.md)] works as designed, but what about that queue operation we had in the code? Well, let's go see if something was written to a queue.

### Verify that a message is written to our queue

Azure Queue Storage queues are hosted in a storage account. You selected the storage account in this exercise  already when creating the output binding. 

1. In the main search box in the Azure portal, type *storage accounts* and in the search results select **Storage accounts** under the *Services* category. This is illustrated in the following screenshot. 

![Screenshot showing search results for Storage Account in the main search box.](../media-draft/search-for-sa-small.png)

2. In the list of storage accounts that are returned, select the storage account you used to create the **newmessage** output binding. The storage account settings are displayed in the main window the portal.

3. Select the **Queues** item from the Services list. This displays a list of queues hosted by this storage account. Verify that the **bookmarks-post-process** queue exists, as shown in the following screenshot.

![Screenshot showing our queue in the list of queues hosted by this storage account](../media-draft/q-in-list-small.png)

4. Click on **bookmarks-post-process** to open the queue. The messages that are in the queue are displayed in a list. If all went according to plan, the message we posted when we added a bookmark to our database should be in the queue and will look like the following entry. 

![Screenshot showing our message in the queue](../media-draft/message-in-q-small.png)

In this example, you can see that the message was given a unique ID and the **MESSAGE TEXT** field displays our bookmark in JSON string format.

5. You can test the function further by changing the request body in the Test panel with new id/url sets and running the function. Watch this queue to see more messages arrive. You can also look at the database to verify new entries have been added. 

In this lab, we expanded our knowledge of bindings to output bindings, writing data to our Azure Cosmos DB. We went further and added another output binding to post messages to an Azure queue. This demonstrates the true power of bindings to help you shape and move data from incoming sources to a variety of destinations. We haven't written any database code or had to manage connection strings ourselves. Instead, we configured bindings declaratively and let the platform take care of securing connections, scaling our function and scaling our connections.