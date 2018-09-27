アプリの準備ができたので、次に操作するために Azure ストレージ アカウントが必要です。

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Azure CLI を使用して Azure ストレージ アカウントを作成する

`az storage account create` コマンドを使用して新しいストレージ アカウントを作成します。 ストレージ アカウントの構成を制御するパラメーターがいくつかあります。

> [!div class="mx-tableFixed"]
> | オプション | 説明 |
> |--------|-------------|
> | `--name` | **ストレージ アカウント名**です。 この名前は、アカウント内のデータへのアクセスで使用されるパブリック URL を作成するのに使用されます。 Azure に存在するストレージ アカウント名の中で一意になる名前を付ける必要があります。 名前の長さは 3 から 24 文字にする必要があり、小文字と数字のみを使用できます。 |
> | `--resource-group` | **<rgn>[サンド ボックス リソース グループ名]</rgn>** を使用し、ストレージ アカウントを無料のサンド ボックスに配置します。 |
> | `--location` | お近くの場所を選択します (下記参照)。 |
> | `--kind` | これにより、ストレージ アカウントの_種類_が決まります。 オプションには `BlobStorage`、`Storage`、`StorageV2` があります。 |
> | `--sku` | これにより、ストレージ アカウントのパフォーマンスとレプリケーション モデルが決まります。 オプションには `Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS` があります。 |
> | `--access-tier` | **アクセス層**は BLOB ストレージでのみ使用されます。使用できるオプションは [`Cool` \| `Hot`] です。 **ホット アクセス層**は頻繁にアクセスされるデータに最適です。**クール アクセス層**はアクセス頻度の低いデータに適しています。 これによって設定されるのは_既定_値のみであることに注意してください。BLOB を作成する場合は、データに対して異なる値を設定できます。 |
    
上記の表を使用して、Cloud Shell 内の右側にコマンド行を作成することで、アカウントを作成できます。
- 一意の名前を使用してください。 "photostore" と自分のイニシャルとランダムな数字を組み合わせたような名前にすることをお勧めします。 名前が一意ではない場合は、エラーが発生します。
- 通常は、自分のアプリ リソースを保持するリソース グループを新規に作成します。ただし、その場合、サンドボックス リソース グループ "**<rgn>[サンドボックス リソース グループ名]</rgn>**" を使用します。
- **sku** には "Standard_LRS" を使用します。 これには、この例に適している、ローカル レプリケーションを備えた Standard Storage が使用されます。
- **アクセス層**には "クール" を使用します。

### <a name="selecting-a-location"></a>場所の選択
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>コマンドの例

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool
```

> [!TIP]
> ストレージ アカウント用のオプションの詳細に関心がある場合は、オプションの内容を掘り下げて説明している「**Azure ストレージ アカウントの作成**」モジュールを参照してください。

アカウントがデプロイされるまで数分かかります。 Azure がそれに対して動作している間、このアカウントで使用する API について見てみましょう。
