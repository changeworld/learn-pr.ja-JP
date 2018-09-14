ここで Azure Redis Cache のインスタンスを作成して起動し、キャッシュに 2 つのデータ値を挿入する単純なトランザクションを作成します。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a>Azure Redis Cache の作成

Azure CLI を使用した Azure Redis Cache を作成してみましょう。 Azure とやり取りするブラウザー ウィンドウの右側にある Cloud Shell を使用します。

使用して、`azure rediscache create`新しい Azure Redis Cache を作成するコマンド。 いくつかのパラメーターを受け取ります。 最も一般的な (完全な一覧取得のマニュアルを参照する) を示します。

> [!div class="mx-tableFixed"]
> | パラメーター | 説明 |
> |-----------|-------------|
> | `--name`    | -キャッシュの名前グローバルに一意の文字、数字、ダッシュで構成されをする必要がありますこれ。 |
> | `--resource-group` | 事前に作成したリソース グループを使用して、 <rgn>[サンド ボックス リソース グループ名]</rgn>、Azure サンド ボックスの一部です。 |
> | `--location` | キャッシュが配置される場所を指定します。 通常、データ コンシューマーに近い場所を選択するされます。 この場合は、Azure サンド ボックス内で使用できる場所に制限しています。 最も近いものを選択します。 |
> | `--size` | Azure Redis Cache のサイズ。 有効な値は、[C0、C1、C2、C3、C4、C5、C6、P1、P2、P3、P4] です。 |
> | `--sku` | Azure Redis Cache の SKU。 有効な値は、[Basic、Standard、Premium] です。 |
> | `--enable-non-ssl-port` | キャッシュの非 SSL ポートを有効にする場合は、このフラグを追加します。 |

### <a name="selecting-a-location"></a>場所の選択
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. 次のオプションを使用してキャッシュを作成します。
    - サイズ: C0
    - SKU: 基本
    - EnableNonSslPort
    
1. コマンドラインの例を次に示します。 置き換えてください、 **[name]** と **[場所]** を有効な値。

    ```azurecli
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

キャッシュの作成には数分をかかります。

## <a name="create-a-net-core-console-application"></a>.NET Core コンソール アプリケーションを作成する

次に、Azure Redis Cache にデータ値を挿入するために使用する .NET Core c# ベースのコンソール アプリケーションを作成します。

1. ウィンドウの右側に統合された Cloud Shell を使用して、新しい .NET Core アプリケーションを作成します。 "RedisData"名前を付けます。

    ```bash
    dotnet new console --name RedisData
    ```
    
1. アプリの作成、新しいディレクトリに変更します。

    ```bash
    cd RedisData
    ```
    
1. プロジェクト ファイルでは、および 1 つを検索する必要があります**Program.cs**ソース ファイル。

1. 「こんにちは, World!」を出力する必要があります - をビルドして、アプリケーションの実行

    ```bash
    dotnet run
    ```
    
## <a name="add-the-servicestackredis-nuget-package"></a>ServiceStack.Redis NuGet パッケージを追加します。

あるので、コンソール アプリケーションを追加する必要があります、 **ServiceStack.Redis** NuGet パッケージ。 これにより、c# でのコマンドを発行、Azure Redis Cache に接続することができます。

1. NuGet パッケージを追加**ServiceStack.Redis**ターミナル シェルを使用します。

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. ビルドして、すべてコンパイルかどうかを確認するには、もう一度アプリケーションを実行します。 「こんにちは, World!」とも出力されます。

## <a name="get-your-azure-redis-cache-connection-string"></a>Azure Redis Cache 接続文字列を取得します。

Azure Redis Cache に接続するには、パスワード、および URL を含む接続文字列必要があります。 この接続文字列は一意に**ServiceStack.Redis**の形式であり:`[password]@[host name]:[port]`します。

Azure portal またはコマンドラインを使用した、このキーを取得できます。 「Redis で読み取り専用のデータをキャッシュすることによって、web アプリケーションの最適化」モジュールでポータルによるアプローチを使用したため、後者のここを使用してみましょう。

使用して、`azure rediscache list-keys`アクセス キーを取得するコマンド。 次に示すよう、名とリソース グループを指定する必要は。

```azurecli
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

次のようなものが戻ります。

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. コピー、**プライマリ**キーをクリップボードにします。

1. ホスト名は、サフィックスを持つ、作成したときに、キャッシュに付けた名前になります`.redis.cache.windows.net`します。

1. ポートは**6379**します。

1. すべてのコンソールにその情報を確認することができます、`azure rediscache list`コマンド。このコマンドによって、(この例では、Azure サンド ボックス) では、アクティブなサブスクリプションで Azure Redis Cache のすべてのインスタンスがわかります。

## <a name="add-the-connection-string-to-your-app"></a>アプリへの接続文字列を追加します。

1. アプリ フォルダーにいることを確認します。 **Program.cs**を入力する場合、現在のフォルダーにあります`ls`または`dir`します。

1. 組み込みのエディターを開く」と入力して`code .`アプリ フォルダーにします。

1. 選択、 **Program.cs**ソース ファイル。

1. 次のフィールドを作成、`Program`クラス。

    ```csharp
    static string redisConnectionString = "";
    ```

1. 接続文字列での貼り付け、 **redisConnectionString**で作成した、フィールドし、使用**6379**として使用するポート番号。 形式で、接続文字列は、注意してください:`[password]@[host name]:[port]`します。

接続文字列の例は次のようになります。

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## <a name="insert-two-data-values-into-your-azure-redis-cache"></a>Azure Redis Cache に 2 つのデータ値を挿入します。

最後に、Azure Redis Cache にデータを追加お見せします。

1. 次の追加`using`ステートメントの先頭に、 **Program.cs**ファイル。

    ```csharp
    using ServiceStack.Redis;
    ```

1. 次のコードのスニペットを追加、 **Main**メソッド。 これにより、2 つの値が整合性追加されます。

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
1. エディター ウィンドウの下部にあるコマンド プロンプトでアプリケーションを実行し、コンソールに表示されることを確認**トランザクションがコミットされた**します。 

    ```bash
    dotnet run
    ```
    
## <a name="verify-your-data"></a>データの検証

を完了するには、データを追加しましたが、Azure Redis Cache をしましょうを確認します。

1. [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。

1. 選択して、Azure Redis Cache を見つけます**すべてのリソース**左側のサイドバーと左側にフィルター ボックスを使用した Azure Redis Cache インスタンスを選択します。 または、上部にある検索ボックスを使用し、キャッシュの名前を入力できます。

1. Azure Redis Cache インスタンスを選択します。

1. **概要**Azure Redis Cache、選択ブレード**コンソール**します。 これにより、低レベルの Azure Redis Cache コマンドを入力することができます、Azure Redis Cache コンソールが開きます。

1. 型**取得 MyKey1**します。 返される値があることを確認**MyValue1**します。

1. 型**取得 MyKey2**します。 返される値があることを確認**MyValue2**します。

    ![MyKey1 と MyKey2 の値を表示する Azure Redis Cache のコンソールのスクリーン ショット。](../media/4-redis-console.png)