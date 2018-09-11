Let's start by creating a Redis cache instance in Azure, then create a simple transaction that inserts two data values into the cache.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## Create an Azure Redis Cache

Let's start by creating an Azure Redis Cache with the Azure CLI. Use the Cloud Shell on the right side of the browser window to interact with Azure.

We'll use the `azure rediscache create` command to create a new Azure Redis Cache. It takes several parameters. Here are the most common (to get a full list, check the documentation).

> [!div class="mx-tableFixed"]
> | Parameter | Description |
> |-----------|-------------|
> | `--name`    | The name of the cache - this must be globally unique and composed of letters, numbers and dashes. |
> | `--resource-group` | Use the pre-created Resource Group <rgn>[Sandbox resource group name]</rgn> which is part of the Azure Sandbox. |
> | `--location` | Specify the location where the cache should be located. Normally, you will want to choose a location close to the data consumers. In this case, you are limited to the locations available in the Azure Sandbox. Select the closest one to you. |
> | `--size` | Size of the Redis Cache. Valid values are [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
> | `--sku` | Redis SKU. Valid values are [Basic, Standard, Premium] |
> | `--enable-non-ssl-port` | Add this flag if you want to enable the Non SSL Port for your cache. |

### Selecting a location
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Create a cache using the following options:
    - Size: C0
    - SKU: Basic
    - EnableNonSslPort
    
1. Here's an example command line, make sure to replace the **[name]** and **[location]** with valid values.

    ```bash
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

1. It will take a few minutes to create the cache.

## Create a .NET Core Console application

Next, create a .NET Core C#-based console application, which will be used to insert data values into our Azure Redis Cache.

1. Create a new .NET Core application using the integrated Cloud Shell on the right hand side of the window. Name it "RedisData".

    ```bash
    dotnet new console --name RedisData
    ```
    
1. Change into the new directory created for your app.

    ```bash
    cd RedisData
    ```
    
1. You should find a project file, and a single **Program.cs** source file.

1. Build and run the application - it should output "Hello, World!".

    ```bash
    dotnet run
    ```
    
## Add the ServiceStack.Redis NuGet package

Now that we have our console application, we need to add the **ServiceStack.Redis** NuGet package. This will allow us to connect to the Redis Cache and issue commands in C#.

1. Add the NuGet package **ServiceStack.Redis** using the terminal shell.

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. Build and run the application again to make sure it all compiles. It should still output "Hello, World!"

## Get your Azure Redis Cache connection string

To connect to your Azure Redis Cache, you need a connection string that contains your password and URL. This connection string is unique to **ServiceStack.Redis** and is in the form of: `[password]@[host name]:[port]`

You can retrieve this key with the Azure portal, or with the command line. Let's use the latter here since we used the portal approach in the **Optimize your web applications by caching read-only data with Redis** module.

Use the `azure rediscache list-keys` command to get the access keys. You will need to supply the name and resource group as shown below:

```bash
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

This will return something like

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. Copy your **Primary** key to the clipboard.

1. The hostname will be the name you gave to the cache when you created it with the suffix `.redis.cache.windows.net`.

1. The port should be **6379**.

1. You can verify all of that information in the console with the `azure rediscache list` command which will show you all the Redis cache instances in your active subscription (in this case, the Azure Sandbox).

## Add the connection string to your app

1. Make sure you are in the app folder. The **Program.cs** should be in the current folder if you type `ls` or `dir`.

1. Open the built-in editor by typing `code .` in the app folder.

1. Select the **Program.cs** source file.

1. Create the following field in the `Program` class.

    ```csharp
    static string redisConnectionString = "";
    ```

1. Paste in your connection string in the **redisConnectionString** field you just created and use **6379** as your port number. Remember, your connection string is in the form of: `[password]@[host name]:[port]`

An example connection string would look something like this:

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## Insert two data values into your Azure Redis Cache

Finally, we're going to add data into your Azure Redis Cache.

1. Add the following using statement to the top of the **Program.cs** file.

    ```csharp
    using ServiceStack.Redis;
    ```

1. Add the following snippet of code in your **Main** method. This will add two values transitionally.

    ```csharp
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        if(transactionResult)
            Console.WriteLine("Transaction committed");
        else
            Console.WriteLine("Transaction failed to commit");
    }
    ```
1. Run the application through the command prompt at the bottom of the editor window and verify that the console says **Transaction committed**. 

    ```bash
    dotnet run
    ```
    
## Verify your data

To finish off, let's verify that the data we added is in our Azure Redis Cache.

1. Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Locate your Redis cache by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances. Alternatively, you can use the search box at the top and type the name of the cache.

1. Select your Redis cache instance.

1. In the **Overview** blade for your Redis Cache, select **Console**. This will open a Redis console, which allows you to enter low-level Redis commands.

1. Type **get MyKey1**. Verify that the value returned is **MyValue1**.

1. Type **get MyKey2**. Verify that the value returned is **MyValue2**.

    ![Screenshot of the Azure Redis console showing the values of MyKey1 and MyKey2.](../media/4-redis-console.png)