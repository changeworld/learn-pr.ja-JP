次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a>リソース グループを使用

独自のマシンと Azure サブスクリプションを使用している場合は、最初に `az login` コマンドを使用して Azure にログインする必要があります。 クラウド シェル環境ではこの作業は不要です。

次に、通常は `az group create` コマンドを使用して関連するすべての Azure リソース用にリソース グループを作成しますが、以下の演習で使用するリソース グループは既に作成されています。 リソース グループに **<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。

1. テーブルにすべてのリソース グループを一覧表示するよう Azure CLI に指示できます。 無料の Azure サンドボックスを使用している場合、リソース グループは 1 つしかありません。

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Azure で開発を行っていると、複数のリソース グループができることがあります。 グループ リスト内に複数のアイテムがある場合は、`--query` オプションを追加することで、返される値をフィルター処理できます。 次のコマンドを試してください。

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    クエリは、JSON 要求のための標準クエリ言語である **JMESPath** を使用して書式設定されます。 この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。 **Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。

### <a name="steps-to-create-a-service-plan"></a>サービス プランの作成手順

Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。 アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。

1. App Service プランを作成して、ご利用のアプリを実行します。 次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。

    > [!WARNING]
    > アプリとプランの名前は "_一意_" でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを一連の数字、自分のイニシャル、または他の何らかのテキストに置き換えて、Azure 全体で確実に一意になるようにします。

    `--location` パラメーターでは、以下のいずれかの場所の値を使用します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --location <location>
    ```

    このコマンドは、完了までに数分かかる場合があります。

1. テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Web アプリの作成手順

次に、サービス プランに Web アプリを作成します。 それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。

1. Web アプリをプラン作成し、前に作成したプランの名前を指定します。 **プランと同じようにアプリ名も一意でなければならない**ので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。

    ```azurecli
    az webapp list --output table
    ```

1. テーブルに表示された **DefaultHostName** をメモしておいてください。これは、新しい Web サイトのアクセス可能な Web アドレスです。 Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。 たとえば、アプリ名を "popupwebapp-mslearn123" とした場合、Web サイトの URL は `http://popupwebapp-mslearn123.azurewebsites.net` となります。

1. サイトには、Azure で作成された "クイック スタート" ページがあり、ブラウザーで、または CURL で単に **DefaultHostName** を使用して表示できます。

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a>GitHub からコードをデプロイする手順

1. 最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。 Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、 "HelloWorld!" と表示されます。 自分で作成した Web アプリ名を必ず使用します。

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. デプロイされたら、ブラウザーまたは CURL でサイトを再表示します。

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    ページに "Hello World!" と表示されます。

    ```output
    Hello World!
    ```

この演習では、対話型の Azure CLI セッションの典型的なパターンを示しました。 最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。 次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。 この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。