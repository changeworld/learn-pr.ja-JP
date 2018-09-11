In this unit, you will create a new storage account in your Azure subscription. You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.

These are the same actions taken by components in a distributed application. For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.

## Create a storage account
<!---TODO: Update for sandbox.--->

Since Azure Storage queues are part of Azure general-purpose storage accounts, you must start by creating a storage account:

1. In a browser, navigate to the [Azure portal](https://portal.azure.com?azure-portal=true), and sign in with your normal credentials.

1. In the top left, click **All services**.

1. Scroll down to the **Storage** section, and then click **Storage accounts**.

1. At the top left of the **Storage accounts** blade, click **Add**.

  ![Screenshot of Storage accounts blade, with Add highlighted](../media-draft/4-create-a-storage-account-1.png)

1. In the resulting dialog, enter the following information, each of these options has a `(i)` icon in the portal which you can use to get more information about what the option does.

    - In the **Name** text box, type a unique name for the storage account.
    - Under **Deployment model**, ensure that **Resource Manager** is selected.
    - In the **Account kind** drop-down list, select **Storage (general purpose v2)**.
    - In the **Location** drop-down list, select a region near you.
    - In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.
    - Under **Performance**, select **Standard**.
    - Under **Access tier**, select **Cool**.
    - Under **Secure transfer required**, select **Disabled**.
    - Under **Subscription**, select your subscription.
    - Under **Resource group**, select **Create new**. In the text box, type **MusicSharingResourceGroup**.
    - Under **Virtual networks**, select **Disabled**. 

    ![Screenshot of Create storage account dialog box](../media-draft/4-create-a-storage-account-2.png)

1. Click **Create** - Azure will create a new resource group and a new storage account associated with it.

    ![Screenshot of Create storage account dialog box, with Create highlighted](../media-draft/4-create-a-storage-account-3.png)

## Create a queue

Now that the storage account has been created, you can add a new queue to it. You must create the queue by using PowerShell commands:

1. In the top right of the portal, click the **Cloud Shell** link `(>_)`.

    ![Screenshot of Azure portal, with Cloud Shell icon highlighted](../media-draft/4-create-a-storage-queue-1.png)

1. In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.

1. If the **You have no storage mounted** screen appears, click **Create storage**.

1. When the `PS Azure` prompt appears, to obtain the storage account, type the following command. Substitute `<storageaccountname>` with the unique name of your storage account you created above, and then press **Enter**. We want to assign the resulting object to a variable named `$storageaccount`.

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. Next, we need to get the storage account _context_ - this is a property on the returned object. Let's assign it to another variable named `$context`.

    ```powershell
    $context = $storageaccount.Context
    ```

1. Now we are ready to create the queue. Use the `New-AzureStorageQueue` command and assign it to a `$messageQueue` variable.
    - Pass a `-Name` parameter with the value `musicsharingmessages`
    - Pass a `-Context` parameter with the value you retrieved in the previous step.

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## Add a message to the queue

Now that you have created a queue in the storage account, you can add a message to it.

1. To create a new message, use the `New-Object` method to create a .NET `CloudQueueMessage` with a string-based argument:

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. To add the new message to the new queue, pass the created `CloudQueueMessage` to the `AddMessageAsync` method on your `$messageQueue` queue.

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## Verify the message was queued

We can use the **Storage Explorer** to work with our queue. There are two variations available:

- A cross-platform desktop app for Linux, macOS, and Windows that you can download.
- A preview web version in the Azure portal. This is the one we will use here, but you can install the desktop version if you prefer - the instructions are very similar.

1. In the Azure portal, in the navigation on the left, click **All resources**.

1. In the list of resources, click the storage account you created earlier.

1. In the storage account blade, click **Storage Explorer (Preview)**.

1. In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**. The Storage Explorer should display the message you just added.

## Retrieve and remove the message

A destination component for a message in a Storage queue must retrieve the message at the front of the queue. Then the destination component must process the message and delete it from the queue so that other components do not retrieve it.

1. We can retrieve the first available message in PowerShell using the `GetMessageAsync` method on our queue. This is an asynchronous .NET method, since we want to wait for it we can just use the `Result` property to get the return value. This returns an object representing the message which we can assign to a parameter.

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. We can get a textual version of the message by calling `AsString` - this will output the value on the console.

    ```powershell
    $retrievedMessage.AsString
    ```

1. Or, we can display all the properties of the message by just typing the variable name and pressing **Enter**.

    ```powershell
    $retrievedMessage
    ```

1. `GetMessageAsync` does *not* remove the message - it simply returns it, which means we could process it again. To remove the message from the queue, we can use the `DeleteMessageAsync` method on the queue - this requires that we pass in the message we want to remove.

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. To verify that the message is gone, refresh the queue display in the Azure portal by navigating to the Storage Account blade and selecting **Overview > Storage Explorer**. Under **QUEUES**, click **musicsharingmessages**. The Storage Explorer should now show that the queue is empty because you removed the only message.


## Summary
Storage account queues are a good solution when you want to pass _messages_ between the components of a distributed application. This is a great choice if you want to guarantee delivery or to ensure that messages are delivered in the same order you sent them. However, queues imply that the sender and receiver understand the format of the data being passed - there's an implied data contract between them which adds a bit of "coupling" between the two communicating services.

Not all architectures need to pass formatted blocks of data, some really just need simple messages which we want to fire-and-forget without any knowledge of what will handle the message. In these scenarios, a queue isn't a great choice. Let's look at another messaging strategy which is more suited to this style of communication.