まず、Azure で Redis Cache インスタンスを作成してから、キャッシュに 2 つのデータ値を挿入する単純なトランザクションを作成しましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Azure Redis Cache の作成

Azure CLI を使用して Azure Redis Cache の作成から始めましょう。 Azure とやりとりするには、ブラウザー ウィンドウの右側にある Cloud Shell を使用します。

`azure rediscache create` コマンドを使用して、新しい Azure Redis Cache を作成します。 これは、いくつかのパラメーターを受け取ります。 最も一般的なものを次に示します (完全なリストを取得するには、マニュアルを参照してください)。

> [!div class="mx-tableFixed"]
> | パラメーター | 説明 |
> |-----------|-------------|
> | `--name`    | キャッシュの名前: グローバルに一意の名前にし、文字、数字、ハイフンで構成する必要があります。 |
> | `--resource-group` | Azure サンドボックスの一部である、事前に作成したリソース グループ <rgn>[サンドボックス リソース グループ名]</rgn> を使用します。 |
> | `--location` | キャッシュが配置される場所を指定します。 通常は、データ コンシューマーに近い場所を選択します。 この場合は、Azure サンドボックス内で使用できる場所に制限されています。 最も近いものを選択します。 |
> | `--size` | Redis Cache のサイズです。 有効な値: [C0、C1、C2、C3、C4、C5、C6、P1、P2、P3、P4] |
> | `--sku` | Redis SKU です。 有効な値: [Basic、Standard、Premium] |
> | `--enable-non-ssl-port` | キャッシュの非 SSL ポートを有効にする場合は、このフラグを追加します。 |

### <a name="selecting-a-location"></a>場所の選択
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. 次のオプションを使用してキャッシュを作成します。
    - サイズ: C0
    - SKU: Basic
    - EnableNonSslPort
    
1. コマンドラインの例を次に示します。**[name]** と **[location]** は、必ず有効な値に置き換えてください。

    ```azurecli
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

1. キャッシュの作成には数分かかることがあります。

## <a name="create-a-net-core-console-application"></a>.NET Core コンソール アプリケーションを作成する

次に、Azure Redis Cache にデータ値を挿入するために使用される .NET Core C# ベースのコンソール アプリケーションを作成します。

1. ウィンドウの右側に統合された Cloud Shell を使用して、新しい .NET Core アプリケーションを作成します。 これに "RedisData" という名前を付けます。

    ```bash
    dotnet new console --name RedisData
    ```
    
1. アプリ用に作成した新しいディレクトリに変更します。

    ```bash
    cd RedisData
    ```
    
1. プロジェクト ファイルと、**Program.cs** ソース ファイルが 1 つあるはずです。

1. アプリケーションをビルドして実行します。"Hello, World!" と出力されます。

    ```bash
    dotnet run
    ```
    
## <a name="add-the-servicestackredis-nuget-package"></a>ServiceStack.Redis NuGet パッケージを追加する

コンソール アプリケーションを作成したので、**ServiceStack.Redis** NuGet パッケージを追加する必要があります。 これにより、Redis Cache に接続して、C# でコマンドを発行できるようになります。

1. ターミナル シェルを使用して、NuGet パッケージ **ServiceStack.Redis** を追加します。

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. すべてコンパイルされていることを確認するため、もう一度アプリケーションをビルドして実行します。 ここでも "Hello, World!" が出力されるはずです。

## <a name="get-your-azure-redis-cache-connection-string"></a>Azure Redis Cache の接続文字列を取得する

Azure Redis Cache に接続するには、パスワードと URL を含む接続文字列が必要です。 この接続文字列は **ServiceStack.Redis** に固有のもので、`[password]@[host name]:[port]` の形式になります。

このキーは、Azure portal またはコマンドラインを使用して取得できます。 ポータルによるアプローチは「**Optimize your web applications by caching read-only data with Redis**」(Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する) モジュールで使用したため、ここでは後者を使用しましょう。

`azure rediscache list-keys` コマンドを使用して、アクセス キーを取得します。 次に示すように、名前とリソース グループを指定する必要があります。

```azurecli
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

次のような結果が返されます。

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. ご自分の**主**キーをクリップボードにコピーします。

1. ホスト名は、キャッシュを作成したときにサフィックス `.redis.cache.windows.net` を付けてそのキャッシュに指定した名前になります。

1. ポートは **6379** になります。

1. コンソール内のすべての情報を確認するには、アクティブなサブスクリプション (この場合は Azure サンドボックス) のすべての Redis Cache インスタンスを示す `azure rediscache list` コマンドを使用します。

## <a name="add-the-connection-string-to-your-app"></a>アプリに接続文字列を追加する

1. アプリ フォルダーにアクセスしていることを確認します。 `ls` または `dir` を入力した場合、**Program.cs** は現在のフォルダーにあるはずです。

1. アプリ フォルダーに `code .` を入力して、組み込みのエディターを開きます。

1. **Program.cs** ソース ファイルを選択します。

1. `Program` クラスで次のフィールドを作成します。

    ```csharp
    static string redisConnectionString = "";
    ```

1. 先ほど作成した **redisConnectionString** フィールドに接続文字列を貼り付け、ポート番号として **6379** を使用します。 接続文字列は `[password]@[host name]:[port]` の形式になっていることを思い出してください。

接続文字列の例は次のようになります。

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Azure Redis Cache に 2 つのデータ値を挿入する

最後に、Azure Redis Cache にデータを追加します。

1. **Program.cs** ファイルの先頭に次の using ステートメントを追加します。

    ```csharp
    using ServiceStack.Redis;
    ```

1. **Main** メソッド内に次のコードのスニペットを追加します。 これにより、2 つの値が過渡的に追加されます。

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
1. エディター ウィンドウの下部にあるコマンド プロンプトでアプリケーションを実行し、コンソールに「**トランザクションをコミットしました**」と表示されることを確認します。 

    ```bash
    dotnet run
    ```
    
## <a name="verify-your-data"></a>データの検証

完了するため、追加したデータが Azure Redis Cache にあることを確認しましょう。

1. [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。

1. 左側のサイドバーで **[すべてのリソース]** を選択して Redis Cache を見つけ、左のフィルター ボックスを使用して Redis Cache インスタンスを選択します。 または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。

1. Redis Cache インスタンスを選択します。

1. ご自分の Redis Cache の **[概要]** ブレードで、**[コンソール]** を選択します。 これにより、詳細な Redis コマンドを入力できる Redis コンソールが開きます。

1. 「**get MyKey1**」を入力します。 返される値が **MyValue1** であることを確認します。

1. 「**get MyKey2**」を入力します。 返される値が **MyValue2** であることを確認します。

    ![MyKey1 と MyKey2 の値を示す Azure Redis コンソールのスクリーンショット。](../media/4-redis-console.png)