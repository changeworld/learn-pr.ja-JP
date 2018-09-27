最初に必要な作業は、使用する空の Azure Cosmos DB データベースとコレクションを作成することです。 このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。 以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a>Azure CLI を使用して Azure Cosmos DB アカウントとデータベースを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a>サブスクリプションを選択する

Azure をしばらく使用したことがある場合は、複数のサブスクリプションを利用できるかもしれません。 たとえば、開発者が Visual Studio 用のサブスクリプションと会社のリソース用の別のサブスクリプションを持っているのはよくあることです。

Azure サンドボックスでは既に Cloud Shell でお客様用のコンシェルジェ サブスクリプションが選択されており、次の手順を使用してサブスクリプションの設定を検証できます。 または、お客様が自分のサブスクリプションで作業するのであれば、Azure CLI を使用して以下の手順でサブスクリプションを切り替えることができます。

1. 最初に、利用可能なサブスクリプションの一覧を表示します。

    ```azurecli
    az account list --output table
    ```

   コンシェルジェ サブスクリプションをご利用の場合は、一覧にはそれだけが表示されます。

1. 次に、既定のサブスクリプションを使いたくない場合は、`account set` コマンドで変更できます。

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. サンドボックスによってお客様用に作成されたリソース グループを取得します。 ご自分のサブスクリプションを使用する場合は、この手順をスキップして、使用する一意の名前を下の `RESOURCE_GROUP` 環境変数に指定します。 リソース グループ名をメモしておきます。 ここにデータベースを作成することになります。

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a>環境変数を設定する

1. いくつかの環境変数を設定すると、毎回共通の値を入力しないで済むようにできます。 まず、Azure Cosmos DB アカウントに名前を付けます。たとえば、「`export NAME="mycosmosdbaccount"`」とします。 このフィールドには小文字、数字、'-' 文字のみを含めることができます。文字数は 3 から 31 文字にする必要があります。

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. 既存のサンドボックス リソース グループを使用するようにリソース グループを設定します。

    ```azurecli
    export RESOURCE_GROUP="<rgn>[sandbox resource group name]</rgn>"
    ```

1. 最も近いリージョンを選択し、`export LOCATION="EastUS"` など、環境変数を設定します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. データベース名の変数を設定します。 名前を "Products" と付けて、直前のモジュールで作成したデータベースと一致するようにします。

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a>サブスクリプションにリソース グループを作成する

ご自分のサブスクリプションに Cosmos DB を作成する場合は、すべての関連リソースを保持するための新しいリソース グループを作成します。

> [!IMPORTANT]
> Microsoft Learn 提供の Azure サンドボックスを使用している場合は、この手順を行う必要はありません。 代わりに、上の `RESOURCE_GROUP` 変数に **<rgn>[サンドボックス リソース グループ名]</rgn>** が設定されていることを確認します。

ご自分のサブスクリプションの場合は、次のコマンドを使用してリソース グループを作成します。 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントを作成する

1. `cosmosdb create` コマンドを使用して Azure Cosmos DB アカウントを作成します。 このコマンドは次のパラメーターを使用し、推奨される環境変数を設定した場合は変更なしで実行できます。
    - `--name`: リソースの一意名です。
    - `--kind`: データベースの種類です。_GlobalDocumentDB_ を使用します。
    - `--resource-group`: リソース グループです。 **<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。 この値が `RESOURCE_GROUP` 変数に割り当てられている必要があります。

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    コマンドが完了するまでには数分かかり、新しいアカウントが展開されると Cloud Shell にその設定が表示されます。

1. アカウントで `Products` データベースを作成します。

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. 最後に、`Clothing` コレクションを作成します。

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

これで、Azure Cosmos DB アカウント、データベース、コレクションができあがりました。データを追加しましょう。