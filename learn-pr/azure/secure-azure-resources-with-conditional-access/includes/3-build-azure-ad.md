<span data-ttu-id="823d2-101">あなたは、Azure AD をデプロイし、Azure portal にアクセスする際に、Azure によって多要素認証を要求する条件付きアクセス ポリシーを使用することを決めました。</span><span class="sxs-lookup"><span data-stu-id="823d2-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="823d2-102">あなたは、ディレクトリを作成し、一時的なライセンスを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="823d2-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="823d2-103">ディレクトリを作成する</span><span class="sxs-lookup"><span data-stu-id="823d2-103">Create a directory</span></span>
<span data-ttu-id="823d2-104">運用環境ユーザーに影響を与えることなくテストできる、First Up Consultants 用の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="823d2-104">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="823d2-105">[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="823d2-105">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="823d2-106">左側のナビゲーション ウィンドウで、**[リソースの作成]** > **[Identity]** > **[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-106">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

1. <span data-ttu-id="823d2-107">**[ディレクトリの作成]** ブレードで、**[組織名]** と **[初期ドメイン名]** に次の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="823d2-107">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="823d2-108">組織名: `First Up Consultants`。</span><span class="sxs-lookup"><span data-stu-id="823d2-108">ORGANIZATION NAME: `First Up Consultants`.</span></span>
   1. <span data-ttu-id="823d2-109">初期ドメイン名: `firstupconsultants<XXXX>.onmicrosoft.com`。</span><span class="sxs-lookup"><span data-stu-id="823d2-109">INITIAL DOMAIN NAME: `firstupconsultants<XXXX>.onmicrosoft.com`.</span></span>

1. <span data-ttu-id="823d2-110">ディレクトリが作成されるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="823d2-110">Wait for the directory to be created.</span></span> <span data-ttu-id="823d2-111">リンクをクリックして、新しいディレクトリに切り替えるか、ウィンドウの上部にある **[ディレクトリおよびサブスクリプションのフィルター]** をクリックして、新しく作成されたディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="823d2-111">Click the link to switch to the new directory or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="823d2-112">試用版ライセンスを取得する</span><span class="sxs-lookup"><span data-stu-id="823d2-112">Get trial licenses</span></span>

<span data-ttu-id="823d2-113">条件付きアクセスや多要素認証などの機能を使用するには、少なくとも試用版ライセンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="823d2-113">In order for you to use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="823d2-114">次の手順は、試用版ライセンスを有効にする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="823d2-114">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="823d2-115">Azure AD の **[概要]** ウィンドウで、**無料試用版を開始する**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-115">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="823d2-116">**[Azure AD Premium P2]** 項目の下で、**[無料試用版]**、**[アクティブ化]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-116">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="823d2-117">テスト ユーザーを作成する</span><span class="sxs-lookup"><span data-stu-id="823d2-117">Create a test user</span></span>

<span data-ttu-id="823d2-118">ユーザーを使ってこれをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="823d2-118">We're going to need to test this out with a user.</span></span> <span data-ttu-id="823d2-119">Isabella Simonsen (別のチームのメンバー) が、あなたに協力することを申し出てくれました。ディレクトリ内に彼女のアカウントが必要になるため、彼女のアカウントを作成する手順を行います。</span><span class="sxs-lookup"><span data-stu-id="823d2-119">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="823d2-120">**[Azure Active Directory]** > **[ユーザー]** > **[すべてのユーザー]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="823d2-120">Browse to **Azure Active Directory** > **Users** > **All users**.</span></span>

1. <span data-ttu-id="823d2-121">**[新しいユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-121">Click **New user**.</span></span>

1. <span data-ttu-id="823d2-122">ユーザー名が **Isabella Simonsen** という名前のユーザー名を作成します。</span><span class="sxs-lookup"><span data-stu-id="823d2-122">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   `Isabella@firstupconsultants<XXXX>.onmicrosoft.com`

   <span data-ttu-id="823d2-123">@ の後のドメインを、前述の「*ディレクトリを作成する*」セクションで作成したドメインと一致させます。</span><span class="sxs-lookup"><span data-stu-id="823d2-123">Match the domain after the @ with the domain you created in the *Create a directory* section above.</span></span>

1. <span data-ttu-id="823d2-124">このユーザーの **[パスワードを表示]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="823d2-124">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="823d2-125">後でテストするときに使用できるように、パスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="823d2-125">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="823d2-126">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-126">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="823d2-127">パイロット グループを作成する</span><span class="sxs-lookup"><span data-stu-id="823d2-127">Create a pilot group</span></span>

<span data-ttu-id="823d2-128">作成するポリシーをユーザーのグループに割り当てますが、このポリシーを割り当てるグループを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="823d2-128">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="823d2-129">次の手順に従って、パイロット デプロイのためのセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="823d2-129">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="823d2-130">**[Azure Active Directory]** > **[グループ]** > **[すべてのグループ]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="823d2-130">Browse to **Azure Active Directory** > **Groups** > **All groups**.</span></span>

1. <span data-ttu-id="823d2-131">**[新しいグループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-131">Click **New group**.</span></span>

1. <span data-ttu-id="823d2-132">グループの種類: **セキュリティ**。</span><span class="sxs-lookup"><span data-stu-id="823d2-132">Group type **Security**.</span></span>

1. <span data-ttu-id="823d2-133">グループ名: **CA-MFA-AzurePortal**。</span><span class="sxs-lookup"><span data-stu-id="823d2-133">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="823d2-134">メンバーシップの種類: **割り当て済み**。</span><span class="sxs-lookup"><span data-stu-id="823d2-134">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="823d2-135">前の手順で作成したユーザーを選択して、**[選択]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="823d2-135">Select the user that we created in the previous step and choose **Select**.</span></span>

1. <span data-ttu-id="823d2-136">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="823d2-136">Click **Create**.</span></span>

## <a name="summary"></a><span data-ttu-id="823d2-137">まとめ</span><span class="sxs-lookup"><span data-stu-id="823d2-137">Summary</span></span>

<span data-ttu-id="823d2-138">このユニットでは、Azure portal で試用版ライセンスのディレクトリ、テスト ユーザー、およびパイロット グループを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="823d2-138">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>
