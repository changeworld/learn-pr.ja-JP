オンプレミスの PostgreSQL データベースを使用するいると仮定します。 会社は、サーバーを Azure に移動することによって、デバイスのサポート、可用性、データの追跡、および処理の機能を展開するを見るようになりました。 Azure Database for PostgreSQL サーバーの作成を自動化にかかる労力を詳しく見ていきます。

単一の Azure Database for PostgreSQL サーバーを Azure portal を使用して作成するは簡単です。 1 つ以上のデータベースを作成して、ポータルのみを使用して継続的なメンテナンスを実行しているは、煩雑になる可能性があります。 Azure CLI を使用して、管理タスクを自動化する場合は、スクリプトを作成します。

Microsoft Azure 内のほぼすべてのリソースの作成を自動化できます Azure CLI を使用しています。 このユニットでは、Azure Database for PostgreSQL サーバーの Azure CLI の使用の管理を自動化する方法を学習します。

## <a name="what-is-the-azure-cli"></a>Azure CLI とは

[Azure CLI](https://docs.microsoft.com/cli/azure/)は Azure リソースを管理するための Microsoft のクロス プラットフォーム コマンド ライン環境です。 Azure CLI を使用するには、Azure Cloud Shell で、ブラウザーから、または Mac OS X、Linux、または Windows で Azure CLI をローカルにインストールできます。 Azure CLI は、bash または PowerShell を使用してローカルのコマンドラインから実行されます。 Azure CLI をローカルで実行するには、追加のセットアップが必要です。 Azure CLI コマンドを実行するため、Azure Cloud Shell を使用します。

## <a name="what-is-azure-cloud-shell"></a>Azure Cloud Shell とは

Azure Cloud Shell は、ブラウザー ベースのシェル エクスペリエンス、クラウドでホストされ、認証されたセッションを使用して Azure に接続することができますです。 Azure Database for PostgreSQL サーバーの管理を自動化する Azure CLI コマンドを実行することができます。 Azure CLI の一般的なツールがプレインストールされ、アカウントを使用するための Cloud Shell で構成されています。

> [!NOTE]
> Cloud Shell には、Azure storage のリソースを Cloud Shell で作業中に作成するすべてのファイルを保持する必要があります。 最初の起動時に Cloud Shell は、リソース グループとストレージ アカウントを作成するように求められ、Azure ファイル共有をします。 これは 1 回限りの手順であり、今後のすべての Cloud Shell セッションを自動的にアタッチされます。

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a>Azure Database for PostgreSQL サーバーの Azure CLI を使用した作成します。

Azure Database for PostgreSQL サーバーの Azure CLI を使用して作成するのに、右側に、Azure Cloud Shell のターミナルを使用します。

使用可能なすべてのパラメーターを示す Azure CLI server 作成コマンド使用状況ヘルプは、次の例のようになります。

   ```azurecli
   az postgres server create [-h] [--verbose] [--debug]
                             [--output {json,jsonc,table,tsv}]
                             [--query JMESPATH]
                              --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                              --sku-name SKU_NAME [--location LOCATION]
                              --admin-user ADMINISTRATOR_LOGIN
                              [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                              [--backup-retention BACKUP_RETENTION]
                              [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                              [--ssl-enforcement {Enabled,Disabled}]
                              [--storage-size STORAGE_MB]
                              [--tags [TAGS [TAGS ...]]]
                              [--version VERSION]
                              [--subscription _SUBSCRIPTION]

   ```

次のコマンド ラインでは、必要な Azure Database for PostgreSQL サーバーを作成するパラメーターのセットを示します。 いくつかのパラメーターは省略可能と記載されていないことがわかります。

   ```azurecli
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>パラメーターの説明

`--resource-group <resource_group_name>`パラメーターは、サーバーを作成するリソース グループを指定します。

サーバー`admin-user`と`admin-password`サーバーとそのデータベースにサインインするために必要なを指定することができます。 新しいサーバーと対話するときに後でこの情報を記録したりします。

使用する、`--sku-name`価格レベルの一部をここで指定するには、コンピューティング リソースのパラメーター。 値は規則に従って`{pricing tier}_{compute generation}_{vCores}`します。

次に例を示します。

- `--sku-name B_Gen4_4` は、"Basic、Gen 4、および 4 個の仮想コア" にマップされます。
- `--sku-name GP_Gen5_32` は、"汎用、Gen 5、および 32 個の仮想コア" にマップされます。
- `--sku-name MO_Gen5_2` は、"メモリ最適化、Gen 5、および 2 個の仮想コア" にマップされます。

ポータルを使用してサーバーを作成した単位の 3 つの価格レベルを説明したことを思い出してください。

Basic、汎用の 5 を使用して、1 仮想コアのコンピューティング リソースと仮定します。 指定します、パラメーターとして`--sku-name B_Gen5_1`します。

使用する、`--storage-size`の価格レベルを指定するパラメーター。 値が指定されていない場合、既定で 5,120 MB にします。 有効なストレージの 5,120 MB の範囲のサイズし、最大 1,024 MB 単位で増加 1,048, 576 MB です。

`--backup-retention`を日数で指定されるバックアップの保有期間を指定する必要があるときにパラメーターを使用します。 値が指定されていない場合、既定で 7 日間です。

使用する、`--version`パラメーターを使用したい PostgreSQL のメジャー バージョンを指定します。

これで、Azure Database for PostgreSQL サーバーの Azure CLI を使用して作成する手順を説明しました。 次の単位では、Azure Database for PostgreSQL サーバーの Azure CLI を使用してを作成します。