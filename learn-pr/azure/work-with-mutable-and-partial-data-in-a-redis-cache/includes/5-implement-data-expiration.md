ここでは、Azure Redis Cache でデータに有効期限を追加します。

## <a name="add-an-expiration-time"></a>有効期限を追加する

前回の演習では、**Program.cs** に次のコードを追加したところで終わりました。

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

**MyKey1** と **MyKey2** の両方に 15 秒の有効期限を追加しましょう。

1. トランザクションをコミットする前に次のコードを追加します。

    ```csharp
    //Add an expiration time
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
    ```

    このコードでは、**Expire** メソッドは **RedisNativeClient** の一部です。 このメソッドにアクセスするには、まずオブジェクトをキャストする必要があります。

## <a name="verify-the-expiration"></a>有効期限を確認する

データを期限切れにするコードを追加したので、プログラムを実行し、Redis からデータが 削除されていることを確認しましょう。

1. プログラムを実行します。

    ```bash
    dotnet run
    ```
    
1. Azure portal の Azure Redis コンソールに戻ります。

1. データがまだ存在することを確認するために、次のコマンドを発行します。

    ```
    get MyKey1
    ```

1. 15 秒後に、コマンドをもう一度発行します。 データがなくなっていることがわかります。

    ![MyKey1 の値が nil であることを示す Azure Redis コンソールのスクリーンショット。](../media/6-redis-console-data-expiration.png)