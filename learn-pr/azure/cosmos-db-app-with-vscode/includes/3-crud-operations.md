<!--TODO: explain Etag in knowledge needed-->

Azure Cosmos DB への接続が完了したら、次の手順はデータベースに格納されるドキュメントの作成、読み込み、置き換え、削除です。 このユニットでは、WebCustomer コレクションにユーザー ドキュメントを作成し、ID を指定して取得し、置き換え、削除します。

## <a name="working-with-documents-programmatically"></a>プログラムによるドキュメントの操作

データは、Azure Cosmos DB 内の JSON ドキュメントに格納されます。 前のモジュールで示したようにポータルで、またはこのモジュールで説明するようにプログラムを使用して、[ドキュメント](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents)を作成、取得、置換、または削除できます。 Azure Cosmos DB では .NET、.NET Core、Java、Node.js、Python 用のクライアント側 SDK が提供されており、いずれもこれらの操作をサポートします。 このモジュールでは、.NET Core SDK を使用して CRUD (作成、取得、更新、削除) 操作を実行します。 

Azure Cosmos DB ドキュメントに対する主要な操作は、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) クラスの一部です。
* [CreateDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [ReadDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [ReplaceDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* [UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet). アップサートでは、ドキュメントが既に存在するかどうかに応じて、作成または置換操作が実行されます。
* [DeleteDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

これらの操作を実行するには、データベースに格納されているオブジェクトを表すクラスを作成する必要があります。 ここではユーザーのデータベースを操作しているので、ユーザーの姓名やユーザー ID (水平スケーリングを有効にするためのパーティション キーであるため必須です) などの主要なデータを格納するための **User** クラスと、出荷方法や注文履歴のためのサブクラスを作成します。

ユーザーを表すこれらのクラスを作成した後、インスタンスごとに新しいユーザー ドキュメントを作成し、ドキュメントに対していくつかのシンプルな CRUD 操作を実行します。

## <a name="create-documents"></a>ドキュメントの作成

1. 最初に、Azure Cosmos DB に格納するオブジェクトを表す **User** クラスを作成します。 また、**User** の内部で使用される **OrderHistory** および **ShippingPreference** サブクラスも作成します。 ドキュメントには、JSON で **id** としてシリアル化される **Id** プロパティが必要であることに注意してください。

    これらのクラスを作成するには、以下の **User**、**OrderHistory**、**ShippingPreference** クラスをコピーして、**BasicOperations** メソッドの下に貼り付けます。

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

1. そして、次のコードを **BasicOperations** メソッドに追加します。

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

1. 統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。

    ```csharp
    dotnet run
    ```

    両方のユーザー レコードが正常に作成されたことを示す次の出力が、ターミナルに表示されます。

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a>ドキュメントを読み取る

1. データベースからドキュメントを読み取るには、次のコードをコピーし、Program.cs ファイルの末尾に配置します。
    
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

1. Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。

    ```
    dotnet run
    ```
    ターミナルに次の出力が表示されます。"Read user 1" という出力は、ドキュメントが取得されたことを示します。

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

## <a name="replace-documents"></a>ドキュメントを置換する

Azure Cosmos DB は、JSON ドキュメントの置換をサポートします。 ここでは、姓の変更を反映するようにユーザー レコードを更新します。

1. **ReplaceFamilyDocument** メソッドをコピーし、Program.cs ファイルの末尾に貼り付けます。

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

1. Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。

    ```
    dotnet run
    ```
    ターミナルに次の出力が表示されます。"Replaced last name for Suh" という出力は、ドキュメントが置き換えられたことを示します。

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

1. 次のコードをコピーし、**BasicOperations** メソッドの 2 回目のクエリ実行のすぐ下に貼り付けます。

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

## <a name="summary"></a>まとめ

このユニットでは、Azure Cosmos DB データベース内のドキュメントを作成、置換、削除しました。