複数の操作を一緒に実行を保証する必要があります。 たとえば、インスタント メッセージング アプリケーションで送信できます個々 の画像や、個々 のテキスト メッセージ、画像およびテキスト メッセージをまとめて。 一緒に画像とテキスト メッセージを送信するユーザーを選択すると、グループの他のメンバーの受信を同時にことを確認する必要があります。 これは、機能は、画像とテキスト メッセージが同時に受信されていない場合、画像とテキスト メッセージの間に個別のメッセージを送信するでした可能性があるため重要です。 全体的なメッセージ交換使用混乱することができます。

ここでは、複数の操作が同時に実行されることを保証するために Azure Redis Cache でトランザクションを作成する方法を紹介します。

## <a name="how-to-create-a-transaction"></a>トランザクションを作成する方法

トランザクションを作成するには、トランザクション ブロックを用意する必要があります。 これは、一緒に実行されるコマンドを含むキューです。 `MULTI`コマンドを使用して、トランザクション ブロックを作成し、すべての後続のコマンドはアトミック実行のキューに登録します。

## <a name="how-to-execute-a-transaction"></a>トランザクションを実行する方法

使用するトランザクション ブロックで、コマンドを実行、`EXEC`コマンド。 すべてのキューに登録されたコマンドの実行を正常に接続状態を復元します。 トランザクションを実行する場合は、使用できます、`DISCARD`コマンドはトランザクション ブロックをオフにし、通常への接続状態を設定します。

Azure Redis Cache のトランザクションを理解するために 1 つの重要な点は、ロールバックをサポートしていないことです。 これは、トランザクション内でのコマンドが失敗した場合、その他のコマンドが実行されることを意味します。

## <a name="what-is-servicestackredis"></a>ServiceStack.Redis とは何ですか。

**ServiceStack.Redis**は Azure Redis Cache を操作する (C#) クライアント ライブラリです。 これは低レベルの Azure Redis Cache コマンドを使用する代わりに使用できる c# クラスとメソッドを意味します。 使用する代わりに、たとえば、`MULTI`と`EXEC`でトランザクションを実行するコマンド**ServiceStack.Redis**を作成、`IRedisTransaction`オブジェクトを使用して、`CreateTransaction()`メソッド。

```csharp
var transaction = redisClient.CreateTransaction();
```

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a>C# と ServiceStack.Redis クライアントを使用してトランザクションを作成します。

使用する例を次に示します**ServiceStack.Redis**画像とテキストを含むメッセージを送信できるトランザクションを作成します。

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
トランザクションは、複数の操作が同時に実行されることを保証します。 使用してトランザクションを作成する、`MULTI`コマンド。 などのクライアント ライブラリを使用している場合**ServiceStack.Redis**、使用することができます、`CreateTransaction()`メソッド。