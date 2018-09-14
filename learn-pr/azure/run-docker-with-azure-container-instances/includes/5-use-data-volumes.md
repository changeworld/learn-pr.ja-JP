既定では、Azure Container Instances はステートレスです。 コンテナーがクラッシュまたは停止すると、すべての状態が失われます。 コンテナーの有効期間後も状態を保持するには、外部ストアからボリュームをマウントする必要があります。

ここでは、Azure コンテナー インスタンスのデータの保存と取得に Azure ファイル共有がマウントされます。

## <a name="create-an-azure-file-share"></a>Azure ファイル共有を作成する

次のスクリプトを実行して、ストレージ アカウントを作成します。 ストレージ アカウント名はグローバルに一意にする必要があるため、このスクリプトではベース文字列にランダムな値が追加されます。

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group <rgn>[Sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --sku Standard_LRS
```

次のコマンドを実行して、*AZURE_STORAGE_CONNECTION_STRING* という名前の環境変数にストレージ アカウントの接続文字列を設定します。 この環境変数は、Azure CLI によって認識され、ストレージ関連の操作で使用できます。

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

`az storage share create` コマンドを実行してファイル共有を作成します。 次の例では、*aci-share-demo* という名前の共有を作成します。

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a>ストレージの資格情報を取得する

次の 3 つの値が必要では、Azure Container Instances のボリュームとして Azure ファイル共有をマウントする: ストレージ アカウント名、共有名、およびストレージ アカウント アクセス キー。

上のスクリプトを使用した場合、ストレージ アカウント名は最後にランダムな値を付加して作成されています。 最後の文字列 (ランダムな部分を含む) のクエリを実行するには、次のコマンドを使用します。

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[Sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

共有名は既にわかっているので (aci-share-demo)、あとはストレージ アカウント キーです。このキーを見つけるには、次のコマンドを使用します。

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[Sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>コンテナーをデプロイしてボリュームをマウントする

Azure ファイル共有をコンテナーのボリュームとしてマウントするには、コンテナーを作成するときに、共有とボリュームのマウント ポイントを指定します。

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
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
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo-files --query ipAddress.ip -o tsv
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 シンプルなフォームが表示されます。 いくつかのテキストを入力して、**[送信]** をクリックします。 これにより、ここで入力したテキストを本体とするファイルが Azure Files 共有で作成されます。

![Azure Container Instances のファイル共有のデモ](../media-draft/files-ui.png)

検証するには、Azure portal でファイル共有に移動し、ファイルをダウンロードします。

![コンテンツ デモ アプリケーションのサンプル テキスト ファイル](../media-draft/sample-text.png)

Azure Files 共有に格納されているファイルとデータに何らかの価値がある場合は、この共有を新しいコンテナー インスタンスに再マウントして、ステートフルなデータを提供できます。