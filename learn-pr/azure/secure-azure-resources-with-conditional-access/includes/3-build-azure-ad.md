<span data-ttu-id="62981-101">あなたは、Azure AD をデプロイし、Azure portal にアクセスする際に、Azure によって多要素認証を要求する条件付きアクセス ポリシーを使用することを決めました。</span><span class="sxs-lookup"><span data-stu-id="62981-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="62981-102">あなたは、ディレクトリを作成し、一時的なライセンスを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62981-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a><span data-ttu-id="62981-103">ラボを開始して、Azure portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="62981-103">Launch lab and sign in to the Azure portal</span></span>

1. <span data-ttu-id="62981-104">ラボを開始するには、上記の **[VM モードの起動]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-104">Click the **Launch VM mode** link above to launch the lab.</span></span>

1. <span data-ttu-id="62981-105">Azure Portal に LabAdmin-<XXXXXXX>@triplecrownlabsoutlook.onmicrosoft.com としてサインインします。</span><span class="sxs-lookup"><span data-stu-id="62981-105">Sign in to the Azure portal as LabAdmin-<XXXXXXX>@triplecrownlabsoutlook.onmicrosoft.com.</span></span> <span data-ttu-id="62981-106">ユーザー名とパスワードは、この指示ウィンドウの上部にある [リソース] タブで見つかります。</span><span class="sxs-lookup"><span data-stu-id="62981-106">You can find the username and password on the Resources tab at the top of the instructions window.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="62981-107">ディレクトリを作成する</span><span class="sxs-lookup"><span data-stu-id="62981-107">Create a directory</span></span>

<span data-ttu-id="62981-108">運用環境ユーザーに影響を与えることなくテストできる、First Up Consultants 用の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="62981-108">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="62981-109">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="62981-109">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="62981-110">左側のナビゲーション ウィンドウで、**[リソースの作成]** > **[ID]** > **[Azure Active Directory]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-110">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

1. <span data-ttu-id="62981-111">**[ディレクトリの作成]** ブレードで、**[組織名]** と **[初期ドメイン名]** に次の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="62981-111">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="62981-112">組織名: `First Up Consultants`。</span><span class="sxs-lookup"><span data-stu-id="62981-112">Organization Name: `First Up Consultants`.</span></span>
   1. <span data-ttu-id="62981-113">初期ドメイン名: `firstupconsultants<XXXXXXX>`。<XXXXXXX> は指示ウィンドウの上部にある **[リソース]** タブのユーザー名の後ろの数値です。</span><span class="sxs-lookup"><span data-stu-id="62981-113">Initial Domain Name: `firstupconsultants<XXXXXXX>` where <XXXXXXX> is the number after the username on the **Resources** tab at the top of the instructions window.</span></span>

1. <span data-ttu-id="62981-114">ディレクトリが作成されるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="62981-114">Wait for the directory to be created.</span></span> <span data-ttu-id="62981-115">リンクをクリックして、新しいディレクトリに切り替えるか、ウィンドウの上部にある **[ディレクトリおよびサブスクリプションのフィルター]** をクリックして、新しく作成されたディレクトリを選択します。</span><span class="sxs-lookup"><span data-stu-id="62981-115">Click the link to switch to the new directory or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="62981-116">試用版ライセンスを取得する</span><span class="sxs-lookup"><span data-stu-id="62981-116">Get trial licenses</span></span>

<span data-ttu-id="62981-117">条件付きアクセスや多要素認証などの機能を使用するには、少なくとも試用版ライセンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="62981-117">To use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="62981-118">次の手順は、試用版ライセンスを有効にする方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="62981-118">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="62981-119">Azure AD の **[概要]** ウィンドウで、**無料試用版を開始する**ためのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-119">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="62981-120">**[Azure AD Premium P2]** 項目の下で、**[無料試用版]**、**[アクティブ化]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-120">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="62981-121">テスト ユーザーを作成する</span><span class="sxs-lookup"><span data-stu-id="62981-121">Create a test user</span></span>

<span data-ttu-id="62981-122">ユーザーを使ってこれをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="62981-122">We're going to need to test this out with a user.</span></span> <span data-ttu-id="62981-123">Isabella Simonsen (別のチームのメンバー) が、あなたに協力することを申し出てくれました。ディレクトリ内に彼女のアカウントが必要になるため、彼女のアカウントを作成する手順を行います。</span><span class="sxs-lookup"><span data-stu-id="62981-123">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="62981-124">**[Azure Active Directory]** > **[ユーザー]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="62981-124">Browse to **Azure Active Directory** > **Users**.</span></span>

1. <span data-ttu-id="62981-125">**[新しいユーザー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-125">Click **New user**.</span></span>

1. <span data-ttu-id="62981-126">ユーザー名が **Isabella Simonsen** という名前のユーザー名を作成します。</span><span class="sxs-lookup"><span data-stu-id="62981-126">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   `Isabella@firstupconsultants<XXXXXXX>.onmicrosoft.com`

   <span data-ttu-id="62981-127">@ の後のドメインを、前述の「*ディレクトリを作成する*」セクションで作成したドメインと一致させます。</span><span class="sxs-lookup"><span data-stu-id="62981-127">Match the domain after the @ with the domain you created in the *Create a directory* section above.</span></span>

1. <span data-ttu-id="62981-128">このユーザーの **[パスワードを表示]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="62981-128">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="62981-129">後でテストするときに使用できるように、パスワードをメモします。</span><span class="sxs-lookup"><span data-stu-id="62981-129">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="62981-130">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-130">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="62981-131">パイロット グループを作成する</span><span class="sxs-lookup"><span data-stu-id="62981-131">Create a pilot group</span></span>

<span data-ttu-id="62981-132">作成するポリシーをユーザーのグループに割り当てますが、このポリシーを割り当てるグループを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62981-132">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="62981-133">次の手順に従って、パイロット デプロイのためのセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="62981-133">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="62981-134">**[Azure Active Directory]** > **[グループ]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="62981-134">Browse to **Azure Active Directory** > **Groups**.</span></span>

1. <span data-ttu-id="62981-135">**[新しいグループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-135">Click **New group**.</span></span>

1. <span data-ttu-id="62981-136">グループの種類: **セキュリティ**。</span><span class="sxs-lookup"><span data-stu-id="62981-136">Group type **Security**.</span></span>

1. <span data-ttu-id="62981-137">グループ名: **CA-MFA-AzurePortal**。</span><span class="sxs-lookup"><span data-stu-id="62981-137">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="62981-138">メンバーシップの種類: **割り当て済み**。</span><span class="sxs-lookup"><span data-stu-id="62981-138">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="62981-139">前の手順で作成したユーザーを選択して、**[選択]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="62981-139">Select the user that we created in the previous step and choose **Select**.</span></span>

1. <span data-ttu-id="62981-140">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="62981-140">Click **Create**.</span></span>

<span data-ttu-id="62981-141">このユニットでは、Azure portal で試用版ライセンスのディレクトリ、テスト ユーザー、およびパイロット グループを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="62981-141">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>