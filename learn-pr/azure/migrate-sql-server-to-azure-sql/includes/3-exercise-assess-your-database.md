<span data-ttu-id="97453-101">このユニットでは、Data Migration Assistant を使用して既存のデータベースを評価し、現在、Azure SQL Database でサポートされていないローカルの SQL Server インスタンスで使用される機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="97453-101">In this unit, you'll assess an existing database using the Data Migration Assistant and review any features used in the local SQL Server instance that aren't currently supported by Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="97453-102">セットアップ</span><span class="sxs-lookup"><span data-stu-id="97453-102">Setup</span></span>

1. <span data-ttu-id="97453-103">まだインストールしていない場合は、[**Data Migration Assistant** をインストール](https://www.microsoft.com/download/details.aspx?id=53595)します。</span><span class="sxs-lookup"><span data-stu-id="97453-103">[Install the **Data Migration Assistant**](https://www.microsoft.com/download/details.aspx?id=53595) if you haven't done so already.</span></span>

1. <span data-ttu-id="97453-104">SQL Server インスタンスを実行している必要であり、接続の詳細を利用できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="97453-104">You'll need a SQL Server instance running, ensure you have connection details available.</span></span>

<!-- 1. [**** likely replace with an LOD VM *****] TODO: -->

1. <span data-ttu-id="97453-105">インターネット ブラウザーを起動して、 https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017 に移動します。</span><span class="sxs-lookup"><span data-stu-id="97453-105">Start an Internet browser and navigate to https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="97453-106">「**OLTP downloads**」(OLTP のダウンロード) で **AdventureWorks2008R2.bak** をクリックして、お使いローカル コンピューターに保存します。</span><span class="sxs-lookup"><span data-stu-id="97453-106">In **OLTP downloads**, click **AdventureWorks2008R2.bak** and save it to your local machine.</span></span>

1. <span data-ttu-id="97453-107">Management Studio で、*AdventureWorks 2008R2* を既定のインスタンスに復元します。</span><span class="sxs-lookup"><span data-stu-id="97453-107">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-assessment"></a><span data-ttu-id="97453-108">評価を作成する</span><span class="sxs-lookup"><span data-stu-id="97453-108">Create an assessment</span></span>

1. <span data-ttu-id="97453-109">**Microsoft Data Migration Assistant** を起動します。</span><span class="sxs-lookup"><span data-stu-id="97453-109">Start the **Microsoft Data Migration Assistant**.</span></span>

1. <span data-ttu-id="97453-110">アプリの左側にあるナビゲーションで、__[+]__ をクリックして新しい DMA プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="97453-110">In the app's left-hand navigation, click __+__ to create a new Data Migration Assistant project.</span></span>

1. <span data-ttu-id="97453-111">次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="97453-111">Specify the following options:</span></span>

    - <span data-ttu-id="97453-112">**[プロジェクトの種類]** - *[評価]* を選択します</span><span class="sxs-lookup"><span data-stu-id="97453-112">**Project type** - Select *Assessment*</span></span>
    - <span data-ttu-id="97453-113">**[プロジェクト名]** - プロジェクトの名前を入力します (例: "Bicycle DB Assessment")</span><span class="sxs-lookup"><span data-stu-id="97453-113">**Project name** - Enter a name for your project - for example, "Bicycle DB Assessment"</span></span>
    - <span data-ttu-id="97453-114">**[Source server type]\(ソース サーバーの種類\)** - *[SQL Server]* を選択します</span><span class="sxs-lookup"><span data-stu-id="97453-114">**Source server type** - Select *SQL Server*</span></span>
    - <span data-ttu-id="97453-115">**[Target server type]\(対象サーバーの種類\)** - *[Azure SQL Database]* を選択します</span><span class="sxs-lookup"><span data-stu-id="97453-115">**Target server type** - Select *Azure SQL Database*</span></span>

1. <span data-ttu-id="97453-116">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-116">Click **Create**.</span></span>
    <span data-ttu-id="97453-117">![Data Migration Assistant でご利用の AdventureWorks SQL Server データに対して記述された構成を示すスクリーンショット。](../media-draft/3-create-assessment.png)</span><span class="sxs-lookup"><span data-stu-id="97453-117">![Screenshot showing the described configuration in the Data Migration Assistant for your AdventureWorks SQL Server data.](../media-draft/3-create-assessment.png)</span></span>

1. <span data-ttu-id="97453-118">評価レポートの種類を選択します。次の両方をオンにします。</span><span class="sxs-lookup"><span data-stu-id="97453-118">Select the assessment report type - check both:</span></span>
    - <span data-ttu-id="97453-119">データベースの互換性を確認します</span><span class="sxs-lookup"><span data-stu-id="97453-119">Check database compatibility</span></span>
    - <span data-ttu-id="97453-120">機能パリティを確認します</span><span class="sxs-lookup"><span data-stu-id="97453-120">Check feature parity</span></span>

1. <span data-ttu-id="97453-121">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-121">Click **Next**.</span></span>

## <a name="add-databases-to-assess"></a><span data-ttu-id="97453-122">評価するデータベースを追加する</span><span class="sxs-lookup"><span data-stu-id="97453-122">Add databases to assess</span></span>

1. <span data-ttu-id="97453-123">**[ソースの追加]** をクリックして接続メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="97453-123">Click **Add Sources** to open the connection menu.</span></span>

1. <span data-ttu-id="97453-124">以下の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="97453-124">Do the following:</span></span>

    - <span data-ttu-id="97453-125">既存の SQL Server インスタンスの名前を入力します</span><span class="sxs-lookup"><span data-stu-id="97453-125">Enter your existing SQL server instance name</span></span>
    - <span data-ttu-id="97453-126">**[認証]** の種類を選択します</span><span class="sxs-lookup"><span data-stu-id="97453-126">Select the **Authentication** type</span></span>
    - <span data-ttu-id="97453-127">サーバーの接続プロパティを指定します</span><span class="sxs-lookup"><span data-stu-id="97453-127">Specify the connection properties for your server</span></span>

1. <span data-ttu-id="97453-128">**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-128">CLick **Connect**.</span></span>

1. <span data-ttu-id="97453-129">**[ソースの追加]** で、評価するデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="97453-129">In **Add sources**, select the databases to assess.</span></span> <span data-ttu-id="97453-130">この演習では、**AdventureWorks2008R2** を選択します。</span><span class="sxs-lookup"><span data-stu-id="97453-130">For this exercise, select **AdventureWorks2008R2**.</span></span>

1. <span data-ttu-id="97453-131">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-131">Click **Add**.</span></span>
    > [!NOTE]
    > <span data-ttu-id="97453-132">複数の SQL Server インスタンスからデータベースを追加するには、**[ソースの追加]** ボタンを使用します。</span><span class="sxs-lookup"><span data-stu-id="97453-132">To add databases from multiple SQL Server instances, use the **Add Sources** button.</span></span> <span data-ttu-id="97453-133">複数のデータベースを削除するには、Shift + Ctrl キーを押しながら削除するデータベースを選択して、**[ソースの削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-133">To remove multiple databases, hold the SHIFT+CTRL keys to select the databases you want to remove, then click **Remove Sources**.</span></span>

1. <span data-ttu-id="97453-134">**[評価の開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97453-134">Click **Start Assessment**.</span></span>

## <a name="view-results"></a><span data-ttu-id="97453-135">結果を表示する</span><span class="sxs-lookup"><span data-stu-id="97453-135">View results</span></span>

<span data-ttu-id="97453-136">複数のデータベースがある場合、各データベースの結果は利用できるようになるとすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="97453-136">If there are multiple databases, the results for each database appears as soon as it is available.</span></span> <span data-ttu-id="97453-137">すべてのデータベースの評価が完了するまで待機する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="97453-137">You don't need to wait for all database assessments to complete.</span></span>

1. <span data-ttu-id="97453-138">**AdventureWorks** の評価が完了したら、**[互換性の問題]** と **[機能に関する推奨事項]** をクリックして結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="97453-138">Once the assessment for **AdventureWorks** is complete, click **Compatibility issues** and **Feature recommendations** to view the results.</span></span>

    - <span data-ttu-id="97453-139">SQL Server の機能パリティ カテゴリには、完全にサポートされない可能性がある機能とこれらの問題を解決するための手順が一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="97453-139">The SQL Server feature parity category lists features that might not be fully supported and steps to remedy these issues.</span></span> <span data-ttu-id="97453-140">機能パリティの問題では、移行は停止されません。</span><span class="sxs-lookup"><span data-stu-id="97453-140">Feature parity issues will not stop a migration.</span></span>
    - <span data-ttu-id="97453-141">互換性の問題カテゴリには、移行を妨げる機能とこれらの問題を解決するための手順が一覧されます。</span><span class="sxs-lookup"><span data-stu-id="97453-141">The Compatibility issues category lists features that would block a migration and steps to remedy these issues.</span></span>

## <a name="summary"></a><span data-ttu-id="97453-142">まとめ</span><span class="sxs-lookup"><span data-stu-id="97453-142">Summary</span></span>

<span data-ttu-id="97453-143">このユニットでは、ローカルにインストールされた SQL Server データベースを評価して、データベースを Azure SQL Database に移行したときに、利用できない機能があるかどうかを検証しました。</span><span class="sxs-lookup"><span data-stu-id="97453-143">In this unit, you assessed a locally installed SQL Server database to verify if any features would be unavailable when you migrate the database to Azure SQL Database.</span></span>