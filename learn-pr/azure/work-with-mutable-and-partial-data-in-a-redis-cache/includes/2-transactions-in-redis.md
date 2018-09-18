場合によっては、複数の操作が同時に実行されることを保証する必要があります。 たとえば、インスタント メッセージング アプリケーションで、ユーザーは個々の画像を送信したり、個々のテキスト メッセージを送信したり、イメージとテキスト メッセージを同時に送信したりできます。 ユーザーが画像とテキスト メッセージを同時に送信する場合、開発者はグループの他のメンバーがそれらを同時に受信するようにする必要があります。 画像とテキスト メッセージが同時に受信されない場合、画像送信とテキスト メッセージ送信の間に別のメッセージが送信される可能性があり、メッセージ交換全体で混乱が生じることが考えられるため、このことは重要です。

ここでは、複数の操作が同時に実行されることを保証するために Redis でトランザクションを作成する方法について説明します。

## <a name="how-to-create-a-transaction"></a>トランザクションを作成する方法

トランザクションを作成するには、トランザクション ブロックが必要です。 これは、同時に実行されるコマンドを含むキューです。 `MULTI` コマンドを使用してトランザクション ブロックを作成すると、後続のすべてのコマンドがアトミック実行のキューに登録されます。

## <a name="how-to-execute-a-transaction"></a>トランザクションを実行する方法

トランザクション ブロックでコマンドを実行するには、`EXEC` コマンドを使用します。 これにより、キューに登録されたすべてのコマンドが実行され、接続状態が正常に回復します。 トランザクションを実行しない場合は、`DISCARD` コマンドを使用して、トランザクション ブロックをクリアし、接続状態を正常に設定することができます。

Redis トランザクションを理解するうえで重要な点は、ロールバックがサポートされないことです。 これは、トランザクション内の 1 つのコマンドが失敗した場合でも、それ以外のコマンドが実行されることを意味します。

## <a name="what-is-servicestackredis"></a>ServiceStack.Redis とは

前のモジュール「**Optimize your web application by caching read-only data with Redis**」(Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する) では、**StackExchange.Redis** クライアントを使用しました。 ここでは、一般的に使用されるもう 1 つのクライアントである **ServiceStack.Redis** を試してみましょう。

**ServiceStack.Redis** は、Redis Cache とやりとりするための C# クライアント ライブラリです。 つまり、低レベルの Redis コマンドを使用する代わりに、C# クラスおよびメソッドを使用できます。

たとえば、トランザクションを作成するには、通常は Redis `MULTI` コマンドを使用する必要があります。 しかし、**ServiceStack.Redis** を使用すれば、`CreateTransaction()` メソッドを使用してトランザクションを作成できます。

```csharp
var transaction = redisClient.CreateTransaction();
```

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>C# と ServiceStack.Redis クライアントを使用してトランザクションを作成する

次に、**ServiceStack.Redis** を使用して、画像とテキストを含むメッセージを送信するためのトランザクションを作成する例を示します。

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        return transactionResult;
    }
}
```
トランザクションにより、複数の操作が同時に実行されることを保証できます。 トランザクションの作成は、`MULTI` コマンドを使用できますが、**ServiceStack.Redis** などのクライアント ライブラリを使用することで `CreateTransaction()` メソッドを使用できます。