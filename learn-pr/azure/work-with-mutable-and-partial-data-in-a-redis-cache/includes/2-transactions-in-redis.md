複数の操作を確実にまとめて実行しなければならない場合があります。 たとえば、インスタント メッセージング アプリケーションでは、ユーザーは個々の画像を送信したり、個々のテキスト メッセージを送信したり、イメージとテキスト メッセージを同時に送信したりできます。 ユーザーが画像とテキスト メッセージを同時に送信することを選択したとき、グループの他のメンバーがそれらを確実に同時に受信するようにしなければなりません。 この同時受信が重要であるのは、画像とテキスト メッセージが同時に受信されない場合、画像とテキスト メッセージの間で別個のメッセージが送信される可能性があるためです。 会話全体が混乱することになりかねません。

ここでは、複数の操作を確実に同時実行するために Azure Redis Cache でトランザクションを作成する方法について説明します。

## <a name="creating-and-running-transactions"></a>トランザクションの作成と実行

Redis のトランザクションは、グループとして実行される複数のコマンドをキューに入れることで動作します。 トランザクションが実行されると、その中でキューに入れられたコマンドは、他のクライアントからのその他のコマンドがインターリーブされずに実行されます。

トランザクション ブロックを開始するには、`MULTI` コマンドを入力します。 追加のコマンドはキューに入れられ、すぐには実行されません。 `EXEC` コマンドを実行すると、トランザクション単位としてキューに入れられているすべてのコマンドが実行されます。 コマンドをキューに入れている間にトランザクションを中止する場合、`DISCARD` コマンドを実行すると、キューに入れられた_どの_コマンドも実行せずにトランザクション ブロックが閉じられます。

Redis トランザクションは、ロールバックの概念をサポートしていません。 誤った構文を含むコマンドをトランザクション ブロックのキューに入れると、そのブロックは開いたままになりますが、`EXEC` で実行しようとすると自動的に破棄されます。 実行_中_ (`EXEC`が呼び出された後) にトランザクションのコマンドが失敗しても、トランザクションは中止または &mdash; にロールバックされません。Redis は引き続きすべてのコマンドを実行し、 トランザクションが正常に完了したと見なします。

## <a name="redis-transactions-with-servicestackredis"></a>ServiceStack.Redis を使用した Redis トランザクション

**ServiceStack.Redis** は、Azure Redis Cache とやりとりするための C# クライアント ライブラリです。

ServiceStack.Redis でのトランザクションは、`IRedisClient.CreateTransaction()` を呼び出すことで作成されます。 返される `IRedisTransaction` オブジェクトでは、`QueueCommand()` を使用して複数のコマンドをキューに入れることができます。 トランザクションで `Commit()` を呼び出すと、オブジェクトがそれを実行します。

`IRedisTransaction` オブジェクトは破棄可能であり、`Commit()` を呼び出す前に破棄されている場合、自動的に `DISCARD` コマンドを実行します。 この機能は、C# の `using` ブロックで正常に作動します。何らかの理由でトランザクションをコミットしなかった場合、Redis 接続を引き続き使用できるように、トランザクションは自動的に破棄されます。

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>C# と ServiceStack.Redis クライアントを使用してトランザクションを作成する

次に、ServiceStack.Redis を使用して、画像 URL 含むメッセージとテキスト メッセージのコンテンツを送信するためのトランザクションを作成する例を示します。

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    return transactionResult;
}
```

トランザクションにより、複数の操作が、トランザクション間で他のクライアントからの操作なしで一緒に実行されることが保証されます。 `MULTI` コマンドを使用してトランザクションを作成します。 ServiceStack.Redis を使用して、`CreateTransaction()` メソッドを使用します。