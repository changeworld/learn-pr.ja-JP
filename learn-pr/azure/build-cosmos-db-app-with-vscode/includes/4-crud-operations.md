<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="6d7fb-101">Azure Cosmos DB への接続が完了した後、次の手順としてデータベースに保存された文書の作成、読み込み、置換、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace, and delete the documents that are stored in the database.</span></span> <span data-ttu-id="6d7fb-102">このユニットでは、WebCustomer コレクションでユーザー ドキュメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-102">In this unit, you will create User documents in your WebCustomer collection.</span></span> <span data-ttu-id="6d7fb-103">次に、ID でドキュメントを取得し、置換し、削除します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-103">Then, you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="6d7fb-104">プログラムによるドキュメントの操作</span><span class="sxs-lookup"><span data-stu-id="6d7fb-104">Working with documents programmatically</span></span>

<span data-ttu-id="6d7fb-105">データは、Azure Cosmos DB 内の JSON ドキュメントに格納されます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-105">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="6d7fb-106">前のモジュールで示したようにポータルで、またはこのモジュールで説明するようにプログラムを使用して、[ドキュメント](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents)を作成、取得、置換、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-106">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="6d7fb-107">Azure Cosmos DB では .NET、.NET Core、Java、Node.js、Python 用のクライアント側 SDK が提供されており、いずれもこれらの操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-107">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="6d7fb-108">このモジュールでは、.NET Core SDK を使用して、Azure Cosmos DB に格納されている NoSQL データに対して CRUD (作成、取得、更新、削除) 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-108">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations on the NoSQL data stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="6d7fb-109">Azure Cosmos DB ドキュメントに対する主要な操作は、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) クラスの一部です。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-109">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="6d7fb-110">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="6d7fb-110">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="6d7fb-111">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="6d7fb-111">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="6d7fb-112">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="6d7fb-112">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="6d7fb-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="6d7fb-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="6d7fb-114">アップサートでは、ドキュメントが既に存在するかどうかに応じて、作成または置換操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-114">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="6d7fb-115">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="6d7fb-115">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="6d7fb-116">これらの操作を実行するには、データベースに格納されているオブジェクトを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-116">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="6d7fb-117">ユーザーのデータベースを使って作業を行っているため、ユーザーの姓名ならびにユーザー ID（水平スケーリングを可能とするためのパーティション キーとなるため、必須です）などのプライマリ データならびに発送用情報や注文履歴のためのサブクラスを格納するための **User** クラスの作成を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-117">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name, and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="6d7fb-118">ユーザーを表すこれらのクラスを作成した後、インスタンスごとに新しいユーザー ドキュメントを作成し、ドキュメントに対していくつかのシンプルな CRUD 操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-118">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="6d7fb-119">ドキュメントの作成</span><span class="sxs-lookup"><span data-stu-id="6d7fb-119">Create documents</span></span>

1. <span data-ttu-id="6d7fb-120">最初に、Azure Cosmos DB に格納するオブジェクトを表す **User** クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-120">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="6d7fb-121">また、**User** の内部で使用される **OrderHistory** および **ShippingPreference** サブクラスも作成します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-121">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="6d7fb-122">ドキュメントには、JSON で **id** としてシリアル化される **Id** プロパティが必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-122">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="6d7fb-123">これらのクラスを作成するためには、**BasicOperations** メソッド下にある **User**、**OrderHistory**、および **ShippingPreference** の各クラスをコピーしてペーストします。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-123">To create these classes, copy and paste the following **User**, **OrderHistory**, and **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

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

1. <span data-ttu-id="6d7fb-124">統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが確実に実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-124">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="6d7fb-125">次に **CreateUserDocumentIfNotExists** をコピーし、Program.cs ファイルの終わりにある **WriteToConsoleAndPromptToContinue** 関数の下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-125">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **WriteToConsoleAndPromptToContinue** function at the end of the Program.cs file.</span></span>

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

1. <span data-ttu-id="6d7fb-126">次に **BasicOperations** メソッドに戻り、そのメソッドの末尾に次を追加します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-126">Then, return to the **BasicOperations** method and add the following to the end of that method.</span></span>

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

1. <span data-ttu-id="6d7fb-127">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-127">In the integrated terminal, again, type the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="6d7fb-128">アプリケーションで新しいユーザー ドキュメントが作成されるたびに、ターミナルに出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-128">The terminal will display output as the application creates each new user document.</span></span> <span data-ttu-id="6d7fb-129">任意のキーを押してプログラムを完了します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-129">Press any key to complete the program.</span></span>

    ```output
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="6d7fb-130">ドキュメントを読み取る</span><span class="sxs-lookup"><span data-stu-id="6d7fb-130">Read documents</span></span>

1. <span data-ttu-id="6d7fb-131">データベースからドキュメントを読み取るには、次のコードをコピーし、Program.cs ファイルの WriteToConsoleAndPromptToContinue メソッドの後に配置します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-131">To read documents from the database, copy in the following code and place after the WriteToConsoleAndPromptToContinue method in the Program.cs file.</span></span>

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

1. <span data-ttu-id="6d7fb-132">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-132">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="6d7fb-133">統合ターミナルで、次のコマンドを入力してプログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-133">In the integrated terminal, type the following command to run the program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="6d7fb-134">ターミナルに次の出力が表示されます。"Read user 1" という出力は、ドキュメントが取得されたことを示します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-134">The terminal displays the following output, where the output "Read user 1" indicates the document was retrieved.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="6d7fb-135">ドキュメントを置換する</span><span class="sxs-lookup"><span data-stu-id="6d7fb-135">Replace documents</span></span>

<span data-ttu-id="6d7fb-136">Azure Cosmos DB は、JSON ドキュメントの置換をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-136">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="6d7fb-137">ここでは、姓の変更を反映するようにユーザー レコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-137">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="6d7fb-138">**ReplaceFamilyDocument** メソッドをコピーし、Program.cs ファイルの ReadUserDocument メソッドの後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-138">Copy and paste the **ReplaceFamilyDocument** method after the ReadUserDocument method in the Program.cs file.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) });
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

1. <span data-ttu-id="6d7fb-139">次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-139">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="6d7fb-140">統合ターミナルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-140">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```
    <span data-ttu-id="6d7fb-141">ターミナルに次の出力が表示されます。"Replaced last name for Suh" という出力は、ドキュメントが置き換えられたことを示します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-141">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="6d7fb-142">ドキュメントの削除</span><span class="sxs-lookup"><span data-stu-id="6d7fb-142">Delete documents</span></span>

1. <span data-ttu-id="6d7fb-143">**DeleteUserDocument** メソッドをコピーし、**ReplaceUserDocument** メソッドの下に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-143">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>

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

1. <span data-ttu-id="6d7fb-144">次のコードをコピーし、**BasicOperations** メソッドの後に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-144">Copy and paste the following code in the end of the **BasicOperations** method.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="6d7fb-145">統合ターミナルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-145">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="6d7fb-146">ターミナルに次の出力が表示されます。"Deleted user 1" という出力は、ドキュメントが削除されたことを示します。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-146">The terminal displays the following output, where the output "Deleted user 1" indicates the document was deleted.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

<span data-ttu-id="6d7fb-147">このユニットでは、Azure Cosmos DB データベース内のドキュメントを作成、置換、削除しました。</span><span class="sxs-lookup"><span data-stu-id="6d7fb-147">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>
