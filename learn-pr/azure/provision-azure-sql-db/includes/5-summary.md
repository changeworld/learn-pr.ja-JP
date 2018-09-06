<span data-ttu-id="321a7-101">Azure SQL Database が稼働しているので、お気に入りの SQL Server 管理ツールに接続して実際のデータを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-101">Now that your Azure SQL Database is up and running, you can connect it to your favorite SQL Server management tool to populate it with real data.</span></span>

<span data-ttu-id="321a7-102">最初に、データベースをオンプレミスとクラウドのどちらで実行するかを検討しました。</span><span class="sxs-lookup"><span data-stu-id="321a7-102">You initially considered whether to run your database on-premise or in the cloud.</span></span> <span data-ttu-id="321a7-103">Azure SQL Database を使用していくつかの基本的なオプションを構成し、アプリに接続できる完全な機能を備えた SQL データベースがあります。</span><span class="sxs-lookup"><span data-stu-id="321a7-103">With Azure SQL Database, you configure a few basic options and you have a fully functional SQL database that you can connect to your apps.</span></span>

<span data-ttu-id="321a7-104">保守が必要なインフラストラクチャやソフトウェア修正プログラムはありません。</span><span class="sxs-lookup"><span data-stu-id="321a7-104">There's no infrastructure or software patches to maintain.</span></span> <span data-ttu-id="321a7-105">輸送物流アプリのプロトタイプを稼働させることに労力を割き、データベース管理にかかる作業を減らすことができるようになりました。</span><span class="sxs-lookup"><span data-stu-id="321a7-105">You're now free to focus more on getting your transportation logistics app prototype up and running and less on database administration.</span></span> <span data-ttu-id="321a7-106">また、このプロトタイプは無駄になるデモではありません。</span><span class="sxs-lookup"><span data-stu-id="321a7-106">Your prototype won't be a throw-away demo, either.</span></span> <span data-ttu-id="321a7-107">Azure SQL Database には、運用レベルのセキュリティとパフォーマンスの機能があります。</span><span class="sxs-lookup"><span data-stu-id="321a7-107">Azure SQL Database provides production-level security and performance features.</span></span>

<span data-ttu-id="321a7-108">各 Azure SQL 論理サーバーには、1 つ以上のデータベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="321a7-108">Remember that each Azure SQL logical server contains one or more databases.</span></span> <span data-ttu-id="321a7-109">Azure SQL Database には DTU と仮想コアという 2 つの価格モデルがあり、すべてのデータベースでコストとパフォーマンスのバランスをとることができます。</span><span class="sxs-lookup"><span data-stu-id="321a7-109">Azure SQL Database provides two pricing models, DTU and vCore, to help you balance cost versus performance across all your databases.</span></span>

<span data-ttu-id="321a7-110">初めて使用する場合や、シンプルで事前に構成された購入オプションが必要な場合は、DTU を選択します。</span><span class="sxs-lookup"><span data-stu-id="321a7-110">Choose DTU if you're just getting started or want a simple, pre-configured buying option.</span></span> <span data-ttu-id="321a7-111">作成して支払うコンピューティング リソースとストレージ リソースをきめ細かく制御するには、仮想コアを選択します。</span><span class="sxs-lookup"><span data-stu-id="321a7-111">Choose vCore when you want greater control over what compute and storage resources you create and pay for.</span></span>

<span data-ttu-id="321a7-112">Cloud Shell を使用すると、データベースの操作を簡単に開始できます。</span><span class="sxs-lookup"><span data-stu-id="321a7-112">Cloud Shell makes it easy to start working with your databases.</span></span> <span data-ttu-id="321a7-113">Cloud Shell から Azure CLI にアクセスできます。そのため、Azure リソースに関する情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="321a7-113">From Cloud Shell, you have access to the Azure CLI, which enables you to get information about your Azure resources.</span></span> <span data-ttu-id="321a7-114">また、Cloud Shell には `sqlcmd` などの他の一般的なユーティリティも多く用意されているので、新しいデータベースをすぐに使用できます。</span><span class="sxs-lookup"><span data-stu-id="321a7-114">Cloud Shell also provides many other common utilities, such as `sqlcmd`, to help you start working right away with your new database.</span></span>

## <a name="cleanup"></a><span data-ttu-id="321a7-115">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="321a7-115">Cleanup</span></span>

<span data-ttu-id="321a7-116">Azure SQL Database のインストールを使用して、他にも自由に実験してみてください。</span><span class="sxs-lookup"><span data-stu-id="321a7-116">Feel free to experiment more with your Azure SQL Database installation.</span></span> <span data-ttu-id="321a7-117">終わった後にデータベースを削除する最も簡単な方法は、親リソース グループを削除することです。</span><span class="sxs-lookup"><span data-stu-id="321a7-117">When you're done, the easiest way to delete your database is to delete its parent resource group.</span></span>

1. <span data-ttu-id="321a7-118">ポータルで **[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="321a7-118">From the portal, click **Resource groups**.</span></span>
1. <span data-ttu-id="321a7-119">**logistics-db-rg** を選択します。</span><span class="sxs-lookup"><span data-stu-id="321a7-119">Select **logistics-db-rg**.</span></span>
1. <span data-ttu-id="321a7-120">**[リソース グループの削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="321a7-120">Click **Delete resource group**.</span></span>

    ![リソース グループを削除する](../media-draft/delete-rg.png)
1. <span data-ttu-id="321a7-122">プロンプトで「logistics-db-rg」と入力し、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="321a7-122">At the prompt, type "logistics-db-rg" and click **Delete**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="321a7-123">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="321a7-123">Additional resources</span></span>

<span data-ttu-id="321a7-124">ドキュメントには、チュートリアルやサンプルなど、多くの情報が提供されています。</span><span class="sxs-lookup"><span data-stu-id="321a7-124">The documentation provides lots more information, including tutorials and samples.</span></span> <span data-ttu-id="321a7-125">ここで取り上げた内容に関するリンクを次にいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="321a7-125">Here are a few links to what we covered here:</span></span>

* [<span data-ttu-id="321a7-126">Azure SQL Database のドキュメント</span><span class="sxs-lookup"><span data-stu-id="321a7-126">Azure SQL Database Documentation</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)
* [<span data-ttu-id="321a7-127">Azure SQL Database の購入モデルとリソース</span><span class="sxs-lookup"><span data-stu-id="321a7-127">Azure SQL Database purchasing models and resources</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-service-tiers)
* [<span data-ttu-id="321a7-128">Azure SQL Database 論理サーバーとその管理</span><span class="sxs-lookup"><span data-stu-id="321a7-128">Azure SQL Database logical servers and their management</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-logical-servers)
* [<span data-ttu-id="321a7-129">Azure SQL Database と SQL Data Warehouse のファイアウォール規則</span><span class="sxs-lookup"><span data-stu-id="321a7-129">Azure SQL Database and SQL Data Warehouse firewall rules</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure)

<span data-ttu-id="321a7-130">Cloud Shell の詳細については、[Azure Cloud Shell の概要](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="321a7-130">To learn more about Cloud Shell, see [Overview of Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

<span data-ttu-id="321a7-131">`sqlcmd` ユーティリティの詳細について関心をお持ちの場合は、[sqlcmd ユーティリティ](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-2017)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="321a7-131">If you're interested in learning more about the `sqlcmd` utility, see [sqlcmd Utility](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-2017).</span></span>