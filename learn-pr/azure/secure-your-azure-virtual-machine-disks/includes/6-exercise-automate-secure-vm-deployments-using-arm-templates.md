<span data-ttu-id="89365-101">あなたの銀行は機密性の高い顧客情報を扱っており、あなたは自分が使用しているディスク (VM ディスクの展開を含む) を常に暗号化するように要求されています。</span><span class="sxs-lookup"><span data-stu-id="89365-101">Your banking company handles highly sensitive customer information and wants you to ensure your disks are encrypted at all times, including VM disk deployment.</span></span> <span data-ttu-id="89365-102">あなたは、会社のデータを保護するためにセキュリティで保護された VM の展開を自動化する作業を任されています。</span><span class="sxs-lookup"><span data-stu-id="89365-102">You've been tasked to automate a secure VM deployment to safeguard your company's data.</span></span>

<span data-ttu-id="89365-103">このユニットでは、ARM テンプレートを使用して、新しい Windows VM の暗号化を自動的に有効にします。</span><span class="sxs-lookup"><span data-stu-id="89365-103">In this unit, you'll use an ARM template to automatically enable encryption for new Windows VMs.</span></span>

## <a name="configure-and-deploy-a-new-vm-using-an-arm-template"></a><span data-ttu-id="89365-104">ARM テンプレートを使用して新しい VM を構成および展開する</span><span class="sxs-lookup"><span data-stu-id="89365-104">Configure and deploy a new VM using an ARM template</span></span>

1. <span data-ttu-id="89365-105">[GitHub の Resource Manager テンプレート](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)に移動し、**[Deploy to Azure]\(Azure に展開\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89365-105">Go to the [Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), and then click **Deploy to Azure**.</span></span>
1. <span data-ttu-id="89365-106">Azure portal の **[Azure クイックスタート テンプレート]** ブレードで、**[リソース グループ]** の **[既存のものを使用]** を選択し、一覧から **moneyapprg** を選択します。</span><span class="sxs-lookup"><span data-stu-id="89365-106">In the Azure portal, on the **Azure quickstart template** blade, under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>
1. <span data-ttu-id="89365-107">**[設定]** セクションで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="89365-107">In the **SETTINGS** section, enter the following information:</span></span>

   - <span data-ttu-id="89365-108">VM 名: **moneyappsvr02**</span><span class="sxs-lookup"><span data-stu-id="89365-108">Vm Name: **moneyappsvr02**</span></span>
   - <span data-ttu-id="89365-109">**管理者ユーザー名**: 前の演習で使用したものと同じ</span><span class="sxs-lookup"><span data-stu-id="89365-109">**Admin Username**: same as you used in the previous exercise</span></span>
   - <span data-ttu-id="89365-110">**管理者パスワード**: 前の演習で使用したものと同じ</span><span class="sxs-lookup"><span data-stu-id="89365-110">**Admin Password**: same as you used in the previous exercise</span></span>
   - <span data-ttu-id="89365-111">**新しいストレージ アカウント名**: 一意の名前を入力します</span><span class="sxs-lookup"><span data-stu-id="89365-111">**New Storage Account Name**: enter a unique name</span></span>
   - <span data-ttu-id="89365-112">**VM サイズ**: **Standard_B1s** など、前の演習で使用したものと同じサイズに置き換えます (同じ Azure リージョンを使用しているので、現在のリージョンでそのサイズを使用できることを確認します)</span><span class="sxs-lookup"><span data-stu-id="89365-112">**Vm Size**: replace with the same size that you used in the previous exercise, such as **Standard_B1s** (as you are using the same Azure region, ensuring that size is available in your current region)</span></span>
   - <span data-ttu-id="89365-113">**仮想ネットワーク名**: **moneyapprg-vnet**</span><span class="sxs-lookup"><span data-stu-id="89365-113">**Virtual Network Name**: **moneyapprg-vnet**</span></span>
   - <span data-ttu-id="89365-114">**サブネット名**: **既定値**</span><span class="sxs-lookup"><span data-stu-id="89365-114">**Subnet Name**: **default**</span></span>
   - <span data-ttu-id="89365-115">**AAD クライアント ID**: メモ帳に貼り付けた情報からコピーします</span><span class="sxs-lookup"><span data-stu-id="89365-115">**AAD Client Id**: copy from the information you pasted to Notepad</span></span>
   - <span data-ttu-id="89365-116">**AAD クライアント シークレット**: メモ帳に貼り付けた情報からコピーします</span><span class="sxs-lookup"><span data-stu-id="89365-116">**AAD Client Secret**: copy from the information you pasted to Notepad</span></span>
   - <span data-ttu-id="89365-117">**キー コンテナー名**: **moneyappkv**</span><span class="sxs-lookup"><span data-stu-id="89365-117">**Key Vault Name**: **moneyappkv**</span></span>
   - <span data-ttu-id="89365-118">**キー コンテナー リソース グループ**: **moneyapprg**</span><span class="sxs-lookup"><span data-stu-id="89365-118">**Key Vault Resource Group**: **moneyapprg**</span></span>
   - <span data-ttu-id="89365-119">**キー暗号化のキーの URL**: メモ帳に貼り付けた情報からコピーします</span><span class="sxs-lookup"><span data-stu-id="89365-119">**Key Encryption Key URL**: copy from the information you pasted to Notepad</span></span>
   - <span data-ttu-id="89365-120">**[上記のご契約条件に同意します]** チェック ボックスをオンにし、**[購入]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89365-120">Select the **I agree to the terms and conditions** check box, and then click **Purchase**.</span></span>
1. <span data-ttu-id="89365-121">展開が完了するまで最大で 5 ～ 10 分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="89365-121">The deployment may take 5 - 10 minutes to complete.</span></span>

## <a name="verify-encryption-status-of-new-vm"></a><span data-ttu-id="89365-122">新しい VM の暗号化の状態を確認する</span><span class="sxs-lookup"><span data-stu-id="89365-122">Verify encryption status of new VM</span></span>

1. <span data-ttu-id="89365-123">Azure portal のサイドバーで、**[仮想マシン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89365-123">In the sidebar of the Azure portal, click **Virtual machines**.</span></span>

1. <span data-ttu-id="89365-124">**[仮想マシン]** ブレードで、**moneyappsvr02** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89365-124">On the **Virtual machines** blade, click **moneyappsvr02**.</span></span>

1. <span data-ttu-id="89365-125">**[仮想マシン]** ブレードの **[設定]** で、**[ディスク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89365-125">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="89365-126">**[ディスク]** ブレードで、OS ディスクの暗号化状態が **[有効]** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="89365-126">On the **Disks** blade, notice the OS disk encryption status is **Enabled**.</span></span>
