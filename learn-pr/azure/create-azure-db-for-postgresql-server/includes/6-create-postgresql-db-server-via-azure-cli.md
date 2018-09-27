ランナーのフィットネス デバイスからキャプチャされたルートを格納するための Azure Database for PostgreSQL サーバーを作成します。 取得されたデータ ボリュームの履歴から、サーバー ストレージの要件を 20 GB に設定する必要があることがわかっています。 この処理要件をサポートするには、仮想コアを 1 個備えた Compute Gen 5 のサポートが必要です。 また、データのバックアップ用に 15 日間の保有期間が必要であることもわかっています。

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a>Azure CLI を使用して Azure PostGreSQL データベースを作成する

ストレージ サイズ 20 GB、1 仮想コアの Compute Gen 5 サポート、データ バックアップの保有期間 15 日に、サーバーを設定することに留意してください。

1. `az postgres server create` メソッドを使用して、新しいデータベースを作成します。 指定するパラメーターがいくつかあります。
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. 下のソリューションを見なくてもコマンドを記述してパラメーターを完了できるかどうかを確認してください。 ここでは、いくつかのヒントを紹介します。
    - `<values>` は実際の値に置き換えます。 
    - 繰り返しになりますが、サーバー名は、小文字アルファベットの 'a' から 'z'、数字の 0 から 9、ハイフンで構成されている必要があります。
    - <rgn>[サンドボックス リソース グループ名]</rgn> をリソース グループとして使用します。
    - 次の一覧の場所を使用します。   [!include[](../../../includes/azure-sandbox-regions-note.md)]
    
```bash
az postgres server create --name <unique_server_name> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \ 
    --location eastus \
    --sku-name B_Gen5_1 \
    --storage-size 20480 \
    --backup-retention 15 \
    --version 10 \
    --admin-user <admin_user_name> \
    --admin-password <server_admin_password>
```

実行時にはシステムによる情報の処理にしばらく時間がかかります。 コマンドが完了するまで待ってください。

完了すると、サーバーを記述する JavaScript Object Notation (JSON) 文字列が返されます。 エラーが発生した場合は、エラー メッセージが表示されます。 このエラー情報を使用してコマンド パラメーターを確認して修正した後でやり直してください。

JSON オブジェクトは次のようになります。

```json
{
  "administratorLogin": "azureuser",
  "earliestRestoreDate": "2018-09-17T00:35:50.170000+00:00",
  "fullyQualifiedDomainName": "secondserver8.postgres.database.azure.com",
  "id": "/subscriptions/xxxxx/resourceGroups/<rgn>[sandbox Resource Group]</rgn>/providers/Microsoft.DBforPostgreSQL/servers/secondserver8",
  "location": "eastus",
  "name": "secondserver8",
  "resourceGroup": "<rgn>[sandbox Resource Group]</rgn>",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 15,
    "geoRedundantBackup": "Disabled",
    "storageMb": 20480
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "10"
}
```

Azure CLI を使用して PostgreSQL サーバーを正常に作成しました。 次のユニットでは、サーバーのセキュリティ設定を構成する方法を説明します。
