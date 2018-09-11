In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.

## Create a blob trigger

Again, let's continue using our existing Azure Functions application and add a blob trigger.

1. Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Point to **Functions** and select the plus (+) icon.

1. Select **Custom Function** and then **Blob trigger**.

1. Select **C#** as the language.

1. Leave the **Name** set to the default value.

1. Leave the **Path** set to the default value.

1. Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.

## Create a blob container

Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.

1. Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name. We want to use a new tab so we can switch between the two services we are working with.

1. Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.

Remember that our blob trigger is monitoring only the location described in the **Path** field. By default, our path should be:

> samples-workitems/{name}

We need to create a container called **samples-workitems**.

1. Right-click **BLOB CONTAINERS** and select **Create blob container**.

1. Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.

## Turn on your blob trigger

Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.

1. Switch back to the Azure Function tab (or reopen it).

1. Select your blob trigger to open the code screen.

1. Select **Run** - this will open the output window.

## Create a blob

Our blob trigger is now up and listening for activity. Let's create a blob to see if we get a log message.

1. In Storage explorer, select the **samples-workitems** container.

1. Select **Upload** from the toolbar.

1. Select any file from your computer.

1. Select **Upload**.

1. Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.

## Pause the Function

To ensure that you aren't charged for additional requests, you can click **Pause** above the log window.
