You're now ready to configure your publisher and consumer applications for your event hub.

In this unit, you'll configure these applications to send or receive messages through your event hub. These applications are stored in a GitHub repository.

You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**). These are Java applications, which enable you to do everything within the browser. However, the same configuration is needed for any platform, such as .NET.

## Create a general-purpose, standard storage account

The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage. Blob Storage requires a storage account.

1. Create a storage account (general-purpose V2) in the resource group using the following command:

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name>  --location <location> --sku Standard_RAGRS --encryption blob
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--name (required)  |Enter a name for your storage account.|
    |--resource-group (required)  |Enter the resource group you created in the previous unit.|
    |--location (optional)    |Enter the location you used to create your resource group in the previous unit.|

1. List all the access keys associated with your storage account using the following command:

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |--account-name (required)  |Enter the name for your storage account.|
    |--resource-group (required)  |Enter the resource group you created in the previous unit.|

     Access keys associated with your storage account are listed. Copy and save the value of **key** for future use. You'll need this key to access your storage account.

1. View the connections string for your storage account using the following command:

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

    |Parameter      |Description|
    |---------------|-----------|
    |-n (required)  |Enter the name for your storage account.|
    |-g (required)  |Enter the name of your resource group.|

    This command returns the connection details for the storage account. Copy and save the value of **connectionString**.

1. Create a container called **messages** in your storage account using the following command. Use the **connectionString** you copied in the previous step:

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## Clone the Event Hubs GitHub repository

Use the following steps to clone the Event Hubs GitHub repository.

1. Sign in to Azure Cloud Shell (Bash).

1. The source files for the applications that you'll build In this unit are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs). Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    The repository is cloned to `/home/<username>/azure-event-hubs`.

## Use nano to edit SimpleSend.java

Use the **nano** editor to edit the SimpleSend application and add your Event Hubs namespace, event hub name, shared access policy name, and primary key. The main commands are displayed at the bottom of the editor window; in this unit, you'll need to write out your edits using CTRL +O, and then ENTER to confirm the output file name, and exit the editor using CTRL +X.

1. Change to the **SimpleSend** folder using the following command:

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. Open the **SimpleSend.java** file in the **nano** editor using the following command:

    ```azurecli
    nano SimpleSend.java
    ```

1. In the nano editor, locate and replace the following strings:

    - `"Your Event Hubs namespace name"` with the name of your event hub namespace.
    - `"Your event hub"` with the name of your event hub.
    - `"Your primary SAS key"` with the value of the **primaryKey** key for your event hub namespace that you saved earlier.
    - `"Your policy name"` with **RootManageSharedAccessKey**.
 
        When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace. In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.

    ![Configuration details for sender application](../media-draft/5-sender-configure.png)

1. Save **SimpleSend.java** using the following command, and exit nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## Use Maven to build SimpleSend.java

You'll now build the Java application using **mvn** commands.

1. Change to the main **SimpleSend** folder using the following command:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. Build the Java SimpleSend application using the following command. This ensures that your application  uses the connection details for your event hub:

    ```azurecli
    mvn clean package -DskipTests
    ```

    The build process may take several minutes to complete. Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.

    ![Build results for sender application](../media-draft/5-sender-build.png)

## Use nano to edit EventProcessorSample.java

You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your event hub.

For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**. EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver. EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.

In this unit, you’ll use the EventProcessorHost method. You'll again use nano, and edit the EventProcessorSample application to add your Event Hubs namespace, event hub name, shared access policy name and primary key, storage account name, connection string, and container name.

1. Change to the **EventProcessorSample** folder using the following command:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. Open the **EventProcessorSample.java** file in the **nano** editor using the following command:

    ```azurecli
    nano EventProcessorSample.java
    ```
1. Locate and replace the following strings in the nano editor:

    - `----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.
    - `----EventHubName----` with the name of your event hub.
    - `----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.
    - `----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.
    - `----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.
    - `----StorageContainerName----` with **messages**.
    - `----HostNamePrefix----` with the name of your storage account.

    ![Configuration details for receiver application](../media-draft/5-receiver-configure.png)

1. Save **EventProcessorSample.java** using the following command and exit nano:

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## Use Maven to build EventProcessorSample.java

1. Change to the main **EventProcessorSample** folder using the following command:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. Build the Java SimpleSend application using the following command. This ensures that your application uses the connection details for your event hub:

    ```azurecli
    mvn clean package -DskipTests
    ```

    The build process may take several minutes to complete. Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.

    ![Build results for receiver application](../media-draft/5-receiver-build.png)

## Start the sender and receiver apps

1. Run Java application from the command line by using the **java** command, and specifying a .jar package. Use the following commands to start the SimpleSend application:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. When you see **Send Complete...**, press ENTER.

    ![Run results for sender application](../media-draft/5-sender-run.png)

1. Start the EventProcessorSample application using the following command.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. When messages stop being displayed to the console, press ENTER.

    ![Run results for receiver application](../media-draft/5-receiver-run.png)

## Summary

You've now configured a sender application ready to send messages to your event hub. You've also configured a receiver application ready to receive messages from your event hub.
