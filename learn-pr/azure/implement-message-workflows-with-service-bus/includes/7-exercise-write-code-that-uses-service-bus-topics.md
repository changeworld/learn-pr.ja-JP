You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application. The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period. Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.

You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions. Now, you want to write the code that sends messages to the topic and retrieves messages from each subscription.

## Configure a connection string to a Service Bus namespace

Start by configuring connection strings both in the sending and receiving components:

1. Switch to the Azure portal.

1. In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. Under **SETTINGS**, click **Shared Access Policies**.

1. In the list of policies, click **RootManageSharedAccessKey**.

1. To the right of the **Primary Connection string** text box, click the **Click to copy** button.

1. Switch to **Visual Studio Code**.

1. In the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.

1. Locate the following line of code:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Place the cursor between the quotation marks, and then press **Ctrl+V**.

1. In the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.

1. Locate the following line of code:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Place the cursor between the quotation marks, and then press **Ctrl+V**.

1. Click **File**, and then click **Save All**.

1. Close all open editor windows.

## Write code that sends a message to the topic

To complete the component that sends messages about sales performance, follow these steps:

1. In Visual Studio Code, in the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.

1. Locate the `SendPerformanceMessageAsync()` method.

1. Within that method, locate the following line of code:

    ```C#
    // Create a Topic Client here
    ```

1. To create a topic client, replace that line of code with the following code:

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. Within the `try...catch` block, locate the following line of code:

    ```C#
    // Create and send a message here
    ```

1. To create and format a message for the queue, replace that line of code with the following code:

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. To display the message in the console, on the next line, add the following code:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. To send the message to the queue, on the next line, add the following code:

    ```C#
    await topicClient.SendAsync(message);
    ```

1. Locate the following line of code:

    ```C#
    // Close the connection to the topic here
    ```

1. To close the connection to Service Bus, replace that line of code with the following code:

    ```C#
    await topicClient.CloseAsync();
    ```

1. In Visual Studio Code, close all editor windows and save all changed files.

## Send a message to the topic

To run the component that sends a message about a sale, follow these steps:

1. In Visual Studio Code, on the **View** menu, click **Debug**.

1. In the **Debug** pane, in the drop-down list, select **Launch Performance Message Sender**, and then press **F5**. Visual Studio Code builds and runs 
the console application in debugging mode.

1. As the program executes, examine the messages in the **Debug Console**.

1. Switch to the Azure portal.

1. If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic. In the list of subscriptions, there should be one message displayed in both the **Americas** and **Europe** subscriptions.

## Write code that receives a message from a topic subscription

To complete the component that retrieves messages about sales performance, follow these steps:

1. In Visual Studio Code, in the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.

1. Locate the `MainAsync()` method.

1. Within that method, locate the following line of code:

    ```C#
    // Create a subscription client here
    ```

1. To create a subscription client, replace that line with the following code:

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
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
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Locate the `ProcessMessagesAsync()` method. You have registered this method as the one that handles incoming messages.

1. To display incoming messages in the console, replace all the code within that method with the following code:

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. To remove the received message from the subscription, on the next line, add the following code:

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Return to the `MainAsync()` method and locate the following line of code:

    ```C#
    // Close the subscription here
    ```

1. To close the connection to Service Bus, replace that code with the following code:

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. In Visual Studio Code, close all editor windows and save all changed files.

## Retrieve a message from a topic subscription

To run the component that retrieves a message about sales performance, follow these steps:

1. In Visual Studio Code, on the **View** menu, click **Debug**.

1. In the **Debug** pane, in the drop-down list, select **Launch Performance Message Receiver**, and then press **F5**. Visual Studio Code builds and runs the console application in debugging mode.

1. As the program executes, examine the messages in the **Debug Console**.

1. When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.

1. Switch to the Azure portal.

1. If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.

1. In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic. In the list of subscriptions, there should be zero messages displayed in the **Americas** subscription because your application has processed and removed the only message. Notice that the message is still present in the **Europe** subscription.
