次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。 

### <a name="create-a-resource-group"></a>リソース グループの作成

1. Linux または macOS 上では Bash Shell を開き、Windows から作業している場合は、コマンド プロンプト ウィンドウまたは PowerShell を開きます。

1. Azure CLI を起動し、ログイン コマンドを実行します。

    ```bash
    az login
    ```
    Web ブラウザーで Azure のサインイン ページが表示されない場合は、コマンド ラインの手順に従って、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) に認証コードを入力します。

1. リソース グループを作成します。

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. テーブルにご利用のリソース グループをすべて一覧表示して、リソース グループが正常に作成されたことを確認します。

    ```bash
    az group list --output table
    ```

> [!TIP]
> Azure portal でリソースが作成されたことを確認することもできます。 Web ブラウザーを開き、ポータルにサインインし、**[リソース グループ]** セクションに移動します。 新しいリソース グループが一覧に表示されるはずです。

1. グループ内にアイテムが多数含まれている場合は、`--query` オプションを追加することで、戻り値をフィルター処理することができます。次のコマンドを試してみてください。

    ```bash
    az group list --query '[?name == popupResGroup]'
    ```

    クエリは、JSON 要求を対象とした標準クエリ言語である **JMESPath** を使用してフォーマットされます。 この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。 **Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。

### <a name="steps-to-create-a-service-plan"></a>サービス プランの作成手順

Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。 アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。

1. App Service プランを作成して、ご利用のアプリを実行します。 次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。

    > [!WARNING]
    > アプリとプランの名前は_一意_でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを、Azure 全体で確実に一意になるように、一連の数値、自分のイニシャル、またはその他の何らかのテキストに置換します。 

    ```bash
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Web アプリの作成手順

次に、サービス プランに Web アプリを作成します。 それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。

1. Web アプリをプラン作成し、前に作成したプランの名前を指定します。 **プランと同じようにアプリ名も一意でなければなりませんので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。**
    ```bash
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。

    ```bash
    az webapp list --output table
    ```

1. **DefaultHostName** を書き留めてください。これは後で必要になります。

### <a name="steps-to-deploy-code-from-github"></a>GitHub からコードをデプロイする手順

1. 最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。 Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、 "HelloWorld!" と表示されます。 自分で作成した Web アプリ名を必ず使用します。

    ```bash
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. コードがデプロイされると、Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。 たとえば、アプリ名を "popupapp mslearn123" とした場合、Web サイトのアドレスは <http://popupapp-mslearn123.azurewebsites.net> となります。 使用する特定のインスタンスに達するには、正しい URL を入力する必要があります。

1. ページに "HelloWorld!" と表示されます。

1. ブラウザー ウィンドウを閉じます。

## <a name="summary"></a>まとめ

この演習では、対話型の Azure CLI セッションの典型的なパターンが示されています。 最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。 次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。 この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。
