まず、Azure Redis Cache のインスタンスを作成してから、キャッシュに 2 つのデータ値を挿入する単純なトランザクションを作成しましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Azure Redis Cache の作成

Azure CLI を使用して Azure Redis Cache の作成から始めましょう。 Azure とやりとりするには、ブラウザー ウィンドウの右側にある Cloud Shell を使用します。

`az redis create` コマンドを使用して、新しい Azure Redis Cache を作成します。 これは、いくつかのパラメーターを受け取ります。 最も一般的なものを次に示します (完全なリストを取得するには、マニュアルを参照してください)。

> [!div class="mx-tableFixed"]
> | パラメーター | 説明 |
> |-----------|-------------|
> | `--name`    | キャッシュの名前: グローバルに一意の名前にし、文字、数字、ハイフンで構成する必要があります。 |
> | `--resource-group` | Azure サンドボックスの一部である、事前に作成したリソース グループ **<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。 |
> | `--location` | キャッシュが配置される場所を指定します。 通常は、データ コンシューマーに近い場所を選択します。 この場合は、Azure サンドボックス内で使用できる場所に制限されています。 最も近いものを選択します。 |
> | `--size` | Azure Redis Cache のサイズ。 有効な値: [C0、C1、C2、C3、C4、C5、C6、P1、P2、P3、P4]。 |
> | `--sku` | Azure Redis Cache SKU。 有効な値: [Basic、Standard、Premium]。 |

### <a name="select-a-location"></a>場所を選択します。
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. 次のオプションを使用してキャッシュを作成します。
    - サイズ: C0
    - SKU: Basic

1. コマンド ラインの例を次に示します。 `[name]` パラメーターを一意の名前に置き換えてください。 米国東部以外のリージョンを使用する場合は場所を置き換えることができます。

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a>.NET Core コンソール アプリケーションを作成する

次に、Azure Redis Cache にデータ値を挿入するために使用される .NET Core コンソール アプリケーションを作成します。

1. ウィンドウの右側に統合された Cloud Shell を使用して、新しい .NET Core アプリケーションを作成します。 これに "RedisData" という名前を付けます。

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. アプリ用に作成した新しいディレクトリに変更します。

    ```bash
    cd RedisData
    ```

1. アプリケーションをビルドして実行します。"Hello World!" と出力されます。

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a>ServiceStack.Redis NuGet パッケージを追加する

コンソール アプリケーションを作成したので、**ServiceStack.Redis** NuGet パッケージを追加する必要があります。 これにより、Azure Redis Cache に接続して、C# でコマンドを発行できるようになります。

1. ターミナル シェルを使用して、NuGet パッケージ **ServiceStack.Redis** を追加します。

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. すべてコンパイルされていることを確認するため、もう一度アプリケーションをビルドして実行します。 ここでも "Hello World!" が出力されるはずです。

## <a name="get-your-azure-redis-cache-connection-string"></a>Azure Redis Cache の接続文字列を取得する

Azure Redis Cache に接続するには、パスワードと URL を含む接続文字列が必要です。 **ServiceStack.Redis** には独自の接続文字列の形式 `[password]@[hostname]:[sslport]?ssl=true` があります。

このパスワードは、Azure portal またはコマンドラインを使用して取得できます。 ポータルによるアプローチは「Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する」モジュールで使用したため、ここでは後者を使用しましょう。

`az redis list-keys` コマンドを使用して、アクセス キーを取得します。 これらのコマンドを実行してプライマリ キーを取得し、それを `REDIS_KEY` という変数に保管して表示します。

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

次に、このコマンドを実行して、接続文字列をまとめて、コマンド ライン上に表示します。 ホスト名は `.redis.cache.windows.net` の前に来るキャッシュの名前で、ポートはデフォルトの Redis SSL ポートである **6380** です。

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

次のステップで使用するクリップボード &mdash; に接続文字列をコピーします。

## <a name="add-the-connection-string-to-your-app"></a>アプリに接続文字列を追加する

1. アプリケーション フォルダーから Cloud Shell エディターを開きます。

    ```bash
    cd ~/RedisData
    code .
    ```

1. **Program.cs** ソース ファイルを選択します。

1. `Program` クラスに次のフィールドを作成し、接続文字列を値として貼り付けます。

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    次に例を示します。

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Azure Redis Cache に 2 つのデータ値を挿入する

最後に、Azure Redis Cache にデータを追加します。

1. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

    ```csharp
    using ServiceStack.Redis;
    ```

1. `Main` メソッドの内容を次のコードに置き換えます。 これにより、トランザクションを使用して 2 つの値が追加されます。

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. アプリケーションをビルドして実行する前に、Redis キャッシュが完全にプロビジョニングされていることを確認してください。 `az redis create` が完了するまでに数分かかる場合があります。 次のコマンドを実行して、状態を確認します。

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    状態が `Creating` の場合は、数分後にもう一度確認してください。 完了すると、`Succeeded` が表示されます。

1. アプリケーションを実行し、**[トランザクションがコミットされました]** とコンソールに表示されることを確認します。

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a>データの検証

完了するため、追加したデータが Azure Redis Cache にあることを確認しましょう。

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。

1. 左側のサイドバーで **[すべてのリソース]** を選択して Azure Redis Cache を見つけ、左のフィルター ボックスを使用して Azure Redis Cache インスタンスを選択します。 または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。

1. Azure Redis Cache インスタンスを選択します。

1. Azure Redis Cache の **[概要]** ブレードで、**[コンソール]** を選択します。 これにより Azure Redis Cache コンソールが開き、低レベルの Azure Redis Cache コマンドを入力できます。

1. 「**get MyKey1**」と入力します。 返される値が **MyValue1** であることを確認します。

1. 「**get MyKey2**」を入力します。 返される値が **MyValue2** であることを確認します。

    ![MyKey1 と MyKey2 の値を示す Azure Redis Cache コンソールのスクリーンショット。](../media/4-redis-console.png)