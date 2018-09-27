<span data-ttu-id="f9a5f-101">ランナーのフィットネス デバイスからキャプチャされたルートを格納するための Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-101">You decide to create an Azure Database for PostgreSQL server to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="f9a5f-102">取得されたデータ ボリュームの履歴から、サーバー ストレージの要件を 20 GB に設定する必要があることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="f9a5f-103">この処理要件をサポートするには、仮想コアを 1 個備えた Compute Gen 5 のサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="f9a5f-104">また、データのバックアップ用に 15 日間の保有期間が必要であることもわかっています。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-104">You also know that you require a retention period of 15 days for data backups.</span></span>

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a><span data-ttu-id="f9a5f-105">Azure CLI を使用して Azure PostGreSQL データベースを作成する</span><span class="sxs-lookup"><span data-stu-id="f9a5f-105">Create an Azure PostGreSQL database with the Azure CLI</span></span>

<span data-ttu-id="f9a5f-106">ストレージ サイズ 20 GB、1 仮想コアの Compute Gen 5 サポート、データ バックアップの保有期間 15 日に、サーバーを設定することに留意してください。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-106">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

1. <span data-ttu-id="f9a5f-107">`az postgres server create` メソッドを使用して、新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-107">Use the `az postgres server create` method to create a new database.</span></span> <span data-ttu-id="f9a5f-108">指定するパラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-108">There are several parameters that you'll specify:</span></span>
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. <span data-ttu-id="f9a5f-109">下のソリューションを見なくてもコマンドを記述してパラメーターを完了できるかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-109">See if you can build the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="f9a5f-110">ここでは、いくつかのヒントを紹介します。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-110">Here are some tips.</span></span>
    - <span data-ttu-id="f9a5f-111">`<values>` は実際の値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-111">Replace the `<values>` with your own values.</span></span> 
    - <span data-ttu-id="f9a5f-112">繰り返しになりますが、サーバー名は、小文字アルファベットの 'a' から 'z'、数字の 0 から 9、ハイフンで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-112">Remember that the server name must be  made up of lowercase letters 'a'-'z', the numbers 0-9 and the hyphen.</span></span>
    - <span data-ttu-id="f9a5f-113"><rgn>[サンドボックス リソース グループ名]</rgn> をリソース グループとして使用します。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-113">Use <rgn>[sandbox resource group name]</rgn> as the resource group.</span></span>
    - <span data-ttu-id="f9a5f-114">次の一覧の場所を使用します。   [!include[](../../../includes/azure-sandbox-regions-note.md)]</span><span class="sxs-lookup"><span data-stu-id="f9a5f-114">Use a location from the following list:   [!include[](../../../includes/azure-sandbox-regions-note.md)]</span></span>
    
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

<span data-ttu-id="f9a5f-115">実行時にはシステムによる情報の処理にしばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-115">The system will take a few moments to process the information when executed.</span></span> <span data-ttu-id="f9a5f-116">コマンドが完了するまで待ってください。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-116">Go ahead and wait for the command to complete.</span></span>

<span data-ttu-id="f9a5f-117">完了すると、サーバーを記述する JavaScript Object Notation (JSON) 文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-117">Once it's done, a JavaScript Object Notation (JSON) string that describes the server is returned.</span></span> <span data-ttu-id="f9a5f-118">エラーが発生した場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-118">If there was a failure, an error message is displayed.</span></span> <span data-ttu-id="f9a5f-119">このエラー情報を使用してコマンド パラメーターを確認して修正した後でやり直してください。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-119">You can use this error information to review and fix your command parameters and try again.</span></span>

<span data-ttu-id="f9a5f-120">JSON オブジェクトは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-120">The JSON object will look something like:</span></span>

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

<span data-ttu-id="f9a5f-121">Azure CLI を使用して PostgreSQL サーバーを正常に作成しました。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-121">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="f9a5f-122">次のユニットでは、サーバーのセキュリティ設定を構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f9a5f-122">In the next unit, you'll see how to configure your server's security settings.</span></span>
