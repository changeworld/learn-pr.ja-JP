<span data-ttu-id="9b41c-101">このユニットでは、.NET Core コンソール アプリケーションと Azure ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-101">In this unit, you will create a .NET Core console application and an Azure storage account.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="9b41c-102">.NET Core コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="9b41c-102">Create a .NET Core console application</span></span>

<span data-ttu-id="9b41c-103">Visual Studio 2017 を使用してアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-103">We will be using Visual Studio 2017 to create our app.</span></span>

1. <span data-ttu-id="9b41c-104">Visual Studio 2017 で、**[ファイル]** > **[新規]** > **[プロジェクト]** メニュー オプションの順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-104">In Visual Studio 2017, select the **File** > **New** > **Project** menu option.</span></span>

1. <span data-ttu-id="9b41c-105">**[Visual C# - .NET Core]** カテゴリで **[Console App (.NET Core)]\(コンソール アプリ (.NET Core)\)** を選択し、アプリの名前を入力して **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-105">Within the **Visual C# - .NET Core** category, select **Console App (.NET Core)**, enter a name for your app and select **OK**.</span></span>
  <span data-ttu-id="9b41c-106">![新しいアプリ](..\media-draft\3-new-console-app.png)</span><span class="sxs-lookup"><span data-stu-id="9b41c-106">![New App](..\media-draft\3-new-console-app.png)</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9b41c-107">Azure ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="9b41c-107">Create an Azure storage account</span></span>

<span data-ttu-id="9b41c-108">Azure ストレージ アカウントに接続するには、まず接続するストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b41c-108">In order to connect to an Azure storage account, we must first create a storage account to connect to.</span></span>

1. <span data-ttu-id="9b41c-109">Azure portal の左上にある [リソースの作成] を選択します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-109">In the top left of the Azure Portal, select 'Create a resource'.</span></span>

1. <span data-ttu-id="9b41c-110">表示される選択ウィンドウで [ストレージ] を選択します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-110">In the selection pane that appears, select 'Storage'.</span></span>

1. <span data-ttu-id="9b41c-111">そのウィンドウの右側にある [ストレージ アカウント - Blob、File、Table、Queue] を選択します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-111">On the right side of that pane, select 'Storage account - blob, file, table, queue'.</span></span>
  <span data-ttu-id="9b41c-112">![ポータルの選択](..\media-draft\3-portal-storage-select.png)</span><span class="sxs-lookup"><span data-stu-id="9b41c-112">![portal select](..\media-draft\3-portal-storage-select.png)</span></span>

1. <span data-ttu-id="9b41c-113">ストレージ アカウントの詳細を入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b41c-113">The details of the storage account need to be entered.</span></span> <span data-ttu-id="9b41c-114">ストレージ アカウントの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-114">Enter a name for the storage account.</span></span>

1. <span data-ttu-id="9b41c-115">リソース グループは、リソースの論理コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="9b41c-115">A resource group is a logical container for resources.</span></span> <span data-ttu-id="9b41c-116">リソース グループの選択については、**[新規作成]** を選択し、リソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9b41c-116">For the resource group selection, select **Create New** and enter a name for your resource group.</span></span>

1. <span data-ttu-id="9b41c-117">その他のオプションはすべて既定値のままにして、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9b41c-117">Leave all other options at their default values and click **Create**.</span></span>
  <span data-ttu-id="9b41c-118">![ポータルの詳細](..\media-draft\3-portal-storage-details.png)。</span><span class="sxs-lookup"><span data-stu-id="9b41c-118">![Portal details](..\media-draft\3-portal-storage-details.png).</span></span>

<span data-ttu-id="9b41c-119">これでリソース グループがプロビジョニングされます。</span><span class="sxs-lookup"><span data-stu-id="9b41c-119">Your resource group is now being provisioned.</span></span> <span data-ttu-id="9b41c-120">その後に、ストレージ アカウントがリソース グループ内に作成されます。</span><span class="sxs-lookup"><span data-stu-id="9b41c-120">After that, your storage account is created within the resource group.</span></span>
<span data-ttu-id="9b41c-121">また、アプリケーションを作成したので、それを構成してストレージ アカウントに接続する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="9b41c-121">Additionally, you have created your application and are ready to configure and connect it to your storage account.</span></span>
