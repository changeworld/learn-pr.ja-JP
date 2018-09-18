アプリの準備ができたので、次に操作するために Azure ストレージ アカウントが必要です。 Azure ストレージ アカウントは、「**Azure ストレージ アカウントの作成**」モジュールで Azure portal を使って作成しました。 今度は、Azure CLI を使ってみましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a>Azure CLI を使用して Azure ストレージ アカウントを作成する

`az storage account create` コマンドを使用して、新しいストレージ アカウントを作成します。 このコマンドには、望みどおりのストレージ アカウントを構成する上で指定する必要がある (または指定すべきである) パラメーターがいくつかあります。

> [!div class="mx-tableFixed"]
> | オプションオプション | 説明 |
> |--------|-------------|
> | `--name` | **ストレージ アカウント名**です。 この名前は、アカウント内のデータへのアクセスで使用されるパブリック URL を作成するのに使用されます。 Azure 内の既存のすべてのストレージ アカウントを通して一意である必要があります。 文字数は 3 から 24 文字にする必要があります。使用できる文字は英小文字と数字のみです。 |
> | `--resource-group` | <rgn>[サンド ボックス リソース グループ名]</rgn> を使用して、ストレージ アカウントを無料のサンド ボックスに配置します。 |
> | `--location` | お近くの場所を選択します。 |
> | `--kind` | これにより、ストレージ アカウントの_種類_が決まります。 オプションには、BlobStorage、Storage、StorageV2 があります。 |
> | `--sku` | これにより、ストレージ アカウントのパフォーマンスとレプリケーション モデルが決まります。 オプションには、Premium_LRS、Standard_GRS、Standard_LRS、Standard_RAGRS、Standard_ZRS があります。 |
> | `--access-tier` | **アクセス層**は BLOB ストレージでのみ使用されます。使用できるオプションはクールとホットです。 **ホット アクセス層**は頻繁にアクセスされるデータに最適です。**クール アクセス層**はアクセス頻度の低いデータに適しています。 これによって設定されるのは_既定_値のみであることに注意してください。BLOB を作成する場合は、データに対して異なる値を設定できます。 |
    
上記の表を使用して、Cloud Shell 内の右側にコマンド行を作成することで、アカウントを作成できます。
- 一意の名前を使用します。"photostore" と自分のイニシャルとランダムな数字を組み合わせたような名前にすることをお勧めします。 名前が一意ではない場合は、エラーが発生します。
- 通常は、ご自分のアプリ リソースを保持するリソース グループを新規に作成します。この場合は、サンドボックス リソース グループを使用します。
- **sku** には "Standard_LRS" を使用します。これには、この例に適している、ローカル レプリケーションを備えた Standard Storage が使用されます。
- **アクセス層**には "コールド" を使用します。

### <a name="selecting-a-location"></a>場所の選択
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a>コマンドの例

```bash
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cold
```

> [!TIP]
> ストレージ アカウント用のオプションの詳細に関心がある場合は、オプションの内容を掘り下げて説明している「**Azure ストレージ アカウントの作成**」を参照してください。

アカウントがデプロイされるまで数分かかります。 Azure がそれに対して動作している間、このアカウントで使用する API について見てみましょう。