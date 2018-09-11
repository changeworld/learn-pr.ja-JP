Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message. We would typically place this code in our front-end apps that generate the data.

## Add the client library for Azure Storage

Let's start by adding the **Azure Storage Client Library for .NET** to our app. It can be installed with NuGet (a .NET package manager). 

> [!NOTE]
> Even though a new Cloud Shell was created, your files are all still present. However, you will need to switch into the `QueueApp` folder since you will now be at the root of your storage area. Just type `cd QueueApp` to switch folders. Make sure to do this before attempting to add the package since the .NET app is in that folder.

1. Change the current directory to the `QueueApp` folder.

1. Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.

```azurecli
dotnet add package WindowsAzure.Storage
```

## Add a method to send a news alert

Next, let's create a new method to send a news story into a queue.

1. Open the code editor by typing `code .`

1. Open the `Program.cs` file.

1. At the top of the file, add the following namespaces. We'll be using types from both of these to connect to Azure Storage and then to work with queues.

    - System.threading.task
    - Microsoft.WindowsAzure.Storage
    - Microsoft.WindowsAzure.Storage.Queue

1. Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`. We'll use this method to send a news article in to our service. Name the input parameter `newsMesasage` as shown below.

    ```csharp
    ...
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string). This method returns a `CloudStorageAccount` object that needs to be stored in a variable.

1. Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.

1. Next, call `GetQueueReference` method on the client object and pass the string `"newsqueue"` for the queue name. This returns a `CloudQueue` object that we can use to work with the queue. It's OK if the queue does not exist yet.

1. Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use. This will create the queue if necessary.
    - Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it. That also means you need to decorate the _method_ with the `async` keyword. 
    - `CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists. Output a message to the console if we created the queue.
    - Here's an example if you need some help.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. Create an instance of a `CloudQueueMessage`. 
    - It takes a `string` parameter, pass in the method parameter (`newsMessage`). This will be the _body_ of the message. There is also a static method named  that can create a binary message payload.
    

1. Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue. This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.

1. Save the file and build it by typing `dotnet build` into the command window. Fix any errors, you can use the following code to check your work.

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference(QueueName);
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## Add code to send a message

Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.

1. Locate the `Main` method.

1. Check the passed `args` parameter to see if any data was passed to the command line. If so, use `String.Join` to create a single string from all the words using a space as the separator.

1. Pass that to the new `SendArticleAsync` method. 

1. Once it returns, use `Console.WriteLine` to output the message we sent.

1. Save the file and build the program (`dotnet build`), you can use the code below to check your work.

> [!NOTE]
> Since our method is technically asynchronous, we normally would want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.2 (it's a new feature). To ensure we can use this on older versions of the language, just call `Wait()` on the method to actually block waiting for the method to return.

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## Execute the application

The application can now send messages. To test it, you can run it from the command line with the `dotnet run` command. Any additional strings are passed as parameters to the application. As an example, you can type:

    ```azurecli
    dotnet run Send this message
    ```

> [!WARNING]
> Make sure you have saved all the files in the online editor before you build and run the program.

This should add the string `"Send this message"` into the queue.

## Check your results

You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.

Alternatively, you can use the Azure CLI or PowerShell. Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

This should dump the information for your message, which will look something like this:

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.

> [!TIP]
> Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!

Now that we have sent a message, the last step is to add support to _receive_ the message.