<!--TODO: explain Etag in knowledge needed-->

Azure Cosmos DB への接続が完了した後、次の手順としてデータベースに保存された文書の作成、読み込み、置換、削除を行います。 このユニットでは、WebCustomer コレクション内にユーザー ドキュメントを作成します。その後、ID を使った読み込み、置換、および削除を行います。

## <a name="working-with-documents-programmatically"></a>プログラムによるドキュメントの操作

データは、Azure Cosmos DB 内の JSON ドキュメントに格納されます。 前のモジュールで示したようにポータルで、またはこのモジュールで説明するようにプログラムを使用して、[ドキュメント](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents)を作成、取得、置換、または削除できます。 Azure Cosmos DB では .NET、.NET Core、Java、Node.js、Python 用のクライアント側 SDK が提供されており、いずれもこれらの操作をサポートします。 このモジュールで使用する .NET Core SDK CRUD を実行する (作成、取得、更新、および削除) Azure Cosmos DB に格納されている NoSQL データの操作。 

Azure Cosmos DB ドキュメントに対する主要な操作は、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) クラスの一部です。
* [CreateDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [ReadDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [ReplaceDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* [UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet). アップサートでは、ドキュメントが既に存在するかどうかに応じて、作成または置換操作が実行されます。
* [DeleteDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

これらの操作を実行するには、データベースに格納されているオブジェクトを表すクラスを作成する必要があります。 ユーザーのデータベースを使って作業を行っているため、ユーザーの姓名ならびにユーザー ID（水平スケーリングを可能とするためのパーティション キーとなるため、必須です）などのプライマリ データならびに発送用情報や注文履歴のためのサブクラスを格納するための **User** クラスの作成を行う必要があります。

ユーザーを表すこれらのクラスを作成した後、インスタンスごとに新しいユーザー ドキュメントを作成し、ドキュメントに対していくつかのシンプルな CRUD 操作を実行します。

## <a name="create-documents"></a>ドキュメントの作成

1. 最初に、Azure Cosmos DB に格納するオブジェクトを表す **User** クラスを作成します。 また、**User** の内部で使用される **OrderHistory** および **ShippingPreference** サブクラスも作成します。 ドキュメントには、JSON で **id** としてシリアル化される **Id** プロパティが必要であることに注意してください。

    これらのクラスを作成するためには、**BasicOperations** メソッド下にある **User**、**OrderHistory**、および **ShippingPreference** の各クラスをコピーしてペーストします。

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

1. 統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが実行されることを確認します。

    ```csharp
    dotnet run
    ```

1. 次に、**CreateUserDocumentIfNotExists** タスクをコピーして、**ShippingPreference** クラスの下に貼り付けます。

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

1. 末尾に、次を追加、 **BasicOperations**メソッド。

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

1. 統合ターミナルで、もう一度プログラムを実行する次のコマンドを入力します。

    ```csharp
    dotnet run
    ```

    アプリケーションには、各新規ユーザー ドキュメントが作成されると、ターミナル出力が表示されます。 プログラムを完了する任意のキーを押します。

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a>ドキュメントを読み取る

1. データベースからドキュメントを参照する次のコードを Program.cs ファイルで WriteToConsoleAndPromptToContinue メソッドの後の場所にコピーします。
    
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

1.  次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. 統合ターミナルで、プログラムを実行するには、次のコマンドを入力します。

    ```
    dotnet run
    ```
    ターミナルに次の出力が表示されます。"Read user 1" という出力は、ドキュメントが取得されたことを示します。

    ```
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a>ドキュメントを置換する

Azure Cosmos DB は、JSON ドキュメントの置換をサポートします。 ここでは、姓の変更を反映するようにユーザー レコードを更新します。

1. コピーして貼り付け、 **ReplaceFamilyDocument** Program.cs ファイルで ReadUserDocument メソッドの後のメソッド。

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

1. 次のコードをコピーし、**BasicOperations** メソッドの最後の `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` 行の後に貼り付けます。

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. 統合ターミナルで、次のコマンドを実行します。

    ```
    dotnet run
    ```
    ターミナルに次の出力が表示されます。"Replaced last name for Suh" という出力は、ドキュメントが置き換えられたことを示します。

    ```
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

## <a name="delete-documents"></a>ドキュメントの削除

1. **DeleteUserDocument** メソッドをコピーし、**ReplaceUserDocument** メソッドの下に貼り付けます。
    
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

1. コピーし、の最後で次のコードを貼り付け、 **BasicOperations**メソッド。

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. 統合ターミナルで、次のコマンドを実行します。

    ```
    dotnet run
    ```

    ターミナルに次の出力が表示されます。"Deleted user 1" という出力は、ドキュメントが削除されたことを示します。

    ```
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

## <a name="summary"></a>まとめ

このユニットでは、Azure Cosmos DB データベース内のドキュメントを作成、置換、削除しました。