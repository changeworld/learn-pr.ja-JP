アプリをしたら、Azure ストレージ アカウントを使用する必要があります。 Azure portal を使用して作成した、 **Azure ストレージ アカウントを作成**モジュール。 この時点の Azure CLI を使用してみましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Azure CLI を使用して、Azure ストレージ アカウントを作成するには

使用して、`az storage account create`新しいストレージ アカウントを作成するコマンド。 いずれかを指定する必要があります (またはする必要があります) をいくつかのパラメーターを受け取るように、希望方法を構成します。

> [!div class="mx-tableFixed"]
> | オプション | 説明 |
> |--------|-------------|
> | `--name` | A**ストレージ アカウント名**します。 名前は、アカウント内のデータへのアクセスに使用されるパブリック URL を生成するためには。 既存のすべてのストレージ アカウント名が Azure の間で一意である必要があります。 長さは 3 ~ 24 文字である必要があり、小文字と数字のみを含めることができます。 |
> | `--resource-group` | 使用<rgn>[サンド ボックス リソース グループ名]</rgn>無料のサンド ボックスに、ストレージ アカウントを配置します。 |
> | `--location` | (以下参照)、近くの場所を選択します。 |
> | `--kind` | これは、ストレージ アカウントを決定します。_型_します。 オプションがあります。 `BlobStorage`、 `Storage`、および`StorageV2`します。 |
> | `--sku` | これは、ストレージ アカウントのパフォーマンスとレプリケーションのモデルを決定します。 オプションがあります。 `Premium_LRS`、 `Standard_GRS`、 `Standard_LRS`、 `Standard_RAGRS`、および`Standard_ZRS`します。 |
> | `--access-tier` | **アクセス層**が使用できるオプションは [で Blob storage の使用、のみ`Cool` | `Hot`]. **ホット アクセス層**は頻繁にアクセスされるデータに最適です、**クール アクセス層**アクセス頻度の低いデータをお勧めします。 設定のみに注意してください、_既定_値&mdash;Blob を作成するときに、データの別の値を設定できます。 |
    
アカウントを作成するのにには、右に Cloud Shell のコマンドラインを作成するのにには、上記の表を使用します。
- 一意の名前を使用してください。 自分のイニシャルとランダムな番号"photostore"のようなものをお勧めします。 一意でない場合、エラーが発生します。
- 通常は、アプリのリソースを保持するが、ここでは、サンド ボックスのリソース グループを使用する新しいリソース グループを作成します。
- "Standard_LRS"を使用して、 **sku**します。 この例の問題はローカル レプリケーションで、standard storage を使用これは。
- 「コールド」を使用して、**アクセス層**します。

### <a name="selecting-a-location"></a>場所の選択
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>コマンドの例

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cool
```

> [!TIP]
> ストレージ アカウントのオプションを調べる場合は、ことを経由することを確認して、 **Azure ストレージ アカウントを作成**どこでそれらの詳細。

アカウントをデプロイするまで数分をかかります。 Azure が動作しているときにこのアカウントを使用して Api を見てみましょう。