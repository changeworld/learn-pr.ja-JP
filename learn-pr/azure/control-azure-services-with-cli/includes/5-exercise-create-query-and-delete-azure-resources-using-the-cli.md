
この演習では、ローカル コンピューター上で Azure CLI を使用してリソース グループを作成し、このリソース グループに Web アプリをデプロイします。 

### <a name="steps-to-create-a-resource-group"></a>リソース グループの作成手順
1. Linux または macOS 上で Bash Shell を開くか、Windows から作業している場合は、コマンド プロンプト ウィンドウまたは PowerShell を開きます。

1. Azure CLI を起動し、ログイン コマンドを実行します。

    ```bash
    az login
    ```
    Web ブラウザーで Azure のサインイン ページが表示されない場合は、コマンド ラインの手順に従って、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) に認証コードを入力します。

1. リソース グループを作成します。

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. テーブルにすべてのリソース グループを一覧表示して、リソース グループが正常に作成されたことを確認します。

    ```bash
    az group list --output table
    ```
1. 必要に応じて、Azure portal でリソースが作成されたことを確認します。 Web ブラウザーを開き、ポータルにサインインし、**リソース グループ**のセクション (下記参照) に移動します。 新しいリソース グループが一覧に表示されるはずです。

![ポータルを使用したリソース グループの一覧表示](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a>サービス プランの作成手順
Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。 アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。

1. アプリを実行するための App Service プランを作成します。 アプリとプランの名前は一意である必要があります。そのため、文字列 "12345" をランダムな数字に変更してください。 次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. テーブルにすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Web アプリの作成手順
次に、サービス プランに Web アプリを作成します。 コードを同時に展開することができますが、この例では、別の手順として行います。

1. Web アプリを作成します。文字列 "12345" を、以前に使用したものと同じランダムな数字に変更することを忘れないでください。
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. テーブルにすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。

    ```bash
    az webapp list --output table
    ```

1. **DefaultHostName** を書き留めてください。これは後で必要になります。

### <a name="steps-to-deploy-code-from-github"></a>GitHub からコードを展開する手順
1. 最後の手順は、GitHub リポジトリから Web アプリへのコードの展開です。ここでも、文字列 "12345" を、以前に使用したものと同じランダムな数字に変更することを忘れないでください。
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Web アプリを表示するには、ブラウザーに次の URL をコピーします。
http://popupapp12345.azurewebsites.net

1. ページに "HelloWorld!" と表示されます

1. ブラウザー ウィンドウを閉じます。

## <a name="summary"></a>まとめ
この演習では、対話型の Azure CLI セッションの典型的なパターンが示されています。 最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。 次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。 この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。
