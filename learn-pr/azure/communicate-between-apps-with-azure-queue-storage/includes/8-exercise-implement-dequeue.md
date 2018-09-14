キューの次のメッセージを読み取り、処理、およびキューから削除するためのコードを記述することで、アプリケーションを完了するようになりました。 

パラメーターが渡されなかった場合は、このコードを前と同じアプリケーションに配置して実行しますが、ニュース サービス シナリオでは、実際にこのコードを中間層サーバーに配置してストーリーを処理します。

## <a name="dequeue-a-message"></a>メッセージをデキューする

次のメッセージをキューから取得する新しいメソッドを追加してみましょう。

1. Cloud Shell で `QueueApp` フォルダーに切り替えて、「`code .`」と入力してオンライン エディターを開きます。
 
1. `Program.cs` ソース ファイルを開きます。

1. パラメーターを取らず、`Task<string>` を返す `ReceiveArticleAsync` という名前の `Program` クラス内で静的メソッドを作成します。 このメソッドを使用して、キューからニュース記事をプルし、それを返します。
    - さらに、メソッドに `async` キーワードを追加します。これは非同期の `Task` ベースのメソッドを使用するためです。

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
1. ご自分の `ReceiveArticleAsync` メソッド内で新しい`GetQueue` メソッドを呼び出して、キュー参照を取得し、それを変数に割り当てます。

1. 次に、`CloudQueue` オブジェクトに対して `ExistsAsync` メソッドを呼び出します。これにより、キューが作成されているかどうかが返されます。 存在しないキューからメッセージを取得しようとすると、API から例外がスローされます。
    - このメソッドは非同期であるため、`await` を使用して戻り値を取得します。
    - `ReceiveArticleAsync` メソッドには `async` キーワードが既に含まれているはずです。そうでない場合はここで追加します。


1. `ExistsAsync` からの戻り値を使用する `if` ブロックを追加します。 キューから値を読み取ってブロックに入れるためのコードを追加します。 値が読み取られなかったことを示すメソッドに最終的な戻り値の文字列を追加します。 メソッドは次のようになっているはずです。

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

1. `CloudQueue` オブジェクトに対して `GetMessageAsync` を呼び出して、キューから最初の `CloudQueueMessage` を取得します。 キューが空の場合、戻り値は `null` になります。

1. 値が null 以外の場合は、`CloudQueueMessage` オブジェクト上で `AsString` プロパティを使用して、メッセージの内容を取得します。

1. `CloudQueue` オブジェクトに対して `DeleteMessageAsync` を呼び出して、キューからメッセージを削除します。

最終的なメソッドの実装は次のようになります。

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```

## <a name="call-the-receivearticleasync-method"></a>ReceiveArticleAsync メソッドを呼び出す

最後に、この新しいメソッドを呼び出すためのサポートを追加してみましょう。 プログラムにパラメーターを渡さない場合に、これを行います。

1. `Main` メソッドを検索します。具体的には、前にパラメーターを検索するために追加した `if` ブロックを検索します。

1. `else` 条件を追加して、`ReceiveArticleAsync` メソッドを呼び出します。 

1. これは非同期であるため、通常は `await` を使用しますが、前述したように C# のすべてのバージョンでそれが動作するわけではありません。そのため、`Result` プロパティのみを使用して戻り値を取得し、それをコンソール ウィンドウに出力します。

コードは次のような内容になります。

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = ReceiveArticleAsync().Result;
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a>アプリケーションを実行する

これでコードは完成しました。 このコードでは、メッセージの送信および取得を行えるようになりました。 これをテストするには、`dotnet run` を使用します。メッセージを送信するにはパラメーターを渡し、1 個のメッセージを読み取るにはパラメーターをオフのままとします。

キューが存在しないときのテストを行う場合は、Azure CLI を使用してキュー (およびすべてのデータ) を削除することができます。 必ず、`<connection-string>` パラメーターを置換 (または環境変数を設定) します。

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

次にメッセージを追加すると、キューが再度作成されます。

> [!NOTE]
> 削除操作は実際に非同期に行われます。 完了していない場合、キューを再作成しようとしたときに、例外を発生可能性があります。