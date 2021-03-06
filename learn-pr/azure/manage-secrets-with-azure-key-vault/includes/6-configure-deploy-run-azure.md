次に Azure でアプリを実行します。 Azure App Service アプリを作成し、マネージド ID とコンテナー構成を使用してそのアプリを設定してから、コードをデプロイする必要があります。

## <a name="create-the-app-service-plan-and-app"></a>App Service のプランとアプリを作成する

App Service アプリの作成は 2 段階のプロセスです。最初に*プラン*を作成し、次に*アプリ*を作成します。

*プラン*名は、サブスクリプション内においてのみ一意であればかまいません。そのため、**keyvault-exercise-plan** という使用済みの名前と同じ名前を使用できます。 一方、アプリ名はグローバルに一意である必要があり、そのため独自の名前を選ぶ必要があります。 コンテナーを作成するときに使用したのと同じ場所を使用します。

Azure Cloud Shell で次を実行します。

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    --location eastus

az webapp create \
    --name <your-unique-app-name> \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

## <a name="add-configuration-to-the-app"></a>アプリに構成を追加する

Azure へのデプロイでは、構成ファイルではなくアプリケーションの設定内に VaultName 構成を配置するという App Service のベスト プラクティスに従います。 次のコマンドを実行して、アプリケーション設定を作成します。

```azurecli
az webapp config appsettings set \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a>マネージド ID を有効にする

アプリ上でマネージド ID を有効にする処理は 1 行で済みます。次の内容を実行することで、ご利用のアプリ上で有効になります。

```azurecli
az webapp identity assign \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

JSON の出力結果から、**principalId** の値をコピーします。 principalId は、Azure Active Directory 内にある、アプリの新しい ID を表す一意の ID です。次の手順で使用します。

## <a name="grant-access-to-the-vault"></a>コンテナーへのアクセスを許可する

デプロイする前に行う最後の手順は、Key Vault アクセス許可をご利用のアプリのマネージド ID に割り当てることです。 前の手順でコピーした **principalId** の値を、以下のコマンドの **object-id** の値として使用します。 このコマンドを実行すると、**Get** および **List** に以下へのアクセス許可が付与されます。

```azurecli
az keyvault set-policy \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid> \
    --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a>アプリをデプロイし、試してみる

すべての構成が設定され、デプロイする準備ができました。 以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。

> [!NOTE]
> `cd` で KeyVaultDemoApp ディレクトリに戻る必要があります (まだそうしていない場合)。

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

サイトがデプロイされたことを示す結果が表示されたら、`https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` をブラウザーで開きます。 シークレットの値として **reindeer_flotilla** が表示されます。

アプリが完成し、デプロイされました。