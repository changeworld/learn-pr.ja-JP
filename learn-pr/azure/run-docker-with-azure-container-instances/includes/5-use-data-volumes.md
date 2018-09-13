既定では、Azure Container Instances はステートレスです。 コンテナーがクラッシュまたは停止すると、すべての状態が失われます。 コンテナーの有効期間後も状態を保持するには、外部ストアからボリュームをマウントする必要があります。

このユニットでは、データの保管と取得のため、Azure コンテナー インスタンスに Azure ファイル共有をマウントします。

## <a name="create-an-azure-file-share"></a>Azure ファイル共有を作成する

Azure Container Instances で Azure ファイル共有を使用するには、まず、ファイル共有を作成する必要があります。 次のスクリプトを実行して、ストレージ アカウントを作成します。 ストレージ アカウント名はグローバルに一意にする必要があるため、このスクリプトではベース文字列にランダムな値が追加されます。

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

次のコマンドを実行して、環境変数 *AZURE_STORAGE_CONNECTION_STRING* にストレージ アカウントの接続文字列を設定します。 この環境変数は、Azure CLI によって認識され、ストレージ関連の操作で使用できます。

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

`az storage share create` コマンドを実行してファイル共有を作成します。 次の例では、*aci-share-demo* という名前の共有を作成します。

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>ストレージの資格情報を取得する

Azure Container Instances 内のボリュームとして Azure ファイル共有をマウントするには、ストレージ アカウント名、共有名、ストレージ アクセス キーの 3 つの値が必要です。

上のスクリプトを使用した場合、ストレージ アカウント名は最後にランダムな値を付加して作成されています。 最後の文字列 (ランダムな部分を含む) のクエリを実行するには、次のコマンドを使用します。

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

共有名は既にわかっているので (aci-share-demo)、あとはストレージ アカウント キーです。このキーを見つけるには、次のコマンドを使用します。

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>コンテナーをデプロイしてボリュームをマウントする

Azure ファイル共有をコンテナーのボリュームとしてマウントするには、コンテナーを作成するときに、共有とボリュームのマウント ポイントを指定します。

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

コンテナーが作成された後、パブリック IP アドレスを取得します。

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 シンプルなフォームが表示されます。 いくつかのテキストを入力して、**[送信]** をクリックします。 これにより、ここで入力したテキストを本体とするファイルが Azure Files 共有で作成されます。

![Azure Container Instances のファイル共有のデモ](../media-draft/files-ui.png)

検証するには、Azure portal でファイル共有に移動し、ファイルをダウンロードします。

![コンテンツ デモ アプリケーションのサンプル テキスト ファイル](../media-draft/sample-text.png)

Azure Files 共有に格納されているファイルとデータに何らかの価値がある場合は、この共有を新しいコンテナー インスタンスに再マウントして、ステートフルなデータを提供できます。


## <a name="summary"></a>まとめ

このユニットでは、Azure Files 共有とコンテナーを作成し、そのコンテナーにファイル共有をマウントしました。 この共有を使用して、アプリケーション データを格納しました。

次のユニットでは、Container Instances の一般的なトラブルシューティング操作について説明します。
