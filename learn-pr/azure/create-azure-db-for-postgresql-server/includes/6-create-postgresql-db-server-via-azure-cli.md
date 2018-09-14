ランナーのフィットネス機器からキャプチャされたルートを格納する、Azure Database for PostgreSQL サーバーを作成すること。 20 GB に設定して、サーバー記憶域要件がわかって履歴のキャプチャされたデータ ボリュームに基づき。 処理の要件をサポートするには、1 仮想コアと Gen 5 のサポートを計算する必要があります。 データのバックアップは、15 日の保有期間を要求することもわかります。

始めましょう。

を検討してください 20 GB に、サーバー ストレージ サイズを設定し、1 仮想コアと 15 日のデータのバックアップのリテンション期間と Gen 5 のサポートを計算します。

いくつかのパラメーターを指定するにがあります。

- `--resource-group <resource_group_name>`
- `--name <new_server_name>`
- `--location <location>`
- `--admin-user <admin_user_name>`
- `--admin-password <server_admin_password>`
- `--sku-name <sku>`
- `--storage-size <size>`
- `--backup-retention <days>`
- `--version <version_number>`

かどうかは、コマンドの記述し、下のソリューションを見ることがなく完了、パラメーターを参照してください。 値を置き換える`<>`独自の値を使用します。

> [!NOTE]
> このコマンドを使用してすべての場所の一覧を取得する`az account list-locations`します。 選択、`displayName`または`name`値し、使用するため、`<location>`パラメーター。

```bash
az postgres server create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
```

実行時に情報の処理にしばらく時間がかかるシステムが表示されます。 サーバーが作成された場合は、サーバーを説明する JavaScript Object Notation (JSON) 文字列が返されます。 サーバーが作成されていない場合は、エラー メッセージが表示されます。 このエラー情報を使用を確認し、コマンド パラメーターを修正します。

Azure CLI を使用して PostgreSQL サーバーを正常に作成されました。 次の単位では、サーバーのセキュリティ設定を構成する方法を確認します。
