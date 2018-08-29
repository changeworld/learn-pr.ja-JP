<!--TODO: explain Etag in knowledge needed-->
<!--TODO: Update to weave in the online retailer story-->


<span data-ttu-id="fd68a-101">データは、Azure Cosmos DB 内の JSON ドキュメントに格納されます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-101">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="fd68a-102">前のモジュールで示したようにポータルで、またはこのモジュールで説明するようにプログラムを使用して、[ドキュメント](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents)を作成、取得、置換、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-102">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="fd68a-103">Azure Cosmos DB では .NET、.NET Core、Java、Node.js、Python 用のクライアント側 SDK が提供されており、いずれもこれらの操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fd68a-103">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="fd68a-104">このモジュールでは、.NET Core SDK を使用して CRUD (作成、取得、更新、削除) 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-104">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations.</span></span> 

<span data-ttu-id="fd68a-105">Azure Cosmos DB ドキュメントに対する主要な操作は、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) クラスの一部です。</span><span class="sxs-lookup"><span data-stu-id="fd68a-105">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="fd68a-106">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="fd68a-106">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="fd68a-107">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="fd68a-107">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="fd68a-108">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="fd68a-108">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="fd68a-109">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="fd68a-109">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="fd68a-110">アップサートでは、ドキュメントが既に存在するかどうかに応じて、作成または置換操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-110">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="fd68a-111">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="fd68a-111">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="fd68a-112">これらの操作を実行するには、データベースに格納されているオブジェクトを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd68a-112">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="fd68a-113">ここではユーザーのデータベースを操作しているので、ユーザー名やユーザー ID (水平スケーリングを有効にするためのパーティション キーであるため必須です) などの主要なデータを格納するための **User** クラスと、出荷方法や注文履歴のためのサブクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-113">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their name and UserId (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="fd68a-114">ユーザーを表すこれらのクラスを作成した後、インスタンスごとに新しいユーザー ドキュメントを作成し、ドキュメントに対していくつかのシンプルな CRUD 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-114">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="fd68a-115">ドキュメントの作成</span><span class="sxs-lookup"><span data-stu-id="fd68a-115">Create documents</span></span>

1. <span data-ttu-id="fd68a-116">最初に、Azure Cosmos DB に格納するオブジェクトを表す **User** クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-116">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="fd68a-117">また、**User** の内部で使用される **OrderHistory** および **ShippingPreference** サブクラスも作成します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-117">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="fd68a-118">ドキュメントには、JSON で **id** としてシリアル化される **Id** プロパティが必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fd68a-118">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> 

    <span data-ttu-id="fd68a-119">これらのクラスを作成するには、以下の **User**、**OrderHistory**、**ShippingPreference** クラスをコピーして、**BasicOperations** メソッドの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-119">To create these classes, copy and paste the following **User**, **OrderHistory**, **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

    <!--TODO: specify JSON for all of these? -->
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
            public OrderHistory[] OrderHistory { get; set; }
            public ShippingPreference[] ShippingPreference { get; set; }
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

2. <span data-ttu-id="fd68a-120">統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-120">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

3. <span data-ttu-id="fd68a-121">次に、**CreateUserDocumentIfNotExists** タスクをコピーして、**ShippingPreference** クラスの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-121">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **ShippingPreference** class.</span></span>

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found {0}", user.Id);
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

4. <span data-ttu-id="fd68a-122">そして、次のコードを **BasicOperations** メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-122">Then add the following to the **BasicOperations** method.</span></span>

    ```csharp
     User yanhe = new User
                {
                    Id = "1",
                    UserId = "yanhe",
                    LastName = "He",
                    FirstName = "Yan",
                    Email = "yanhe@todo.com",
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
                    Email = "nelapin@todo.com",
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

5. <span data-ttu-id="fd68a-123">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-123">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="fd68a-124">両方のユーザー レコードが正常に作成されたことを示す次の出力が、ターミナルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-124">The terminal displays the following output, indicating that both user records were successfully created.</span></span> 

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="fd68a-125">ドキュメントを読み取る</span><span class="sxs-lookup"><span data-stu-id="fd68a-125">Read documents</span></span>

1. <span data-ttu-id="fd68a-126">データベースからドキュメントを読み取るには、次のコードをコピーし、Program.cs ファイルの末尾に配置します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-126">To read documents from the database, copy in the following code and place it at the end of the Program.cs file.</span></span>

    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found user {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

2.  <span data-ttu-id="fd68a-127">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-127">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

4. <span data-ttu-id="fd68a-128">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-128">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="fd68a-129">ターミナルに次の出力が表示されます。"Found user 1" という出力は、ドキュメントが見つかったことを示します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-129">The terminal displays the following output, where the output "Found user 1" indicates the document was found.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="fd68a-130">ドキュメントを置換する</span><span class="sxs-lookup"><span data-stu-id="fd68a-130">Replace documents</span></span>
<span data-ttu-id="fd68a-131">Azure Cosmos DB は、JSON ドキュメントの置換をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fd68a-131">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="fd68a-132">ここでは、姓の変更を反映するようにユーザー レコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-132">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="fd68a-133">**ReplaceFamilyDocument** メソッドをコピーし、**CreateUserDocumentIfNotExists** メソッドの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-133">Copy and paste the **ReplaceFamilyDocument** method underneath your **CreateUserDocumentIfNotExists** method.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, string LastName, User updatedUser)
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, LastName), updatedUser);
        this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", LastName);
    }
    ```

2. <span data-ttu-id="fd68a-134">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-134">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe.LastName, yanhe);
    ```
    <!--TODO: need to fix this as it's giving an error-->

4. <span data-ttu-id="fd68a-135">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-135">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="fd68a-136">ターミナルに次の出力が表示されます。"Replaced last name for Suh" という出力は、ドキュメントが置き換えられたことを示します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-136">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    Replaced last name for Suh 
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="fd68a-137">ドキュメントの削除</span><span class="sxs-lookup"><span data-stu-id="fd68a-137">Delete documents</span></span>

1. <span data-ttu-id="fd68a-138">**DeleteUserDocument** メソッドをコピーし、**ReplaceUserDocument** メソッドの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-138">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, string documentName)
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName), new RequestOptions { PartitionKey = new PartitionKey("User.UserId") });
        Console.WriteLine("Deleted User {0}", documentName);
    }
    ```

2. <span data-ttu-id="fd68a-139">次のコードをコピーし、**BasicOperations** メソッドの 2 回目のクエリ実行のすぐ下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fd68a-139">Copy and paste the following code to your **BasicOperations** method underneath the second query execution.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", "1");
    ```

3. <span data-ttu-id="fd68a-140">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fd68a-140">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="fd68a-141">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="fd68a-141">Congratulations!</span></span> <span data-ttu-id="fd68a-142">これで、Azure Cosmos DB ドキュメントが削除されました。</span><span class="sxs-lookup"><span data-stu-id="fd68a-142">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <a name="summary"></a><span data-ttu-id="fd68a-143">まとめ</span><span class="sxs-lookup"><span data-stu-id="fd68a-143">Summary</span></span>

<span data-ttu-id="fd68a-144">このユニットでは、Azure Cosmos DB データベース内のドキュメントを作成、置換、削除しました。</span><span class="sxs-lookup"><span data-stu-id="fd68a-144">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>