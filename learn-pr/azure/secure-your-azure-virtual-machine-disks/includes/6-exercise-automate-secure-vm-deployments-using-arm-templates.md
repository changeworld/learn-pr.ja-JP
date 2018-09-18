<span data-ttu-id="d0077-101">あなたの銀行は機密性の高い顧客情報を扱っており、あなたは自分が使用しているディスク (VM ディスクの展開を含む) を常に暗号化するように要求されています。</span><span class="sxs-lookup"><span data-stu-id="d0077-101">Your banking company handles highly sensitive customer information and wants you to ensure your disks are encrypted at all times, including VM disk deployment.</span></span> <span data-ttu-id="d0077-102">あなたは、会社のデータを保護するためにセキュリティで保護された VM のデプロイを自動化する作業を任されています。</span><span class="sxs-lookup"><span data-stu-id="d0077-102">You've been tasked to automate a secure VM deployment to safeguard your company's data.</span></span>

<span data-ttu-id="d0077-103">このユニットでは、Azure Resource Manager テンプレートを使用して、新しい Windows VM の暗号化を自動的に有効にします。</span><span class="sxs-lookup"><span data-stu-id="d0077-103">In this unit, you'll use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.</span></span>

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a><span data-ttu-id="d0077-104">Azure Resource Manager テンプレートを使った新しい VM の構成とデプロイ</span><span class="sxs-lookup"><span data-stu-id="d0077-104">Configure and deploy a new VM using an Azure Resource Manager template</span></span>

1. <span data-ttu-id="d0077-105">[GitHub の Resource Manager テンプレート](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)に移動し、**[Azure へのデプロイ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0077-105">Go to the [Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), and then click **Deploy to Azure**.</span></span>
1. <span data-ttu-id="d0077-106">Azure portal の **[Azure クイックスタート テンプレート]** ブレードで、**[リソース グループ]** の **[既存のものを使用]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d0077-106">In the Azure portal, on the **Azure quickstart template** blade, under **Resource Group**, select **Use existing**.</span></span> <span data-ttu-id="d0077-107">一覧で **moneyapprg** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d0077-107">In the list, select **moneyapprg**.</span></span>
1. <span data-ttu-id="d0077-108">**[設定]** セクションで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="d0077-108">In the **SETTINGS** section, enter the following information:</span></span>

   - <span data-ttu-id="d0077-109">VM 名: **moneyappsvr02**</span><span class="sxs-lookup"><span data-stu-id="d0077-109">Vm Name: **moneyappsvr02**</span></span>
   - <span data-ttu-id="d0077-110">**管理者ユーザー名**: 前の演習で使用したものと同じ。</span><span class="sxs-lookup"><span data-stu-id="d0077-110">**Admin Username**: Same as you used in the previous exercise.</span></span>
   - <span data-ttu-id="d0077-111">**管理者パスワード**: 前の演習で使用したものと同じ。</span><span class="sxs-lookup"><span data-stu-id="d0077-111">**Admin Password**: Same as you used in the previous exercise.</span></span>
   - <span data-ttu-id="d0077-112">**新しいストレージ アカウント名**: 一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="d0077-112">**New Storage Account Name**: Enter a unique name.</span></span>
   - <span data-ttu-id="d0077-113">**VM サイズ**: **Standard_B1s** など、前の演習で使用したものと同じサイズに置き換えます (同じ Azure リージョンを使用しているので、現在のリージョンでそのサイズを使用できることを確認します)。</span><span class="sxs-lookup"><span data-stu-id="d0077-113">**Vm Size**: Replace with the same size that you used in the previous exercise, such as **Standard_B1s** (as you are using the same Azure region, ensuring that size is available in your current region).</span></span>
   - <span data-ttu-id="d0077-114">**仮想ネットワーク名**: **moneyapprg-vnet**</span><span class="sxs-lookup"><span data-stu-id="d0077-114">**Virtual Network Name**: **moneyapprg-vnet**</span></span>
   - <span data-ttu-id="d0077-115">**サブネット名**: **既定値**</span><span class="sxs-lookup"><span data-stu-id="d0077-115">**Subnet Name**: **default**</span></span>
   - <span data-ttu-id="d0077-116">**AAD クライアント ID**: メモ帳に貼り付けた情報からコピーします。</span><span class="sxs-lookup"><span data-stu-id="d0077-116">**AAD Client ID**: Copy from the information you pasted to Notepad.</span></span>
   - <span data-ttu-id="d0077-117">**AAD クライアント シークレット**: メモ帳に貼り付けた情報からコピーします。</span><span class="sxs-lookup"><span data-stu-id="d0077-117">**AAD Client Secret**: Copy from the information you pasted to Notepad.</span></span>
   - <span data-ttu-id="d0077-118">**キー コンテナー名**: **moneyappkv**</span><span class="sxs-lookup"><span data-stu-id="d0077-118">**Key Vault Name**: **moneyappkv**</span></span>
   - <span data-ttu-id="d0077-119">**キー コンテナー リソース グループ**: **moneyapprg**</span><span class="sxs-lookup"><span data-stu-id="d0077-119">**Key Vault Resource Group**: **moneyapprg**</span></span>
   - <span data-ttu-id="d0077-120">**キー暗号化のキーの URL**: メモ帳に貼り付けた情報からコピーします。</span><span class="sxs-lookup"><span data-stu-id="d0077-120">**Key Encryption Key URL**: Copy from the information you pasted to Notepad.</span></span>
1. <span data-ttu-id="d0077-121">**[上記のご契約条件に同意します]** チェック ボックスをオンにし、**[購入]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0077-121">Select the **I agree to the terms and conditions** check box, and then click **Purchase**.</span></span>

<span data-ttu-id="d0077-122">デプロイが完了するまで最大で 5 分から 10 分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="d0077-122">The deployment may take 5-10 minutes to complete.</span></span>

## <a name="verify-encryption-status-of-new-vm"></a><span data-ttu-id="d0077-123">新しい VM の暗号化の状態を確認する</span><span class="sxs-lookup"><span data-stu-id="d0077-123">Verify encryption status of new VM</span></span>

1. <span data-ttu-id="d0077-124">Azure portal のサイドバーで、**[仮想マシン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0077-124">In the sidebar of the Azure portal, click **Virtual machines**.</span></span>

1. <span data-ttu-id="d0077-125">**[仮想マシン]** ブレードで、**moneyappsvr02** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0077-125">On the **Virtual machines** blade, click **moneyappsvr02**.</span></span>

1. <span data-ttu-id="d0077-126">**[仮想マシン]** ブレードの **[設定]** で、**[ディスク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0077-126">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="d0077-127">**[ディスク]** ブレードで、OS ディスクの暗号化状態が **[有効]** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d0077-127">On the **Disks** blade, notice the OS disk encryption status is **Enabled**.</span></span>
