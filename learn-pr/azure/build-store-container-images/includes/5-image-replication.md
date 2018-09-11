ベスト プラクティスとして、イメージが実行されている各リージョンにコンテナー レジストリを配置してネットワーク上の近い場所での操作を可能にすることで、高速で信頼性の高いイメージ レイヤー転送を実現します。 geo レプリケーションにより、Azure コンテナー レジストリが単一のレジストリとして機能することが可能になり、マルチマスター リージョン レジストリで複数のリージョンに対応できます。

geo レプリケートされたレジストリには次の利点があります。

* 複数のリージョンで 1 つのレジストリ/イメージ/タグ名を使用できる
* リージョン デプロイからネットワーク上の近い場所のレジストリにアクセスできる
* コンテナー ホストと同じリージョンにあるレプリケートされたローカルのレジストリからイメージがプルされるので、追加のエグレス料金が発生しない
* 複数のリージョンにまたがってレジストリを一元管理できる

## <a name="replicate-image-to-multiple-locations"></a>イメージを複数の場所にレプリケートする

`az acr replication create` コマンドを使用して、コンテナー イメージを別のリージョンにレプリケートします。 この例では、`japaneast` リージョン用のレプリケーションが作成されます。 `<acrName>` を、コンテナー レジストリの名前で更新します。

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

出力は次のようになります。

```console
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

すべてのコンテナー イメージ レプリカの一覧を表示するには、`az acr replication list` コマンドを使用します。 `<acrName>` を、コンテナー レジストリの名前で更新します。

```azurecli
az acr replication list --registry <acrName> --output table
```

出力は次のようになります。

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

この演習では Azure Portal を使用しませんでしたが、Portal のエクスペリエンスを確認しておくとよいでしょう。

Azure Portal 内から、Azure コンテナー レジストリに対して `Replications` を選択すると、現在のレプリケーションの詳細を示すマップが表示されます。 マップ上のリージョンを選択すると、追加のリージョンにコンテナー イメージをレプリケートできます。

![Azure Portal に表示されるコンテナー レプリケーション マップ](../media/replication-map.png)

## <a name="clean-up"></a>クリーンアップ

これは、Azure Container Registry のラーニング モジュールの最後のユニットです。 この時点で、リソース グループを削除して、作成したリソースをクリーンアップすることができます。 そのためには、`az group delete` コマンドを使用します。

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a>まとめ

このモジュールでは、Azure CLI を使用して、コンテナー イメージを複数の Azure リージョンにレプリケートしました。