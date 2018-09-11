<span data-ttu-id="6fcd3-101">この演習では、Azure portal を使用して、南カリフォルニアの架空のサーフィン レポート Web App に適したストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-101">In this exercise, you will use the Azure portal to create a storage account that is appropriate for a fictitious southern California surf report Web App.</span></span>

## <a name="design-goals"></a><span data-ttu-id="6fcd3-102">デザインの目標</span><span class="sxs-lookup"><span data-stu-id="6fcd3-102">Design goals</span></span>

<span data-ttu-id="6fcd3-103">このサーフィン レポート サイトでは、ユーザーが現地のビーチの状態の写真とビデオをアップロードすることができる。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-103">The surf-report site lets users upload photos and videos of their local beach conditions.</span></span> <span data-ttu-id="6fcd3-104">表示するユーザーは、そのコンテンツを使って、サーフィンに最適な状態のビーチを選ぶことができる。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-104">Viewers will use the content to help them choose the beach with the best surfing conditions.</span></span> <span data-ttu-id="6fcd3-105">デザインと機能の目標は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-105">Your list of design and feature goals is:</span></span>

- <span data-ttu-id="6fcd3-106">ビデオ コンテンツはすばやく読み込まれる必要がある</span><span class="sxs-lookup"><span data-stu-id="6fcd3-106">Video content must load quickly</span></span>
- <span data-ttu-id="6fcd3-107">このサイトではアップロード ボリュームの予期しない上昇を処理する必要がある</span><span class="sxs-lookup"><span data-stu-id="6fcd3-107">The site must handle unexpected spikes in upload volume</span></span>
- <span data-ttu-id="6fcd3-108">サーフィンの状態が変更されると、古くなったコンテンツは削除されるため、サイトでは常に現在の状態が示される</span><span class="sxs-lookup"><span data-stu-id="6fcd3-108">Outdated content will be removed as surf conditions change so the site always shows current conditions</span></span>

<span data-ttu-id="6fcd3-109">処理用の Azure キューでアップロードされたコンテンツをバッファー処理し、ストレージ用の Azure BLOB に移動する実装に決めます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-109">You decide on an implementation that buffers uploaded content in an Azure Queue for processing and then moves it into an Azure Blob for storage.</span></span> <span data-ttu-id="6fcd3-110">コンテンツに対して待機時間の短いアクセスを提供しながら、キューと BLOB の両方を保持できるストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-110">You need a storage account that can hold both Queues and Blobs while delivering low-latency access to your content.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="6fcd3-111">演習の手順</span><span class="sxs-lookup"><span data-stu-id="6fcd3-111">Exercise steps</span></span>

### <a name="launch-the-blade"></a><span data-ttu-id="6fcd3-112">ブレードを起動する</span><span class="sxs-lookup"><span data-stu-id="6fcd3-112">Launch the blade</span></span>

1. <span data-ttu-id="6fcd3-113">ご利用の Web ブラウザーから [Azure portal](https://portal.azure.com?azure-portal=true) に移動し、ご自分のアカウントでサインインします。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-113">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="6fcd3-114">左サイドバーで **[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-114">In the left sidebar, select **Create a resource**.</span></span>

1. <span data-ttu-id="6fcd3-115">Azure Marketplace で **[ストレージ]** 見出しを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-115">Select the **Storage** heading in the Azure Marketplace.</span></span>

1. <span data-ttu-id="6fcd3-116">**[ストレージ アカウント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-116">Select **Storage account**.</span></span> <span data-ttu-id="6fcd3-117">ポータルに **[ストレージ アカウントを作成する]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-117">The portal will display the **Create storage account** blade.</span></span>

### <a name="configure-the-basic-options"></a><span data-ttu-id="6fcd3-118">基本的なオプションを構成する</span><span class="sxs-lookup"><span data-stu-id="6fcd3-118">Configure the basic options</span></span>

1. <span data-ttu-id="6fcd3-119">ブレードの上部で **[基本]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-119">Select the **Basics** tab at the top of the blade.</span></span>

1. <span data-ttu-id="6fcd3-120">**[サブスクリプション]**: ご使用のサブスクリプションのいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-120">**Subscription**: Select one of your subscriptions.</span></span>

1. <span data-ttu-id="6fcd3-121">**[リソース グループ]**: 「**SurfReportResourceGroup**」という名前の新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-121">**Resource group**: Create a new resource group named **SurfReportResourceGroup**.</span></span>

1. <span data-ttu-id="6fcd3-122">**[ストレージ アカウント名]**: グローバルに一意の値 (`surfreport` + イニシャル + 数字など) を入力します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-122">**Storage account name**: Enter a globally-unique value like `surfreport` + your initials + a number.</span></span>

 1. <span data-ttu-id="6fcd3-123">**[場所]**: **[米国西部]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-123">**Location**: Select **West US**.</span></span>

    <span data-ttu-id="6fcd3-124">根拠: このアプリケーションは、南カリフォルニアのユーザーを想定しています。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-124">Rationale: The application is intended for users in southern California.</span></span> <span data-ttu-id="6fcd3-125">ビデオを読み込むときの待機時間を最小限に抑えるには、BLOB をこれらのユーザーの近くでホストする必要があります。そのため、**[米国西部]** が最適な選択になります。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-125">To minimize latency when loading videos, the Blobs should be hosted close to these users; this makes **West US** a good choice.</span></span>

1. <span data-ttu-id="6fcd3-126">**[デプロイ モデル]**: **[Resource Manager]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-126">**Deployment model**: Select **Resource manager**.</span></span>
    
    <span data-ttu-id="6fcd3-127">根拠: リソース グループを使用して、このアプリケーション用の Web App、ストレージ アカウントなどを管理できるようにするため、**[Resource Manager]** が適しています。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-127">Rationale: **Resource manager** is appropriate because it will let you use a resource group to manage the Web App, storage account, etc. for the application.</span></span>

1. <span data-ttu-id="6fcd3-128">**[パフォーマンス]**: **[Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-128">**Performance**: Select **Standard**.</span></span>

    <span data-ttu-id="6fcd3-129">根拠: **[Premium]** オプションでは、ページ BLOB に対してストレージ アカウントを制限するため、このオプションを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-129">Rationale: You cannot use the **Premium** option because it would limit the storage account to page Blobs.</span></span> <span data-ttu-id="6fcd3-130">どちらも **[Standard]** オプションでのみ利用できる、ビデオ用のブロック BLOB とバッファリング用のキューが必要です。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-130">You need block Blobs for your videos and a Queue for buffering both of which are only available in the **Standard** option.</span></span>

1. <span data-ttu-id="6fcd3-131">**[アカウントの種類]**: **[StorageV2 (汎用 v2)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-131">**Account kind**: Select **StorageV2 (general purpose v2)**.</span></span>

    <span data-ttu-id="6fcd3-132">根拠: ここでは **[StorageV2 (汎用 v2)]** が最適な選択です。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-132">Rationale: **StorageV2 (general purpose v2)** is the right choice here.</span></span> <span data-ttu-id="6fcd3-133">BLOB とキューを組み合わせる必要があるため、**[BLOB ストレージ]** オプションは使用できません。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-133">You need a mix of Blobs and a Queue, so the **Blob storage** option will not work.</span></span> <span data-ttu-id="6fcd3-134">このアプリケーションでは、アクセスできる機能が制限され、予想されるワークロードのコストを削減できる可能性が低いため、**[Storage (汎用 v1)]** アカウントを選択してもメリットはありません。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-134">For this application, there would be no benefit to choosing a **Storage (general purpose v1)** account since that would limit the features you could access and would be unlikely to reduce the cost of your expected workload.</span></span>

1. <span data-ttu-id="6fcd3-135">**[レプリケーション]**: **[ローカル冗長ストレージ (LRS)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-135">**Replication**: Select **Locally-redundant storage (LRS)**.</span></span>

    <span data-ttu-id="6fcd3-136">根拠: イメージとビデオはすぐに古くなり、サイトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-136">Rationale: The images and videos quickly become out-of-date and are removed from the site.</span></span> <span data-ttu-id="6fcd3-137">つまり、グローバルな冗長性のために追加料金を払う価値がほとんどないということです。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-137">This means there is little value to paying extra for global redundancy.</span></span> <span data-ttu-id="6fcd3-138">致命的な障害でデータを失った場合は、ユーザーからの最新コンテンツでサイトを再開することができます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-138">If a catastrophic event results in data loss, you can restart the site with fresh content from your users.</span></span>

1. <span data-ttu-id="6fcd3-139">**[アクセス層 (既定)]**: **[ホット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-139">**Access tier (default)**: Select **Hot**.</span></span>
   
    <span data-ttu-id="6fcd3-140">根拠: ビデオをすばやく読み込む必要があるため、BLOB に高パフォーマンス オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-140">Rationale: You want the videos to load quickly so you will use the high-performance option for your Blobs.</span></span>
   
<span data-ttu-id="6fcd3-141">次のスクリーンショットは、**[基本]** タブの設定が完了したことを示しています。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-141">The following screenshot shows the completed settings for the **Basics** tab.</span></span>

![**[基本]** タブが選択されている [ストレージ アカウントを作成する] ブレードのスクリーンショット。](../media-drafts/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a><span data-ttu-id="6fcd3-143">詳細オプションを構成する</span><span class="sxs-lookup"><span data-stu-id="6fcd3-143">Configure the advanced options</span></span>

1. <span data-ttu-id="6fcd3-144">ブレードの上部で **[詳細]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-144">Select the **Advanced** tab at the top of the blade.</span></span>

1. <span data-ttu-id="6fcd3-145">**[安全な転送が必須]**: **[有効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-145">**Secure transfer required**: Select **Enabled**.</span></span>

    <span data-ttu-id="6fcd3-146">根拠: 一般的にネットワーク全体で HTTP を使用することが、ベスト プラクティスと見なされます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-146">Rationale: Https across the wire is generally considered best practice.</span></span>

1. <span data-ttu-id="6fcd3-147">**[仮想ネットワーク]**: **[無効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-147">**Virtual Networks**: Select **Disabled**.</span></span>

    <span data-ttu-id="6fcd3-148">根拠: コンテンツは一般向けであり、パブリック コンテンツからのアクセスを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-148">Rationale: The content is public facing and you need to allow access from public clients.</span></span>

<span data-ttu-id="6fcd3-149">次のスクリーンショットは、**[詳細]** タブの設定が完了したことを示しています。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-149">The following screenshot shows the completed settings for the **Advanced** tab.</span></span>

![**詳細** タブが選択されている [ストレージ アカウントを作成する] ブレードのスクリーンショット。](../media-drafts/5-create-storage-account-advanced.png)

### <a name="create"></a><span data-ttu-id="6fcd3-151">作成</span><span class="sxs-lookup"><span data-stu-id="6fcd3-151">Create</span></span>

1. <span data-ttu-id="6fcd3-152">ブレードの下部にある **[確認および作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-152">Click the **Review + create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="6fcd3-153">次の画面で、ブレードの下部にある **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-153">On the next screen, click the **Create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="6fcd3-154">リソースが作成されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-154">Wait for the resource to be created.</span></span>

### <a name="verify"></a><span data-ttu-id="6fcd3-155">確認</span><span class="sxs-lookup"><span data-stu-id="6fcd3-155">Verify</span></span>

1. <span data-ttu-id="6fcd3-156">左サイドバーで **[ストレージ アカウント]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-156">Select the **Storage accounts** link in the left sidebar.</span></span>

1. <span data-ttu-id="6fcd3-157">一覧で新しいストレージ アカウントを見つけて、作成が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-157">Locate the new storage account in the list to verify that creation succeeded.</span></span>

### <a name="clean-up"></a><span data-ttu-id="6fcd3-158">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="6fcd3-158">Clean up</span></span>

1. <span data-ttu-id="6fcd3-159">左サイドバーで **[リソース グループ]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-159">Select the **Resource groups** link in the left sidebar.</span></span>

1. <span data-ttu-id="6fcd3-160">一覧で **[SurfReportResourceGroup]** を見つけます。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-160">Locate **SurfReportResourceGroup** in the list.</span></span>

1. <span data-ttu-id="6fcd3-161">**[SurfReportResourceGroup]** エントリを右クリックして、コンテキスト メニューから **[リソース グループの削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-161">Right-click on the **SurfReportResourceGroup** entry and select **Delete resource group** from the context menu.</span></span>

1. <span data-ttu-id="6fcd3-162">確認フィールドにリソース グループ名を入力します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-162">Type the resource group name into the confirmation field.</span></span>

1. <span data-ttu-id="6fcd3-163">**[削除]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-163">Click the **Delete** button.</span></span>

## <a name="summary"></a><span data-ttu-id="6fcd3-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="6fcd3-164">Summary</span></span>

<span data-ttu-id="6fcd3-165">お客様のビジネス要件による設定でストレージ アカウントを作成しました。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-165">You created a storage account with settings driven by your business requirements.</span></span> <span data-ttu-id="6fcd3-166">たとえば、顧客の所在地が主に南カリフォルニアのため、米国西部のデータセンターを選択しています。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-166">For example, you selected a West US datacenter because your customers were primarily located in southern California.</span></span> <span data-ttu-id="6fcd3-167">これが一般的なフローです。まず、ご自分のデータと目標を分析し、条件に合うストレージ アカウントのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="6fcd3-167">This is a typical flow: first analyze your data and goals and then configure the storage account options to match.</span></span>