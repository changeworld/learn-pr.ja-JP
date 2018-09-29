<span data-ttu-id="58cf2-101">このユニットでは、Azure Resource Manager テンプレートを使用し、以前に作成した Windows VM を復号します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-101">In this unit, you'll use an Azure Resource Manager template to decrypt our Windows VM we created earlier.</span></span> <span data-ttu-id="58cf2-102">Windows VM で OS ドライブを暗号化しました。</span><span class="sxs-lookup"><span data-stu-id="58cf2-102">We encrypted the OS drive on our Windows VM.</span></span> <span data-ttu-id="58cf2-103">しかしながら、この OS ドライブには取り扱いに慎重を要する情報が格納する予定がありません。そのため、暗号化しなくても問題ないでしょう。</span><span class="sxs-lookup"><span data-stu-id="58cf2-103">However, the OS drive won't have any confidential information on it, so we could leave it unencrypted.</span></span> <span data-ttu-id="58cf2-104">それではテンプレートを使用し、OS ドライブを復号しましょう。</span><span class="sxs-lookup"><span data-stu-id="58cf2-104">Let's use a template to decrypt the OS drive.</span></span>

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a><span data-ttu-id="58cf2-105">Azure Resource Manager テンプレートを使用して新しい VM を構成し、デプロイする</span><span class="sxs-lookup"><span data-stu-id="58cf2-105">Configure and deploy a new VM using an Azure Resource Manager template</span></span>

<span data-ttu-id="58cf2-106">Microsoft が Github で公開しているテンプレートを使用し、新しい VM を作成します。その際、既定で暗号化をオンにします。</span><span class="sxs-lookup"><span data-stu-id="58cf2-106">We're going to use a template Microsoft has published on Github to create a new VM with encryption turned on by default.</span></span>

1. <span data-ttu-id="58cf2-107">サンドボックスをアクティブ化したときと同じアカウントを使用して [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="58cf2-107">Sign into the [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) with the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="58cf2-108">左サイドバーで **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="58cf2-108">Click **Create a Resource** in the left sidebar.</span></span>

1. <span data-ttu-id="58cf2-109">検索ボックスに「**Template**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-109">Type **Template** in the search box.</span></span>

1. <span data-ttu-id="58cf2-110">検索結果の一覧から **[テンプレートのデプロイ]** を選択し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="58cf2-110">Select **Template Deployment** from the resulting list and click **Create**.</span></span>

    ![スクリーンショット。[テンプレートのデプロイ] の項目が選択され、[作成] ボタンが強調表示されています。](../media/6-create-template.png)

1. <span data-ttu-id="58cf2-112">**[テンプレートの選択]** 検索ボックスに「201-decrypt」と入力したところで "201-decrypt-running-windows-vm-without-aad" テンプレートが表示されるのでそれを選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-112">In the **Select a template** search box, start typing "201-decrypt" and select the "201-decrypt-running-windows-vm-without-aad" template.</span></span>

    ![[テンプレートの選択] 検索ボックスのスクリーンショット。入力候補が表示されます。](../media/6-custom-deployment.png)

1. <span data-ttu-id="58cf2-114">**[テンプレートの選択]** をクリックし、テンプレート ランナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-114">Click **Select Template** to launch the template runner.</span></span>

1. <span data-ttu-id="58cf2-115">設定ビューで次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-115">In the settings view, enter the following information:</span></span>
    - <span data-ttu-id="58cf2-116">**[サブスクリプション]** には _[コンシェルジェ サブスクリプション]_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-116">Select _Concierge Subscription_ for the **Subscription**.</span></span>
    - <span data-ttu-id="58cf2-117">サンドボックスで作成した自分のリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-117">Select your Resource Group, created in the sandbox.</span></span>
    - <span data-ttu-id="58cf2-118">VM を作成した場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-118">Select the location you created the VM in.</span></span>
    - <span data-ttu-id="58cf2-119">**[VM 名]** に「fmdata-vm01」と入力します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-119">Enter "fmdata-vm01" for the **VM Name**.</span></span>
    - <span data-ttu-id="58cf2-120">**[ボリュームの種類]** は _[すべて]_ が選択されているまま残します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-120">Leave the **Volume Type** as _All_.</span></span>

1. <span data-ttu-id="58cf2-121">**[上記の使用条件に同意する]** チェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-121">Select the **I agree to the terms and conditions** check box.</span></span>
1. <span data-ttu-id="58cf2-122">**[購入]** をクリックし、テンプレートを実行します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-122">Click **Purchase** to run the template.</span></span> <span data-ttu-id="58cf2-123">これにはコストが発生しません。これは標準のボタンです。</span><span class="sxs-lookup"><span data-stu-id="58cf2-123">Note that there is no cost to this - it's a standard button.</span></span>

<span data-ttu-id="58cf2-124">デプロイが完了するまでに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="58cf2-124">The deployment may take a few minutes to complete.</span></span>

## <a name="verify-the-encryption-status-of-the-vm"></a><span data-ttu-id="58cf2-125">VM の暗号化状態を確認する</span><span class="sxs-lookup"><span data-stu-id="58cf2-125">Verify the encryption status of the VM</span></span>

1. <span data-ttu-id="58cf2-126">Azure portal のサイドバーで **[Virtual machines]** をクリックし、自分の VM **fmdata-vm01** を選択します。</span><span class="sxs-lookup"><span data-stu-id="58cf2-126">In the sidebar of the Azure portal, click **Virtual machines** and select your VM **fmdata-vm01**.</span></span> <span data-ttu-id="58cf2-127">あるいは、**[すべてのリソース]** から名前で VM を検索できます。</span><span class="sxs-lookup"><span data-stu-id="58cf2-127">Alternatively, you can search for your VM by name from **All Resources**.</span></span>

1. <span data-ttu-id="58cf2-128">**[仮想マシン]** ブレードの **[設定]** で、**[ディスク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="58cf2-128">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="58cf2-129">これで、**[ディスク]** ブレードで、OS ディスクの暗号化状態が **[有効]** になりました。</span><span class="sxs-lookup"><span data-stu-id="58cf2-129">On the **Disks** blade, notice the OS disk encryption status is now **Disabled**.</span></span>
