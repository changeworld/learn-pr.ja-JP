<span data-ttu-id="fa6c7-101">Azure portal を使用すると、PostgreSQL データベース サーバーの管理およびスケーリングを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-101">The Azure portal allows you to manage and scale PostgreSQL database servers.</span></span> <span data-ttu-id="fa6c7-102">あなたは、ランナーのパフォーマンス データを格納するために Azure Database for PostgreSQL サーバーを作成することに決めました。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-102">You decide to create an Azure Database for PostgreSQL server to store runner performance data.</span></span> <span data-ttu-id="fa6c7-103">取得されたデータ ボリュームの履歴から、サーバー ストレージの要件を 10 GB に設定する必要があることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-103">Based on historic captured data volumes, you know your server storage requirements should be set at 10 GB.</span></span> <span data-ttu-id="fa6c7-104">ご自分の処理要件をサポートするには、仮想コアを 1 個備えたコンピューティング世代 Gen 5 のサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-104">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="fa6c7-105">また、通常、25 日間のバックアップを格納していることもわかっています。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-105">You also know that you typically store backups for 25 days.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="fa6c7-106">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-106">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span> 

<span data-ttu-id="fa6c7-107">左側には Azure リソースの作成と管理のメニューが表示されます。画面の残りの部分はダッシュボードになっています。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-107">You'll see the Azure resource creation and management menu on your left and the dashboard filling the rest of the screen.</span></span>

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="fa6c7-108">Azure Database for PostgreSQL サーバーの作成</span><span class="sxs-lookup"><span data-stu-id="fa6c7-108">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="fa6c7-109">サインインした後、既定のダッシュ ボードが表示されるのを確認できます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-109">After you sign in, you'll see the default Dashboard displayed.</span></span> <span data-ttu-id="fa6c7-110">Azure Database for PostgreSQL サーバーの作成に使用可能なオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-110">You have a couple of options available to you to create an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="fa6c7-111">ダッシュボードからは、次のいずれかの操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-111">From the Dashboard, you can either:</span></span>

- <span data-ttu-id="fa6c7-112">**[すべてのサービス]** オプションを選択し、次に **[Azure Database for PostgreSQL サーバー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-112">Select the **All services** option and then search for **Azure Database for PostgreSQL server**.</span></span> <span data-ttu-id="fa6c7-113">ご自分のアカウントで既に構成されているサーバーがあれば、この画面に表示されます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-113">This screen will display any configured servers that are already in your account.</span></span> <span data-ttu-id="fa6c7-114">ここでは、**[追加]** を選択して、新しいサーバーを作成するためのブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-114">From here, you select **Add**, which will take you to the new server creation blade.</span></span>

<span data-ttu-id="fa6c7-115">**または**</span><span class="sxs-lookup"><span data-stu-id="fa6c7-115">**or**</span></span>

- <span data-ttu-id="fa6c7-116">**[リソースの作成]** オプションを選択します。これにより、Azure Marketplace リソース オプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-116">Select the **Create a resource** option, which will present you with Azure Marketplace resource options.</span></span> <span data-ttu-id="fa6c7-117">ここでは、**[データベース]** オプション、**[Azure Database for PostgreSQL]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-117">From here, you select the **Databases** option and choose **Azure Database for PostgreSQL**.</span></span>

### <a name="configure-the-server"></a><span data-ttu-id="fa6c7-118">サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="fa6c7-118">Configure the server</span></span>

<span data-ttu-id="fa6c7-119">ここで、次の図に示すような PostgreSQL サーバーの作成ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-119">You'll now see the PostgreSQL server create blade, similar to the following illustration.</span></span>

![新しい PostgreSQL データベースの作成ブレードを示す Azure portal のスクリーンショット](../media/4-create-blade.png)

> [!NOTE]
> <span data-ttu-id="fa6c7-121">PostgreSQL サーバーを作成する場合は、いくつかの詳細情報を覚えておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-121">You'll need to remember some details as you create the PostgreSQL server.</span></span> <span data-ttu-id="fa6c7-122">たとえば、サーバーにアクセスするためのユーザー名とパスワードがあります。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-122">For example, the username and password to access the server.</span></span> <span data-ttu-id="fa6c7-123">この情報は、後でご自分のサーバーに接続するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-123">You'll use this information to connect to your server later.</span></span>

1. <span data-ttu-id="fa6c7-124">サーバーの一意の名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-124">Choose a unique name for the server.</span></span> <span data-ttu-id="fa6c7-125">名前はすべて小文字とする必要があり、名前には数字とハイフンを含めることができることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-125">Recall that then name must be all lowercase and can have numbers and hyphens.</span></span>

1. <span data-ttu-id="fa6c7-126">サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-126">Select a subscription.</span></span> <span data-ttu-id="fa6c7-127">このフィールドが使用するサブスクリプションに設定されていることを必ず確認します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-127">Check to be sure this field is set to the subscription that you want to use.</span></span>

1. <span data-ttu-id="fa6c7-128">これで、リソース グループを作成するか、または既存のリソース グループを再利用するオプションを使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-128">You now have the option to create or reuse an existing resource group.</span></span> <span data-ttu-id="fa6c7-129">**[既存のものを使用]** を選択し、ドロップダウンから "<rgn>[サンドボックス リソース グループ名]</rgn>" を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-129">Select **Use existing** and choose "<rgn>[Sandbox resource group name]</rgn>" from the dropdown.</span></span> <span data-ttu-id="fa6c7-130">このモジュールの残りの部分には、このグループを使用します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-130">You'll use this group for the rest of this module.</span></span>

1. <span data-ttu-id="fa6c7-131">ご自分の新しいサーバーのソースを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-131">Select the source of your new server.</span></span> <span data-ttu-id="fa6c7-132">このラボでは、このオプションを _[空白]_ のままとします。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-132">For this lab, you'll leave the option set to _Blank_.</span></span> <span data-ttu-id="fa6c7-133">既存のサーバーのバックアップを復元する場合は、オプションを _[バックアップ]_ に変更できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-133">Recall that you can change the option to _Back up_ if you want to restore an existing server backup.</span></span>

1. <span data-ttu-id="fa6c7-134">新しいサーバー用の管理者ログインとして使用するログイン名を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-134">Choose a login name to use as an administrator login for the new server.</span></span> <span data-ttu-id="fa6c7-135">管理者のログイン名に azure_superuser、azure_pg_admin、admin、administrator、root、guest、または public は使用できないことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-135">Recall that the admin login name can't be azure_superuser, azure_pg_admin, admin, administrator, root, guest, or public.</span></span> <span data-ttu-id="fa6c7-136">pg_ で始めることはできません。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-136">It can't start with pg_.</span></span> <span data-ttu-id="fa6c7-137">今後の使用に備えて名前を覚えておくか書き留めます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-137">Remember or write down the name for future use.</span></span>

1. <span data-ttu-id="fa6c7-138">上記の管理者ログイン名と一緒に使用するパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-138">Choose a password to use with the above administrator login name.</span></span> <span data-ttu-id="fa6c7-139">パスワードには次のカテゴリのうち 3 つの文字が含まれている必要があることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-139">Recall,= that our password must include characters from three of the following categories:</span></span>
   - <span data-ttu-id="fa6c7-140">英大文字</span><span class="sxs-lookup"><span data-stu-id="fa6c7-140">English uppercase letters</span></span>
   - <span data-ttu-id="fa6c7-141">英小文字</span><span class="sxs-lookup"><span data-stu-id="fa6c7-141">English lowercase letters</span></span>
   - <span data-ttu-id="fa6c7-142">数字 (0 から 9)</span><span class="sxs-lookup"><span data-stu-id="fa6c7-142">Numbers (0 through 9)</span></span>
   - <span data-ttu-id="fa6c7-143">英数字以外の文字 (!、$、#、% など)</span><span class="sxs-lookup"><span data-stu-id="fa6c7-143">Non-alphanumeric characters (!, $, #, %, and so on)</span></span>

1. <span data-ttu-id="fa6c7-144">パスワードをもう一度入力して、そのパスワードを確定します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-144">Retype the password to confirm your password.</span></span>

1. <span data-ttu-id="fa6c7-145">ご自分のサーバーの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-145">Choose a location for your server.</span></span> <span data-ttu-id="fa6c7-146">次の一覧から自分に最も近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-146">You'll want to choose a location closest to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]


1. <span data-ttu-id="fa6c7-147">サーバーのバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-147">You'll now select the version of your server.</span></span> <span data-ttu-id="fa6c7-148">PostgreSQL の最新バージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-148">Select the latest version of PostgreSQL.</span></span>

1. <span data-ttu-id="fa6c7-149">最後から 2 番目の手順として、**[価格レベル]** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-149">As the second to last step, select the **Pricing tier** option.</span></span>

    <span data-ttu-id="fa6c7-150">特定のストレージ オプションとコンピューティング オプションを使用してご自分のサーバーを構成する必要があることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-150">Recall that you need to configure your server with specific storage and compute options:</span></span>

    - <span data-ttu-id="fa6c7-151">10 GB のディスク ストレージ</span><span class="sxs-lookup"><span data-stu-id="fa6c7-151">10 GB of disk storage</span></span>
    - <span data-ttu-id="fa6c7-152">コンピューティング世代 Gen 5 のサポート</span><span class="sxs-lookup"><span data-stu-id="fa6c7-152">Compute Generation 5 support</span></span>
    - <span data-ttu-id="fa6c7-153">保有期間 25 日</span><span class="sxs-lookup"><span data-stu-id="fa6c7-153">Retention period of 25 days</span></span>

    <span data-ttu-id="fa6c7-154">**[価格レベル]** をクリックして価格レベル ブレードにアクセスし、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-154">Click **Pricing tier** to access the pricing tier blade and make the following changes:</span></span>

    - <span data-ttu-id="fa6c7-155">**[Basic]** オプション タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-155">Choose the **Basic** option tab.</span></span>
    - <span data-ttu-id="fa6c7-156">**[Gen 5 Computation Generation]\(コンピューティング世代 Gen 5\)** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-156">Choose the **Gen 5 Computation Generation** option.</span></span>
    - <span data-ttu-id="fa6c7-157">**[仮想コア]** スライダーから [1 個の仮想コア] を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-157">Choose 1 vCore from the **vCore** slider.</span></span> <span data-ttu-id="fa6c7-158">スライダーを変更した結果、**[価格の概要]** にどのような影響があったかを注目してください。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-158">Notice how the changes in the slider affect the **Price Summary**.</span></span>
    - <span data-ttu-id="fa6c7-159">**[ストレージ]** スライダーから 10 GB を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-159">Choose 10 GB from the **Storage** slider.</span></span> <span data-ttu-id="fa6c7-160">10 GB に正確にスライドするのが難しい場合は、キーボードの左方向キーと右方向キーを使用して正確な値を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-160">If you're having trouble sliding to exactly 10 GB, you can use the left and right cursor keys on your keyboard to get a precise value.</span></span>
    - <span data-ttu-id="fa6c7-161">**[バックアップの保有期間]** スライダーから 25 日を選択します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-161">Choose 25 Days from the **Backup Retention Period** slider.</span></span>
    - <span data-ttu-id="fa6c7-162">バックアップ冗長性オプションの設定は **[ローカル冗長]** のままにします。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-162">Leave the Backup Redudancy Options set to **Locally Redundant**.</span></span>

    ![新しい PostgreSQL データベースのデータベース価格レベルを示す Azure portal のスクリーンショット](../media/4-azure-db-pricing-tier.png)

1. <span data-ttu-id="fa6c7-164">右側に表示される価格の概要を確認します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-164">Check out the price summary on the right side.</span></span> <span data-ttu-id="fa6c7-165">サーバーのコスト内訳と月額料金の見積もりが提供します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-165">It provides a cost-breakdown for the server along with an estimated monthly cost.</span></span>

1. <span data-ttu-id="fa6c7-166">選択が完了して選択内容をコミットしたら **[OK]** をクリックして価格レベルのオプションを閉じます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-166">Click **OK** after you're satisfied with your selection to commit your selections and close the pricing tier options.</span></span>

1. <span data-ttu-id="fa6c7-167">残りの作業は、入力した値を確認して **[作成]** をクリックするだけとなりました。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-167">All that's left now is to review the values you entered and click **Create**.</span></span> <span data-ttu-id="fa6c7-168">作成には数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-168">Creation can take several minutes.</span></span> <span data-ttu-id="fa6c7-169">Azure portal 画面の上部にある通知アイコン (ベル) を選択すると、進行状況を監視できます。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-169">You can select the Notifications icon (a bell) at the top of the Azure portal screen to monitor progress.</span></span>

<span data-ttu-id="fa6c7-170">これで PostgreSQL サーバーが使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-170">You now have a PostgreSQL server available.</span></span> <span data-ttu-id="fa6c7-171">次のユニットでは、Azure CLI を使用して、上記と同じサーバーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fa6c7-171">In the next unit, you'll see how to create the same server using the Azure CLI.</span></span>
