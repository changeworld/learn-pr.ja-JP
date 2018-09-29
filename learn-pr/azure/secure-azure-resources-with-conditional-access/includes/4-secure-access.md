<span data-ttu-id="08795-101">前の演習では、試用版ライセンスを有効にし、ソリューションをテストするためにディレクトリの作成、ユーザーの作成、グループの作成を行いました。</span><span class="sxs-lookup"><span data-stu-id="08795-101">In the previous exercise, we enabled trial licenses, created a directory, created a user, and created a group to test our solution.</span></span> <span data-ttu-id="08795-102">このユニットでは、Azure portal に対して Azure Multi-Factor Authentication を必須にする条件付きアクセス規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="08795-102">In this unit, we will create our conditional access rule to require Azure Multi-Factor Authentication for the Azure portal.</span></span>

## <a name="enable-conditional-access-based-multi-factor-authentication"></a><span data-ttu-id="08795-103">条件付きアクセスベースの多要素認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="08795-103">Enable conditional access-based Multi-Factor Authentication</span></span>

<span data-ttu-id="08795-104">条件付きアクセスを使用すると、管理者は、何かを発生させたい場合と発生させたくない場合を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="08795-104">Conditional access allows administrators to configure when they do or do not want something to happen.</span></span> <span data-ttu-id="08795-105">複数の規則を同時に使用して、リソースへのアクセスを許可または拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="08795-105">They can use multiple rules in parallel to grant or deny access to resources.</span></span> <span data-ttu-id="08795-106">作成する必要がある規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="08795-106">Here's the rule that we need to create:</span></span>

<span data-ttu-id="08795-107">**Azure portal へのアクセス時 - 多要素認証を要求**</span><span class="sxs-lookup"><span data-stu-id="08795-107">**When accessing the Azure portal - Require multi-factor authentication**</span></span>

<span data-ttu-id="08795-108">この後に示す手順では、Azure portal へのアクセス時に多要素認証の実行をユーザーに要求する、条件付きアクセス規則の作成プロセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="08795-108">The steps that follow will walk you through the process to create a conditional access rule to require users to perform multi-factor authentication when they access the Azure portal.</span></span>

1. <span data-ttu-id="08795-109">**[Azure Active Directory]** > **[条件付きアクセス]** の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="08795-109">Browse to **Azure Active Directory** > **Conditional access**.</span></span>

1. <span data-ttu-id="08795-110">**[新しいポリシー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08795-110">Click **New policy**.</span></span>

1. <span data-ttu-id="08795-111">ポリシーに「**Azure portal に MFA を要求**」という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="08795-111">Name the policy **Require MFA for Azure portal**.</span></span>

1. <span data-ttu-id="08795-112">**[割り当て]** > **[ユーザーとグループ]** で、**[ユーザーとグループ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="08795-112">Under **Assignments** > **Users and groups**, select **Users and groups**.</span></span> <span data-ttu-id="08795-113">作成したグループ **CA-MFA-AzurePortal** を選択します。</span><span class="sxs-lookup"><span data-stu-id="08795-113">Select the group that we created **CA-MFA-AzurePortal**.</span></span> <span data-ttu-id="08795-114">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08795-114">and click **Done**.</span></span>

1. <span data-ttu-id="08795-115">**[クラウド アプリ]** > **[アプリを選択]** で、**[Microsoft Azure Management]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="08795-115">Under **Cloud apps** > **Select apps**, select **Microsoft Azure Management**.</span></span>

1. <span data-ttu-id="08795-116">**[アクセス制御]** > **[許可]** で、**[多要素認証を要求する]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="08795-116">Under **Access controls** > **Grant**, select **Require multi-factor authentication**.</span></span>

1. <span data-ttu-id="08795-117">**[ポリシーの有効化]** を **[オン]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="08795-117">Set **Enable policy** to **On**.</span></span>

1. <span data-ttu-id="08795-118">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="08795-118">Click **Create**.</span></span>

<span data-ttu-id="08795-119">このユニットでは、条件付きアクセス規則を作成する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="08795-119">In this unit, you learned how to create a conditional access rule.</span></span> <span data-ttu-id="08795-120">その規則では、Azure portal へのアクセス時に Multi-Factor Authentication を要求します。</span><span class="sxs-lookup"><span data-stu-id="08795-120">The rule requires Multi-Factor Authentication when accessing the Azure portal.</span></span>