<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="1d04c-101">Azure Cosmos DB への接続が完了した後、次の手順としてデータベースに保存された文書の作成、読み込み、置換、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="1d04c-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace, and delete the documents that are stored in the database.</span></span> <span data-ttu-id="1d04c-102">このユニットでは、WebCustomer コレクション内にユーザー ドキュメントを作成します。その後、ID を使った読み込み、置換、および削除を行います。</span><span class="sxs-lookup"><span data-stu-id="1d04c-102">In this unit, you will create User documents in your WebCustomer collection, then you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="1d04c-103">プログラムによるドキュメントの操作</span><span class="sxs-lookup"><span data-stu-id="1d04c-103">Working with documents programmatically</span></span>

<span data-ttu-id="1d04c-104">データは、Azure Cosmos DB 内の JSON ドキュメントに格納されます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-104">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="1d04c-105">前のモジュールで示したようにポータルで、またはこのモジュールで説明するようにプログラムを使用して、[ドキュメント](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents)を作成、取得、置換、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-105">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="1d04c-106">Azure Cosmos DB では .NET、.NET Core、Java、Node.js、Python 用のクライアント側 SDK が提供されており、いずれもこれらの操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1d04c-106">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="1d04c-107">このモジュールでは、.NET Core SDK を使用して CRUD (作成、取得、更新、削除) 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-107">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations.</span></span> 

<span data-ttu-id="1d04c-108">Azure Cosmos DB ドキュメントに対する主要な操作は、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) クラスの一部です。</span><span class="sxs-lookup"><span data-stu-id="1d04c-108">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="1d04c-109">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="1d04c-109">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="1d04c-110">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="1d04c-110">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="1d04c-111">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="1d04c-111">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="1d04c-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="1d04c-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="1d04c-113">アップサートでは、ドキュメントが既に存在するかどうかに応じて、作成または置換操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-113">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="1d04c-114">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="1d04c-114">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="1d04c-115">これらの操作を実行するには、データベースに格納されているオブジェクトを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d04c-115">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="1d04c-116">ユーザーのデータベースを使って作業を行っているため、ユーザーの姓名ならびにユーザー ID（水平スケーリングを可能とするためのパーティション キーとなるため、必須です）などのプライマリ データならびに発送用情報や注文履歴のためのサブクラスを格納するための **User** クラスの作成を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d04c-116">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name, and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="1d04c-117">ユーザーを表すこれらのクラスを作成した後、インスタンスごとに新しいユーザー ドキュメントを作成し、ドキュメントに対していくつかのシンプルな CRUD 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-117">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="1d04c-118">ドキュメントの作成</span><span class="sxs-lookup"><span data-stu-id="1d04c-118">Create documents</span></span>

1. <span data-ttu-id="1d04c-119">最初に、Azure Cosmos DB に格納するオブジェクトを表す **User** クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-119">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="1d04c-120">また、**User** の内部で使用される **OrderHistory** および **ShippingPreference** サブクラスも作成します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-120">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="1d04c-121">ドキュメントには、JSON で **id** としてシリアル化される **Id** プロパティが必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1d04c-121">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="1d04c-122">これらのクラスを作成するためには、**BasicOperations** メソッド下にある **User**、**OrderHistory**、および **ShippingPreference** の各クラスをコピーしてペーストします。</span><span class="sxs-lookup"><span data-stu-id="1d04c-122">To create these classes, copy and paste the following **User**, **OrderHistory**, and **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

    ```csharp
    public class User
    {
            [JsonProperty("id")]
            public string Id { get; set; }
            [JsonProperty("userId")]
            public string UserId { get; set; }
            [JsonProperty("lastName")]
            public string LastName { get; set; }
            [JsonProperty("firstName")]
            public string FirstName { get; set; }
            [JsonProperty("email")]
            public string Email { get; set; }
            [JsonProperty("dividend")]
            public string Dividend { get; set; }
            [JsonProperty("OrderHistory")]
            public OrderHistory[] OrderHistory { get; set; }
            [JsonProperty("ShippingPreference")]
            public ShippingPreference[] ShippingPreference { get; set; }
            [JsonProperty("CouponsUsed")]
            public CouponsUsed[] Coupons { get; set; }
            public override string ToString()
            {
            return JsonConvert.SerializeObject(this);
            }
    }
    
    public class OrderHistory
    {
            public string OrderId { get; set; }
            public string DateShipped { get; set; }
            public string Total { get; set; }
    }
    
    public class ShippingPreference
    {
            public int Priority { get; set; }
            public string AddressLine1 { get; set; }
            public string AddressLine2 { get; set; }
            public string City { get; set; }
            public string State { get; set; }
            public string ZipCode { get; set; }
            public string Country { get; set; }
    }
    
    public class CouponsUsed
    {
            public string CouponCode { get; set; }
    
    }
    
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }
    ```

1. <span data-ttu-id="1d04c-123">統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-123">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="1d04c-124">次に、**CreateUserDocumentIfNotExists** タスクをコピーして、**ShippingPreference** クラスの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-124">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **ShippingPreference** class.</span></span>

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
        {
            try
            {
                await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
                this.WriteToConsoleAndPromptToContinue("User {0} already exists in the database", user.Id);
            }
            catch (DocumentClientException de)
            {
             if (de.StatusCode == HttpStatusCode.NotFound)
                {
                    await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), user);
                    this.WriteToConsoleAndPromptToContinue("Created User {0}", user.Id);
                }
                else
                {
                    throw;
                }
            }
        }
    ```

1. <span data-ttu-id="1d04c-125">そして、次のコードを **BasicOperations** メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-125">Then add the following to the **BasicOperations** method.</span></span>

    ```csharp
     User yanhe = new User
                {
                    Id = "1",
                    UserId = "yanhe",
                    LastName = "He",
                    FirstName = "Yan",
                    Email = "yanhe@contoso.com",
                    OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1000",
                    DateShipped = "08/17/2018",
                    Total = "52.49"
                }
            },
                    ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "90 W 8th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
                };
    
                await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", yanhe);
    
                User nelapin = new User
                {
                    Id = "2",
                    UserId = "nelapin",
                    LastName = "Pindakova",
                    FirstName = "Nela",
                    Email = "nelapin@contoso.com",
                    Dividend = "8.50",
                    OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1001",
                    DateShipped = "08/17/2018",
                    Total = "105.89"
                }
            },
                    ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "505 NW 5th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    },
                    new ShippingPreference {
                            Priority = 2,
                            AddressLine1 = "505 NW 5th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
                    Coupons = new CouponsUsed[]
             {
                 new CouponsUsed{
                     CouponCode = "Fall2018"
                 }
             }
                };
    
                await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);
    ```

1. <span data-ttu-id="1d04c-126">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-126">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="1d04c-127">両方のユーザー レコードが正常に作成されたことを示す次の出力が、ターミナルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-127">The terminal displays the following output, indicating that both user records were successfully created.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="1d04c-128">ドキュメントを読み取る</span><span class="sxs-lookup"><span data-stu-id="1d04c-128">Read documents</span></span>

1. <span data-ttu-id="1d04c-129">データベースからドキュメントを読み取るには、次のコードをコピーし、Program.cs ファイルの末尾に配置します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-129">To read documents from the database, copy in the following code and place it at the end of the Program.cs file.</span></span>
    
    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("Read user {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not read", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

1.  <span data-ttu-id="1d04c-130">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-130">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="1d04c-131">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-131">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="1d04c-132">「Read user 1」の出力によって文書の読み込みが行われた状態で、ターミナルは次の出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-132">The terminal displays the following output, where the output "Read user 1" indicates the document was retrieved.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="1d04c-133">ドキュメントを置換する</span><span class="sxs-lookup"><span data-stu-id="1d04c-133">Replace documents</span></span>

<span data-ttu-id="1d04c-134">Azure Cosmos DB は、JSON ドキュメントの置換をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1d04c-134">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="1d04c-135">ここでは、姓の変更を反映するようにユーザー レコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-135">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="1d04c-136">**ReplaceFamilyDocument** メソッドをコピーし、Program.cs ファイルの末尾に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-136">Copy and paste the **ReplaceFamilyDocument** method at the end of the Program.cs file.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
            try
            {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) } );
            this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser.LastName);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found for replacement", updatedUser.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

1. <span data-ttu-id="1d04c-137">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-137">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="1d04c-138">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-138">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="1d04c-139">ターミナルに次の出力が表示されます。"Replaced last name for Suh" という出力は、ドキュメントが置き換えられたことを示します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-139">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="1d04c-140">ドキュメントの削除</span><span class="sxs-lookup"><span data-stu-id="1d04c-140">Delete documents</span></span>

1. <span data-ttu-id="1d04c-141">**DeleteUserDocument** メソッドをコピーし、**ReplaceUserDocument** メソッドの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-141">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, User deletedUser)
    {
        try
        {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, deletedUser.Id), new RequestOptions { PartitionKey = new PartitionKey(deletedUser.UserId) });
        Console.WriteLine("Deleted user {0}", deletedUser.Id);
        }
        catch (DocumentClientException de)
        {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            this.WriteToConsoleAndPromptToContinue("User {0} not found for deletion", deletedUser.Id);
        }
        else
        {
            throw;
        }
        }
    }
    ```

1. <span data-ttu-id="1d04c-142">次のコードをコピーし、**BasicOperations** メソッドの 2 回目のクエリ実行のすぐ下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="1d04c-142">Copy and paste the following code to your **BasicOperations** method underneath the second query execution.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="1d04c-143">統合ターミナルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-143">In the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="1d04c-144">「Read user 1」の出力によって文書の削除が行われた状態で、ターミナルは次の出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="1d04c-144">The terminal displays the following output, where the output "Deleted user 1" indicates the document was deleted.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="1d04c-145">まとめ</span><span class="sxs-lookup"><span data-stu-id="1d04c-145">Summary</span></span>

<span data-ttu-id="1d04c-146">このユニットでは、Azure Cosmos DB データベース内のドキュメントを作成、置換、削除しました。</span><span class="sxs-lookup"><span data-stu-id="1d04c-146">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>