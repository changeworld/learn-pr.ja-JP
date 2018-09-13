<span data-ttu-id="76d2b-101">このユニットでは、構成から Azure ストレージ アカウントの接続文字列を読み込み、その接続文字列を使用して Azure ストレージ アカウントに接続します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-101">In this unit, you'll load the Azure storage account connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="76d2b-102">接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="76d2b-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="76d2b-103">前のユニットからのコンソール アプリケーションが確実に Visual Studio に読み込まれているようにします。</span><span class="sxs-lookup"><span data-stu-id="76d2b-103">Ensure the console application from the previous unit is loaded into Visual Studio.</span></span>

1. <span data-ttu-id="76d2b-104">**Program.cs** ファイルでは、**Microsoft.WindowsAzure.Storage** ライブラリを参照するために、ファイルの上部で *using* ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-104">In the **Program.cs** file, add a *using* statement at the top of the file to reference the **Microsoft.WindowsAzure.Storage** library:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="76d2b-105">**Main** メソッドの下部に、次の行を追加して、構成ファイルから Azure ストレージ アカウントの接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-105">At the bottom of the **Main** method, add the following line to retrieve the Azure storage account connection string from the configuration file:</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="76d2b-106">BLOB クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="76d2b-106">Create a blob client</span></span>

1. <span data-ttu-id="76d2b-107">**Main** メソッドの下部に次のコードを挿入して、接続文字列を解析し、BLOB クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-107">Insert the following code at the bottom of the **Main** method to parse the connection string and create a blob client:</span></span>

    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString,out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="76d2b-108">次のコードを挿入し、BLOB クライアントを作成してコンテナーへの参照を取得します。**Main** メソッドの下部に存在しない場合は、必要に応じて作成します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-108">Insert the following code to create a blob client and retrieve a reference to a container, optionally creating it if it does not exist at the bottom of the **Main** method:</span></span>

    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

  <span data-ttu-id="76d2b-109">***async** メソッドの **CreateIfNotExistsAsync()** と対応する **Result** プロパティの使用に注意してください。一般的なアプリケーションでは、通常、\*\*await*\* キーワードを使用します。ただし、このアプリケーションはコンソール アプリケーションであり、非同期ではないため、**Result** プロパティを呼び出して **CreateIfNotExistsAsync()** メソッドによって作成された非同期タスクの結果を取得する必要があります。\*</span><span class="sxs-lookup"><span data-stu-id="76d2b-109">*Note the use of the **async** method **CreateIfNotExistsAsync()** and corresponding **Result** property. In a typical application, we would normally use the **await** keyword. However, this application is a console application and is not async, so we need to call the **Result** property to get the results of the async task created by the **CreateIfNotExistsAsync()** method.*</span></span>

## <a name="print-a-status-message"></a><span data-ttu-id="76d2b-110">状態メッセージを出力する</span><span class="sxs-lookup"><span data-stu-id="76d2b-110">Print a status message</span></span>

1. <span data-ttu-id="76d2b-111">**Main** メソッドの下部に次のコードを追加して、成功または失敗のメッセージを出力します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-111">Add the following code at the bottom of the **Main** method to print a success or failure message.</span></span>

    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```
1. <span data-ttu-id="76d2b-112">最後に、アプリケーションを実行して接続が成功したことを確認し、Azure portal を表示して、以前に存在していなかった場合は、Azure Storage コンテナーが作成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="76d2b-112">Finally, run the application to verify that a successful connection is made, and view the Azure portal to ensure the Azure Storage container is created if it did not exist previously.</span></span>

1. <span data-ttu-id="76d2b-113">**省略可能**: コンテナーを使用する計画がない場合は、コンテナーが配置されているリソース グループを削除し、確実に使用していないリソースに対して請求されないようにします。</span><span class="sxs-lookup"><span data-stu-id="76d2b-113">**Optional**: If you don't plan on using the container, delete the resource group where the container is located to ensure you are not charged for resources you are not using.</span></span>
