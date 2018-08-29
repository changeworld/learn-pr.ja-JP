次に Azure でアプリを実行します。 Azure App Service アプリを作成し、MSI とコンテナー構成を使用してそのアプリを設定したうえで、コードをデプロイする必要があります。

## <a name="exercise"></a>演習

### <a name="create-the-app-service-plan-and-app"></a>App Service のプランとアプリを作成する

App Service アプリの作成は 2 段階のプロセスです。最初に*プラン*を作成し、次に*アプリ*を作成します。

*プラン*名は、サブスクリプション内においてのみ一意であればかまいません。そのため、**keyvault-exercise-plan** という使用済みの名前と同じ名前を使用できます。 一方、アプリ名はグローバルに一意である必要があり、そのため独自の名前を選ぶ必要があります。

Azure Cloud Shell で次のコマンドを実行します。

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

### <a name="add-configuration-to-the-app"></a>アプリに構成を追加する

Azure へのデプロイでは、App Service のベスト プラクティスに従い、構成ファイルではなくアプリケーションの設定に構成を配置します。

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

### <a name="enable-msi"></a>MSI を有効化する

アプリでの MSI の有効化は 1 行で指定できます。

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

JSON の出力結果から、**principalId** の値をコピーします。 principalId は、Azure Active Directory 内にある、アプリの新しい ID を表す一意の ID です。次の手順で使用します。

### <a name="grant-access-to-the-vault"></a>コンテナーへのアクセスを許可する

次に、アプリの ID に対して、運用環境の資格情報コンテナーからシークレットを取得して一覧表示するためのアクセス許可を付与する必要があります。 前の手順でコピーした **principalId** の値を、以下のコマンドの **object-id** の値として使用します。

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

### <a name="deploy-the-app-and-try-it-out"></a>アプリをデプロイし、試してみる

すべての構成が設定され、デプロイする準備ができました。 以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。

> [!NOTE]
> `cd` で KeyVaultDemoApp ディレクトリに戻る必要があります (まだそうしていない場合)。

```console
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

サイトがデプロイされたことを示す結果が表示されたら、`https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` をブラウザーで開きます。 シークレットの値として **reindeer_flotilla** が表示されます。

アプリが完成し、デプロイされました。