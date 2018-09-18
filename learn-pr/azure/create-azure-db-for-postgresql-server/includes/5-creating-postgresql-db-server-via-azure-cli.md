あなたはオンプレミスの PostgreSQL データベースを使っているものとしましょう。 あなたの会社は、サーバーを Azure に移動することによって、デバイスのサポート、可用性、データの追跡、および処理の機能を拡張しようとしています。 あなたは、Azure Database for PostgreSQL の作成の自動化にかかる労力を調べています。

Azure portal を使用して作成する Azure Database for PostgreSQL サーバーが 1 つであれば難しくありません。 複数のデータベースの作成と継続的なメンテナンスをポータルのみを使用して行おうとすると、面倒になる可能性があります。 管理タスクを自動化したいときは、Azure CLI を使用してスクリプトを作成します。

Azure CLI を使用すると、Microsoft Azure 内のほぼすべてのリソースの作成を自動化できます。 このユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーの管理を自動化する方法を学習します。

## <a name="what-is-azure-cli"></a>Azure CLI とは

[Azure CLI](https://docs.microsoft.com/cli/azure/) は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン環境です。 ブラウザーから Azure Cloud Shell で Azure CLI を使用することも、Mac OS X、Linux、または Windows に Azure CLI をローカルにインストールすることもできます。 Azure CLI は、bash または Powershell を使用してローカル コマンド ラインから実行します。 ただし、Azure CLI をローカルに実行するには追加のセットアップが必要です。 ここでは、Azure Cloud Shell を使用して Azure CLI コマンドを実行します。

## <a name="what-is-azure-cloud-shell"></a>Azure Cloud Shell とは

Azure Cloud Shell はクラウドでホストされているブラウザー ベースのシェル エクスペリエンスであり、認証されたセッションを使用して Azure に接続することができます。 Azure CLI コマンドを実行して、Azure Database for PostgreSQL の管理を自動化できます。 Cloud Shell には一般的な Azure CLI ツールが事前にインストールされており、アカウントで使用できるように構成されています。

> [!NOTE]
> Cloud Shell には、Cloud Shell で作業中に作成するすべてのファイルを保持するための Azure ストレージ リソースが必要です。 Cloud Shell の初回起動時に、リソース グループ、ストレージ アカウント、Azure Files 共有を作成するように求められます。 これは 1 回限りの作業であり、それ以降のすべての Cloud Shell セッションに対して自動的に接続されます。

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a>Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成する

Azure Cloud Shell で Azure CLI を使用して、Azure Database for PostgreSQL サーバーを作成します。 その手順を見ていきましょう。

まず、Azure portal にサインインします。

Azure portal から Cloud Shell を開きます。 ブラウザーを開いて [Azure portal](https://portal.azure.com?azure-portal=true) に移動し、[Open Cloud Shell]\(Cloud Shell を開く\) ボタンをクリックします。

![Cloud Shell ボタン](../media-draft/cloud-shell-button.png)

Cloud Shell では、`bash` または `PowerShell` のいずれかでコマンドを実行できます。 すべての例で、`bash` コマンド ライン オプションを使用します。

複数のサブスクリプションがある場合は、次のコマンドを使用して適切なサブスクリプションをアクティブにします。0 はお使いのサブスクリプション ID に置き換えます。

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

自分のすべてのサブスクリプションを一覧表示するには、次のコマンドを使用します。

   ```bash
   az account list --output table
   ```

次に、サーバーを管理するためのリソース グループと、リソース グループを配置する場所を作成します。 リソース グループはサーバーに関連するすべてのリソースを管理するために使用することを思い出してください。 場所のオプションでは、サーバーが物理的に作成される場所を指定できます。 次のコマンドの `<resource_group_name>` と `<location>` をそれぞれを適切な値に置き換えて実行します。

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

最後に、Azure Database for PostgreSQL サーバーを作成します。

   使用可能なすべてのパラメーターが示される Azure CLI のサーバー作成コマンドの使用方法のヘルプは、次の例のようになります。

   ```bash
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

   次のコマンド ラインでは、Azure Database for PostgreSQL サーバーを作成するために必要なパラメーターのセットを示します。 一部の省略可能なパラメーターは表示されていないことがわかります。

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>パラメーターの説明

`--resource-group <resource_group_name>` パラメーターは、サーバーを作成するリソース グループを指定します。

指定するサーバーの `admin-user` と `admin-password` は、サーバーとそのデータベースにサインインするために必要です。 後で新しいサーバーとやりとりするときのために、この情報を憶えるか記録しておきます。

`--sku-name` パラメーターは、価格レベルの部分を指定するために使用します (この場合はコンピューティング リソース)。 値は `{pricing tier}_{compute generation}_{vCores}` という規則に従います。

次に例を示します。

- `--sku-name B_Gen4_4` は、"Basic、Gen 4、および 4 個の仮想コア" にマップされます。
- `--sku-name GP_Gen5_32` は、"汎用、Gen 5、および 32 個の仮想コア" にマップされます。
- `--sku-name MO_Gen5_2` は、"メモリ最適化、Gen 5、および 2 個の仮想コア" にマップされます。

ポータルを使用してサーバーを作成するユニットで 3 つの価格レベルを説明したことを思い出してください。

"Basic、Gen 5、1 仮想コア" のコンピューティング リソースを使用する場合は、`--sku-name B_Gen5_1` というパラメーターを指定します。

価格レベルの部分を指定するには `--storage-size` パラメーターも使用します。 値を指定しない場合の既定値は 5,120 MB です。 有効なストレージ サイズの範囲は 5,120 MB - 1,048,576 MB で、1,024 MB 刻みです。

`--backup-retention` パラメーターは、バックアップの保有期間の日数を指定する必要があるときに使用します。 値を指定しない場合の既定値は 7 日です。

`--version` パラメーターは、使用する PostgreSQL のメジャー バージョンを指定するために使用します。

ここまでは、Azure CLI を使用して Azure Database for PostgreSQL を作成する手順を説明しました。 次のユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成します。