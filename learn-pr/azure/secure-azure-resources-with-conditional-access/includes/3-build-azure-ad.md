<span data-ttu-id="08406-101">あなたは、Azure AD をデプロイし、Azure portal にアクセスする際に、Azure によって多要素認証を要求する条件付きアクセス ポリシーを使用することを決めました。</span><span class="sxs-lookup"><span data-stu-id="08406-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="08406-102">あなたは、ディレクトリを作成し、一時的なライセンスを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="08406-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a><span data-ttu-id="08406-103">ラボを起動して Azure portal にサインインする</span><span class="sxs-lookup"><span data-stu-id="08406-103">Launch lab and sign in to the Azure portal</span></span>

1. <span data-ttu-id="08406-104">ラボを起動するには、上記の **[Launch Lab]\(ラボの起動\)** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-104">Click the **Launch Lab** link above to launch the lab.</span></span>

> [!NOTE]
> <span data-ttu-id="08406-105">ラボの起動後、サインインするために必要なユーザー名とパスワードは、手順の横にある **[リソース]** タブに配置されます。</span><span class="sxs-lookup"><span data-stu-id="08406-105">After launching the lab, the username and password you need to sign in with is located on the **Resources** tab next to the instructions.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="08406-106">ディレクトリを作成する</span><span class="sxs-lookup"><span data-stu-id="08406-106">Create a directory</span></span>

<span data-ttu-id="08406-107">運用環境ユーザーに影響を与えることなくテストできる、First Up Consultants 用の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="08406-107">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="08406-108">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="08406-108">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

2. <span data-ttu-id="08406-109">左側のナビゲーション ウィンドウで、**[リソースの作成]** > **[ID]** > **[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-109">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

3. <span data-ttu-id="08406-110">**[ディレクトリの作成]** ブレードで、**[組織名]** と **[初期ドメイン名]** に次の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="08406-110">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="08406-111">組織名: `First Up Consultants`。</span><span class="sxs-lookup"><span data-stu-id="08406-111">Organization Name: `First Up Consultants`.</span></span>
   2. <span data-ttu-id="08406-112">初期ドメイン名: `firstupconsultants<XXXXXXX>`。<XXXXXXX> は指示ウィンドウの上部にある **[リソース]** タブのユーザー名の後ろの数値です。</span><span class="sxs-lookup"><span data-stu-id="08406-112">Initial Domain Name: `firstupconsultants<XXXXXXX>` where <XXXXXXX> is the number after the username on the **Resources** tab at the top of the instructions window.</span></span>

4. <span data-ttu-id="08406-113">ディレクトリが作成されるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="08406-113">Wait for the directory to be created.</span></span> <span data-ttu-id="08406-114">リンクをクリックして、新しいディレクトリに切り替えるか、ウィンドウの上部にある **[ディレクトリおよびサブスクリプションのフィルター]** をクリックして、新しく作成されたディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="08406-114">Click the link to switch to the new directory or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="08406-115">試用版ライセンスを取得する</span><span class="sxs-lookup"><span data-stu-id="08406-115">Get trial licenses</span></span>

<span data-ttu-id="08406-116">条件付きアクセスや多要素認証などの機能を使用するには、少なくとも試用版ライセンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="08406-116">To use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="08406-117">次の手順は、試用版ライセンスを有効にする方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="08406-117">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="08406-118">Azure AD の **[概要]** ウィンドウで、**無料試用版を開始する**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-118">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="08406-119">**[Azure AD Premium P2]** 項目の下で、**[無料試用版]**、**[アクティブ化]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-119">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="08406-120">テスト ユーザーを作成する</span><span class="sxs-lookup"><span data-stu-id="08406-120">Create a test user</span></span>

<span data-ttu-id="08406-121">ユーザーを使ってこれをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="08406-121">We're going to need to test this out with a user.</span></span> <span data-ttu-id="08406-122">Isabella Simonsen (別のチームのメンバー) が、あなたに協力することを申し出てくれました。ディレクトリ内に彼女のアカウントが必要になるため、彼女のアカウントを作成する手順を行います。</span><span class="sxs-lookup"><span data-stu-id="08406-122">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="08406-123">**[Azure Active Directory]** > **[ユーザー]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="08406-123">Browse to **Azure Active Directory** > **Users**.</span></span>

1. <span data-ttu-id="08406-124">**[新しいユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-124">Click **New user**.</span></span>

1. <span data-ttu-id="08406-125">ユーザー名が **Isabella Simonsen** という名前のユーザー名を作成します。</span><span class="sxs-lookup"><span data-stu-id="08406-125">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   `Isabella@firstupconsultants<XXXXXXX>.onmicrosoft.com`

   <span data-ttu-id="08406-126">@ の後のドメインを、前述の「*ディレクトリを作成する*」セクションで作成したドメインと一致させます。</span><span class="sxs-lookup"><span data-stu-id="08406-126">Match the domain after the @ with the domain you created in the *Create a directory* section above.</span></span>

1. <span data-ttu-id="08406-127">このユーザーの **[パスワードを表示]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="08406-127">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="08406-128">後でテストするときに使用できるように、パスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="08406-128">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="08406-129">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-129">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="08406-130">パイロット グループを作成する</span><span class="sxs-lookup"><span data-stu-id="08406-130">Create a pilot group</span></span>

<span data-ttu-id="08406-131">作成するポリシーをユーザーのグループに割り当てますが、このポリシーを割り当てるグループを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="08406-131">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="08406-132">次の手順に従って、パイロット デプロイのためのセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="08406-132">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="08406-133">**[Azure Active Directory]** > **[グループ]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="08406-133">Browse to **Azure Active Directory** > **Groups**.</span></span>

1. <span data-ttu-id="08406-134">**[新しいグループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-134">Click **New group**.</span></span>

1. <span data-ttu-id="08406-135">グループの種類: **セキュリティ**。</span><span class="sxs-lookup"><span data-stu-id="08406-135">Group type **Security**.</span></span>

1. <span data-ttu-id="08406-136">グループ名: **CA-MFA-AzurePortal**。</span><span class="sxs-lookup"><span data-stu-id="08406-136">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="08406-137">メンバーシップの種類: **割り当て済み**。</span><span class="sxs-lookup"><span data-stu-id="08406-137">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="08406-138">前の手順で作成したユーザーを選択して、**[選択]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="08406-138">Select the user that we created in the previous step and choose **Select**.</span></span>

1. <span data-ttu-id="08406-139">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08406-139">Click **Create**.</span></span>

<span data-ttu-id="08406-140">このユニットでは、Azure portal で試用版ライセンスのディレクトリ、テスト ユーザー、およびパイロット グループを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="08406-140">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>