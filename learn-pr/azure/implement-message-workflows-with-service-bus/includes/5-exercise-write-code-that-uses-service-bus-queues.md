You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.

You've already implemented the necessary objects in your Azure subscription. Now, you want to write code that sends messages to that queue and retrieves messages.

## Clone and open the starter application

In this unit, you'll build two console applications in **Visual Studio Code**. The first application places messages into a Service Bus queue and the second retrieves them. The applications are part of a single .NET Core solution. 

Start by cloning the solution:

1. Start a command prompt and change to the directory where you want to host the source code for the application.

1. Type the following command, and then press **Enter**:

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. When the clone operation is complete, change to the starter folder.

1. Type the following command, and then press **Enter**.

    ```powershell
    code .
    ```

1. If a message appears asking if you want to restore dependencies, click **Yes**.

## Configure a connection string to a Service Bus namespace

In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:

* The endpoint for your namespace
* The shared access key for authentication

Both of these values can be obtained from the Azure portal in the form of a complete connection string.

> [!NOTE]
> For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications. In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.

1. Switch to the Azure portal.

1. In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. Under **SETTINGS**, click **Shared Access Policies**.

1. In the list of policies, click **RootManageSharedAccessKey**.

1. To the right of the **Primary Connection string** text box, click the **Click to copy** button.

1. Switch to **Visual Studio Code**.

1. In the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.

1. Locate the following line of code:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Place the cursor between the quotation marks, and then press **Ctrl+V**.

1. In the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.

1. Locate the following line of code:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Place the cursor between the quotation marks, and then press **Ctrl+V**.

1. Click **File**, and then click **Save All**.

1. Close all open editor windows.

## Write code that sends a message to the queue

To complete the component that sends messages about sales, follow these steps:

1. In Visual Studio Code, in the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.

1. Locate the `SendSalesMessageAsync()` method.

1. Within that method, locate the following line of code:

    ```C#
    // Create a queue client here
    ```

1. To create a queue client, replace that line of code with the following code:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. Within the `try...catch` block, locate the following line of code:

    ```C#
    // Create and send a message here
    ```

1. To create and format a message for the queue, replace that line of code with the following code:

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. To display the message in the console, on the next line, add the following code:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. To send the message to the queue, on the next line, add the following code:

    ```C#
    await queueClient.SendAsync(message);
    ```

1. Locate the following line of code:

    ```C#
    // Close the connection to the queue here
    ```

1. To close the connection the Service Bus, replace that line of code with the following code:

    ```C#
    await queueClient.CloseAsync();
    ```

1. In Visual Studio Code, close all editor windows and save all changed files.

## Send a message to the queue

To run the component that sends a message about a sale, follow these steps:

1. In Visual Studio Code, on the **View** menu, click **Debug**.

1. In the **Debug** pane, in the drop-down list, select **Launch Private Message Sender**, and then press **F5**. Visual Studio Code builds and runs the console application in debugging mode.

1. As the program executes, examine the messages in the **Debug Console**.

1. Switch to the Azure portal.

1. If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue. The **ACTIVE MESSAGE COUNT** should indicate that one message has been added to the queue.

## Write code that receives a message from the queue

To complete the component that retrieves messages about sales, follow these steps:

1. In Visual Studio Code, in the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.

1. Locate the `ReceiveSalesMessageAsync()` method.

1. Within that method, locate the following line of code:

    ```C#
    // Create a queue client here
    ```

1. To create a queue client, replace that line with the following code:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. Locate the `RegisterMessageHandler()` method.

1. To configure message handling options, replace all the code within that method with the following code:

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. To register the message handler, on the next line, add the following code:

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Locate the `ProcessMessagesAsync()` method. You have registered this method as the one that handles incoming messages.

1. To display incoming messages in the console, replace all the code within that method with the following code:

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. To remove the received message from the queue, on the next line, add the following code:

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:

    ```C#
    // Close the queue here
    ```

1. To close the connection to Service Bus, replace that line with the following code:

    ```C#
    await queueClient.CloseAsync();
    ```

1. In Visual Studio Code, close all editor windows and save all changed files.

## Retrieve a message from the queue

To run the component that retrieves a message about a sale, follow these steps:

1. In Visual Studio Code, on the **View** menu, click **Debug**.

1. In the **Debug** pane, in the drop-down list, select **Launch Private Message Receiver**, and then press **F5**. Visual Studio Code builds and runs the console application in debugging mode.

1. As the program executes, examine the messages in the **Debug Console**.

1. When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.

1. Switch to the Azure portal.

1. If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue. The **ACTIVE MESSAGE COUNT** should indicate that the message has been removed from the queue.

You have written code that sends a message about individual sales to a Service Bus queue. In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.

You have also written code that receives a message from the Service Bus queue. In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.
