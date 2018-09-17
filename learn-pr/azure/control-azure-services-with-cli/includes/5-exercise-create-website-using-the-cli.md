次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。

自分の Azure サブスクリプションでインストールした Azure CLI を使用できますが、この演習に使用できる無料のサンド ボックス環境があり、その Azure Cloud Shell には Azure CLI が既にインストールされています。 次の手順では、その無料サンドボックスを使うものとします。

### <a name="create-a-resource-group"></a>リソース グループを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> 通常は、`az group create ...` コマンドを使用して関連するすべての Azure リソース用にリソース グループを作成しますが、以下の演習で使用するリソース グループは既に作成されています。 このリソース グループの名前が、この演習の以降のコマンドに挿入されます。

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. すべてのリソース グループをテーブルに覧表示して、リソース グループが正常に作成されたことを確認します。

    ```azurecli
    az group list --output table
    ```

    無料のサンドボックスで使用するために生成された `<rgn>[Sandbox resource group name]</rgn>` リソース グループのみを表示できます。

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. Azure で開発を行っていると、複数のリソース グループができることがあります。 グループ リスト内に複数のアイテムがある場合は、`--query` オプションを追加することで、返される値をフィルター処理できます。 次のコマンドを試してください。

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    クエリは、JSON 要求のための標準クエリ言語である **JMESPath** を使用して書式設定されます。 この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。 **Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。

### <a name="steps-to-create-a-service-plan"></a>サービス プランの作成手順

Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。 アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. App Service プランを作成して、ご利用のアプリを実行します。 次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。

    > [!WARNING]
    > アプリとプランの名前は "_一意_" でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを一連の数字、自分のイニシャル、または他の何らかのテキストに置き換えて、Azure 全体で確実に一意になるようにします。

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    このコマンドは、完了までに数分かかる場合があります。

1. テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Web アプリの作成手順

次に、サービス プランに Web アプリを作成します。 それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。

1. Web アプリをプラン作成し、前に作成したプランの名前を指定します。 **プランと同じようにアプリ名も一意でなければなりませんので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。**
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。

    ```azurecli
    az webapp list --output table
    ```

1. **DefaultHostName** を書き留めてください。これは後で必要になります。

### <a name="steps-to-deploy-code-from-github"></a>GitHub からコードをデプロイする手順

1. 最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。 Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、 "HelloWorld!" と表示されます。 自分で作成した Web アプリ名を必ず使用します。

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. コードがデプロイされると、Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。 たとえば、アプリ名を "popupapp mslearn123" とした場合、Web サイトのアドレスは <http://popupapp-mslearn123.azurewebsites.net> となります。 特定のインスタンスにアクセスするには、正しい URL を入力する必要があります。

1. ページに "Hello World!" と表示されます。

1. ブラウザー ウィンドウを閉じます。

この演習では、対話型の Azure CLI セッションの典型的なパターンを示しました。 最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。 次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。 この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。
