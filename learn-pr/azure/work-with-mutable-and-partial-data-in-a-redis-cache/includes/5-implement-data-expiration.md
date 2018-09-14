ここでは、Azure Redis Cache でデータを有効期限を追加します。

## <a name="add-an-expiration-time"></a>有効期限を追加します。

前回の演習では、話では、次のコードで**Program.cs**します。

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

両方を追加して、15 秒の有効期限が**MyKey1**と**MyKey2**します。

トランザクションをコミットする前に、次のコードを追加します。

    ```csharp
    //Add an expiration time
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
    ```

このコードで、**有効期限は切れます**メソッドの一部である、 **RedisNativeClient**します。 メソッドへのアクセス、私たちには、まず、オブジェクトをキャストする必要があります。

## <a name="verify-the-expiration"></a>有効期限を確認します。

データを期限切れにコードを追加したのでみましょうプログラムを実行し、Azure Redis Cache からのデータが削除されるかを確認します。

1. プログラムを実行します。

    ```bash
    dotnet run
    ```
    
1. Azure portal で Azure Redis Cache のコンソールに切り替えます。

1. データがまだあることを確認するには、次のコマンドを発行します。

    ```
    get MyKey1
    ```

1. 15 秒後、もう一度コマンドを発行します。 データがなくなったことがありますが表示されます。

    ![Nil される MyKey1 の値を表示する Azure Redis Cache のコンソールのスクリーン ショット](../media/6-redis-console-data-expiration.png)