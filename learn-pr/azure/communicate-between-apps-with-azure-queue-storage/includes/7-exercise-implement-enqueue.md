これですべての要件が満たされましたので、新しいストレージ キューの作成およびメッセージの追加を行うコードを記述することができます。 通常、このコードは、データを生成するフロント エンド アプリ内に置きます。

## <a name="add-the-client-library-for-azure-storage"></a>Azure Storage 用クライアント ライブラリを追加する

**.NET 用 Azure Storage クライアント ライブラリ**をアプリに追加することから始めてみましょう。 これは、NuGet (.NET パッケージ マネージャー) を使用してインストールできます。 

1. `dotnet add package` コマンドを使用してプロジェクトに `WindowsAzure.Storage` パッケージをインストールします。 これは、Cloud Shell の_下_のターミナルウィンドウで実行するか、ローカル コンピューターで作業している場合は、プロジェクトと同じフォルダーのターミナル/コンソール ウィンドウで実行します。

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a>ニュース速報を送信するメソッドを追加する

次に、ニュース記事をキューに送信する新しいメソッドを作成しましょう。

1. コード エディターで `Program.cs` ファイルを開きます。

1. ファイルの先頭に次の名前空間を追加します。 次の両方からの型を使用して Azure Storage に接続して、キューを操作します。

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. `string` を取り、`Task` を返す `SendArticleAsync` という名前の `Program` クラス内に静的メソッドを作成します。 このメソッドを使用して、利用しているサービスにニュース記事を送信します。 次に示すように、入力パラメーターの名前を `newsMessage` とします。

    ```csharp
    ...
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. ご自分の新しいメソッド内で、静的メソッド `CloudStorageAccount.Parse` を使用してご利用の接続文字列を解析します (それを定数文字列内に配置したことを思い出してください)。 このメソッドからは、変数内に格納する必要がある `CloudStorageAccount` オブジェクトが返されます。

1. ストレージ アカウント オブジェクトに対して `CreateCloudQueueClient()` メソッドを呼び出すことで、クライアント オブジェクトを取得し、それを変数内に格納します。

1. 次に、そのクライアント オブジェクトに対して `GetQueueReference` メソッドを呼び出し、キュー名として文字列 "newsqueue" を渡します。 これにより、キューを操作するのに使用できる `CloudQueue` オブジェクトが返されます。 キューがまだ存在していなくても問題ありません。

1. `CloudQueue` オブジェクトに対して `CreateIfNotExistsAsync()` を呼び出して、確実にキューが使用できる状態になるようにします。 この場合は、必要に応じて、キューが作成されます。
    - これは非同期メソッドであるため、C# の `await` キーワードを使用して、確実にその適切な操作が行えるようにします。 そのことはまた、_メソッド_を `async` キーワードで修飾する必要があることを意味します。 
    - `CreateIfNotExistsAsync` によって `bool` 値が返されます。その値はキューが作成された場合は `true` となり、キューが既に存在する場合は `false` となります。 キューを作成した場合は、コンソールにメッセージが出力されます。
    - ヘルプが必要な場合は、次の例を参照してください。

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. `CloudQueueMessage` のインスタンスを作成します。 
    - これは `string` パラメーターを取り、メソッド パラメーターを渡します (`newsMessage`)。 これは、メッセージの_本文_となります。 バイナリ メッセージ ペイロードを作成できる  という名前の静的メソッドもあります。

1. `CloudQueue` オブジェクトに対して `AddMessageAsync` を呼び出して、キューにメッセージを追加します。 これも非同期メソッドであるので、確実にその適切な操作が行えるようにするために `await` キーワードを使用する必要があります。

1. ファイルを保存し、コマンド ウィンドウに「`dotnet build`」と入力してそれをビルドします。 エラーがある場合は修正します。次のコードを使用して、ご自分の作業内容をチェックすることができます。

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a>メッセージを送信するためのコードを追加する

この新しい送信メソッドをテストするために、受信したパラメーターが新しいメソッドに渡されるように `Main` メソッドを修正してみましょう。

1. `Main` メソッドを検索します。

1. 渡された `args` パラメーターを調べて、任意のデータがコマンドラインに渡されたかどうかを確認します。 そうである場合は、`String.Join` を使用して、区切り記号としてスペースを使用しているすべての単語から、単一の文字列を作成します。

1. それを新しい `SendArticleAsync` メソッドに渡します。 

1. そのメソッドから制御が返されたら、`Console.WriteLine` を使用して、送信してあるメッセージを出力します。

1. ファイルを保存し、プログラムをビルドします (`dotnet build`)。次のコードを使用することで、作業内容を確認することができます。

    > [!NOTE]
    > このメソッドは技術的には非同期であるため、`await` キーワードを使用したいです。ただし、この機能は C# 7.1 以降を使用していない限り、ご自分の `Main` メソッドでは利用できません。 メソッドで単に `Wait()` を呼び出し、メソッドの応答の待機を実際にブロックします。これは 1 分以内に修正します。

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a>アプリケーションを実行する

アプリケーションでは、メッセージを送信できるようになりました。 それをテストするには、`dotnet run` コマンドを使用してコマンド ラインから実行します。 任意の追加の文字列がパラメーターとしてアプリケーションに渡されます。 

> [!WARNING]
> 必ず、オンライン エディターですべてのファイルが保存されていることを確認してから、プログラムをビルドして実行します。

たとえば、次のように入力することができます。

```azurecli
dotnet run Send this message
```

これにより、文字列 `"Send this message"` がキューに追加されます。

## <a name="check-your-results"></a>結果を確認する

Azure portal 内で **ストレージ エクスプローラー** を使用してキューを確認することができます。その製品を開くと、自分が所有している各ストレージ アカウント内のデータを探索できるようになります。

別の方法として、Azure CLI または PowerShell を使用することもできます。 このコマンドをシェル内で試してみます。それには、`<connection-string>` 値をご自分の特定の接続文字列に置換します。

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

これによりご自分のメッセージに関する情報がダンプされます。それは次のようになります。

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

上記のツールを使用して試すことができるコマンドが他にもいくつか用意されています。それらを調べるには、`az storage queue --help` と `az storage message --help` の両方をチェックアウトします。

> [!TIP]
> `AZURE_STORAGE_CONNECTION_STRING` という名前の環境変数にご自分の接続文字列値を格納し、`--connection-string` を毎回入力しなくても済むようにします。

## <a name="optional---use-the-async-versions-of-the-methods"></a>省略可能 - メソッドの非同期バージョンを使用する

上述の送信メソッドでは、`Wait()` を使用しましたが、これはコンピューター リソースを効率的に使用するものではありません。 代わりに C# の `async` と `await` のメソッドを使用します。 ただし、これらのキーワードを **Main** メソッドに適用するには、C# 7.1 _以上_を使用する必要があります。

### <a name="switch-to-c-71"></a>C# 7.1 に切り替える

C# 7.1 より前では、C# の `async` および `await` キーワードは **Main** メソッドで有効なキーワードではありませんでした。 **.csproj** ファイルでフラグを使用することで、そのコンパイラに簡単に切り替えることができます。

1. エディターで **QueueApp.csproj** ファイルを開きます。

1. ビルド ファイル内の最初の `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。 完了すると、次のようになります。
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>async キーワードを適用する

次に、`async` キーワードを **Main** メソッドに適用します。 次の 3 つのことを行う必要があります。

1. `async` キーワードを **Main** メソッド シグネチャに追加します。
1. 戻り値の型を `void` から `Task` に変更します。
1. `SendArticleAsync` への呼び出しから `Wait()` への呼び出しを削除し、`await` に変更します。

アプリを再実行します。アプリは以前と同様に動作するはずですが、メッセージがキューに入るのを待機している間、コードは .NET スレッドプールにスレッドをリリースして戻せるようになっているはずです。

これでメッセージの送信は完了しました。最後の手順では、メッセージを_受信_するためのサポートを追加します。