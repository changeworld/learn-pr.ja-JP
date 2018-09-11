<span data-ttu-id="50506-101">この演習では、Data Migration Assistant を使用して既存のデータベースを評価し、現在、Azure SQL Database でサポートされていないローカルの SQL Server インスタンスで使用される機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="50506-101">In this exercise, you'll assess an existing database using Data Migration Assistant and review any features used in the local SQL Server instance that aren't currently supported in by Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="50506-102">セットアップ</span><span class="sxs-lookup"><span data-stu-id="50506-102">Setup</span></span>

1. <span data-ttu-id="50506-103">まだインストールしていない場合は、[**Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="50506-103">[Install the **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) if you haven't done so already.</span></span>

1. <span data-ttu-id="50506-104">SQL Server インスタンスを実行している必要であり、接続の詳細を利用できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="50506-104">You'll need a SQL Server instance running, ensure you have connection details available.</span></span>
1. <span data-ttu-id="50506-105">[\*\*\*\* LOD VM に置き換えられる可能性が高い \*\*\*\*\*] <!-- TODO: --></span><span class="sxs-lookup"><span data-stu-id="50506-105">[\*\*\*\* likely replace with an LOD VM \*\*\*\*\*] <!-- TODO: --></span></span>

1. <span data-ttu-id="50506-106">インターネット ブラウザーを起動して、 https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017 に移動します。</span><span class="sxs-lookup"><span data-stu-id="50506-106">Start an Internet browser and navigate to https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="50506-107">「**OLTP のダウンロード**」で、**AdventureWorks2008R2.bak** をクリックして、ご利用のローカル コンピューターに保存します。</span><span class="sxs-lookup"><span data-stu-id="50506-107">In **OLTP downloads**, click **AdventureWorks2008R2.bak** and save it to your local machine.</span></span>

1. <span data-ttu-id="50506-108">Management Studio で、*AdventureWorks 2008R2* をご使用の既定のインスタンスに復元します。</span><span class="sxs-lookup"><span data-stu-id="50506-108">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-assessment"></a><span data-ttu-id="50506-109">評価の作成</span><span class="sxs-lookup"><span data-stu-id="50506-109">Create an assessment</span></span>

1. <span data-ttu-id="50506-110">**Microsoft Data Migration Assistant** を起動します。</span><span class="sxs-lookup"><span data-stu-id="50506-110">Start the **Microsoft Data Migration Assistant**.</span></span>

1. <span data-ttu-id="50506-111">アプリの左側にあるナビゲーションで、__+__ をクリックして新しい DMA プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="50506-111">In the app's left-hand navigation, click __+__ to create a new DMA project.</span></span>

1. <span data-ttu-id="50506-112">次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="50506-112">Specify the following options:</span></span>
    - <span data-ttu-id="50506-113">**プロジェクト タイプ** - *[評価]* を選択します</span><span class="sxs-lookup"><span data-stu-id="50506-113">**Project type** - Select *Assessment*</span></span>
    - <span data-ttu-id="50506-114">**プロジェクト名** - プロジェクトの名前を入力します。たとえば、"Bicycle DB Assessment" です</span><span class="sxs-lookup"><span data-stu-id="50506-114">**Project name** - Enter a name for your project - for example "Bicycle DB Assessment"</span></span>
    - <span data-ttu-id="50506-115">**ソース サーバーの種類** - *[SQL Server]* を選択します</span><span class="sxs-lookup"><span data-stu-id="50506-115">**Source server type** - Select *SQL Server*</span></span>
    - <span data-ttu-id="50506-116">**対象サーバーの種類** - *[Azure SQL Database]* を選択します</span><span class="sxs-lookup"><span data-stu-id="50506-116">**Target server type** - Select *Azure SQL Database*</span></span>

1. <span data-ttu-id="50506-117">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-117">Click **Create**.</span></span>
    <span data-ttu-id="50506-118">![Data Migration Assistant でご利用の AdventureWorks SQL Server データに対して記述された構成を示すスクリーンショット。](../media-draft/3-create-assessment.png)</span><span class="sxs-lookup"><span data-stu-id="50506-118">![Screenshot showing the described configuration in the Data Migration Assistant for your AdventureWorks SQL Server data.](../media-draft/3-create-assessment.png)</span></span>

1. <span data-ttu-id="50506-119">評価レポートの種類を選択します。次の両方をチェックします。</span><span class="sxs-lookup"><span data-stu-id="50506-119">Select the assessment report type - check both:</span></span>
    - <span data-ttu-id="50506-120">データベース互換性をチェックする</span><span class="sxs-lookup"><span data-stu-id="50506-120">Check database compatibility</span></span>
    - <span data-ttu-id="50506-121">機能パリティをチェックする</span><span class="sxs-lookup"><span data-stu-id="50506-121">Check feature parity</span></span>

1. <span data-ttu-id="50506-122">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-122">Click **Next**.</span></span>

## <a name="add-databases-to-assess"></a><span data-ttu-id="50506-123">評価するデータベースの追加</span><span class="sxs-lookup"><span data-stu-id="50506-123">Add databases to assess</span></span>

1. <span data-ttu-id="50506-124">**[ソースの追加]** をクリックして接続メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="50506-124">Click **Add Sources** to open the connection menu.</span></span>
2. <span data-ttu-id="50506-125">次の情報を入力します</span><span class="sxs-lookup"><span data-stu-id="50506-125">Enter the following information</span></span>
    - <span data-ttu-id="50506-126">既存の SQL Server インスタンスの名前を入力します</span><span class="sxs-lookup"><span data-stu-id="50506-126">Enter your existing SQL server instance name</span></span>
    - <span data-ttu-id="50506-127">**[認証]** の種類を選択します</span><span class="sxs-lookup"><span data-stu-id="50506-127">Select the **Authentication** type</span></span>
    - <span data-ttu-id="50506-128">サーバーの接続プロパティを指定します</span><span class="sxs-lookup"><span data-stu-id="50506-128">Specify the connection properties for your server</span></span>
3. <span data-ttu-id="50506-129">**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-129">CLick **Connect**.</span></span>
4. <span data-ttu-id="50506-130">**[ソースの追加]** で、評価するデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="50506-130">In **Add sources**, select the databases to assess.</span></span> <span data-ttu-id="50506-131">この演習では、**AdventureWorks2008R2** を選択します。</span><span class="sxs-lookup"><span data-stu-id="50506-131">For this exercise, select **AdventureWorks2008R2**.</span></span>
5. <span data-ttu-id="50506-132">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-132">Click **Add**.</span></span>
    > [!NOTE]
    > <span data-ttu-id="50506-133">複数の SQL Server インスタンスからデータベースを追加するには、**[ソースの追加]** ボタンを使用します。</span><span class="sxs-lookup"><span data-stu-id="50506-133">To add databases from multiple SQL Server instances, use the **Add Sources** button.</span></span> <span data-ttu-id="50506-134">複数のデータベースを削除するには、Shift + Ctrl キーを押しながら削除するデータベースを選択して、**[ソースの削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-134">To remove multiple databases, hold the SHIFT+CTRL keys and select the databases to remove then click **Remove Sources**.</span></span>

6. <span data-ttu-id="50506-135">**[評価の開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50506-135">Click **Start Assessment**.</span></span>

## <a name="view-results"></a><span data-ttu-id="50506-136">結果の表示</span><span class="sxs-lookup"><span data-stu-id="50506-136">View results</span></span>

<span data-ttu-id="50506-137">複数のデータベースがある場合、各データベースの結果は利用できるようになるとすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="50506-137">If there are multiple databases, the results for each database will appear as soon as it is available.</span></span> <span data-ttu-id="50506-138">すべてのデータベースの評価が完了するまで待機する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="50506-138">You don't need to wait for all database assessments to complete.</span></span>

1. <span data-ttu-id="50506-139">**AdventureWorks** の評価が完了したら、**[互換性の問題]** と **[機能に関する推奨事項]** をクリックして結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="50506-139">Once the assessment for **AdventureWorks** is complete, click **Compatibility issues** and **Feature recommendations** to view the results.</span></span>
    - <span data-ttu-id="50506-140">SQL Server の機能パリティ カテゴリには、完全にサポートされない可能性がある機能とこれらの問題を解決するための手順が一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="50506-140">The SQL Server feature parity category lists features that might not be fully supported and steps to remedy these issues.</span></span> <span data-ttu-id="50506-141">機能パリティの問題では、移行は停止されません。</span><span class="sxs-lookup"><span data-stu-id="50506-141">Feature parity issues will not stop a migration.</span></span>
    - <span data-ttu-id="50506-142">互換性の問題カテゴリには、移行を妨げる機能とこれらの問題を解決するための手順が一覧されます。</span><span class="sxs-lookup"><span data-stu-id="50506-142">The Compatibility issues category lists features that would block a migration and steps to remedy these issues.</span></span>

## <a name="summary"></a><span data-ttu-id="50506-143">まとめ</span><span class="sxs-lookup"><span data-stu-id="50506-143">Summary</span></span>

<span data-ttu-id="50506-144">この演習では、ローカルにインストールされた SQL Server データベースを評価して、データベースを Azure SQL Database に移行したときに、利用できない機能があるかどうかを検証しました。</span><span class="sxs-lookup"><span data-stu-id="50506-144">In this exercise, you assessed a locally installed SQL Server database to verify if any features would be unavailable when you migrate the database to Azure SQL Database.</span></span>