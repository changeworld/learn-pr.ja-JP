<span data-ttu-id="67b7d-101">Azure Multi-Factor Authentication は、すべてのユーザーにすべての認証を有効にするため、IT 部門によって従来からデプロイされています。</span><span class="sxs-lookup"><span data-stu-id="67b7d-101">Azure Multi-Factor Authentication has traditionally been deployed like a hammer by IT departments, enabled for every user for every authentication.</span></span> <span data-ttu-id="67b7d-102">この機能と条件付きアクセスを統合すると、Azure Multi-Factor Authentication は非常に強力なツールとなり、それを必要とするアプリと状況を指定できるようになります。</span><span class="sxs-lookup"><span data-stu-id="67b7d-102">When the feature is integrated with conditional access, Azure Multi-Factor Authentication becomes a razor-sharp scalpel that allows you to specify which apps and circumstances require it.</span></span>

<span data-ttu-id="67b7d-103">あなたは、回路と電気設計を専門とするエンジニアリング会社である First Up Consultants に勤務しているとします。</span><span class="sxs-lookup"><span data-stu-id="67b7d-103">Imagine that you work for First Up Consultants, an engineering firm that specializes in circuit and electrical design.</span></span> <span data-ttu-id="67b7d-104">会社では、オンプレミス ワークロードを Azure に移行しています。</span><span class="sxs-lookup"><span data-stu-id="67b7d-104">They're moving their on-premises workloads to Azure.</span></span> <span data-ttu-id="67b7d-105">IT 部門でのあなたの上司が、会社の資産を保護する責任を負っています。</span><span class="sxs-lookup"><span data-stu-id="67b7d-105">Your manager in the IT department is responsible for keeping the company’s assets secure.</span></span> <span data-ttu-id="67b7d-106">あなたは管理者から、Azure portal に管理者がアクセスするときは常に、多要素認証を実行するように依頼されました。</span><span class="sxs-lookup"><span data-stu-id="67b7d-106">The admin wants you to make sure that any time an administrator accesses the Azure portal, they've performed multi-factor authentication.</span></span> <span data-ttu-id="67b7d-107">あなたは、管理者からこの機能を有効にしてそれをテストする仕事を課されました。</span><span class="sxs-lookup"><span data-stu-id="67b7d-107">The admin has tasked you with enabling this functionality and testing it.</span></span> <span data-ttu-id="67b7d-108">あなたは、Azure AD、条件付きアクセス、Azure Multi-Factor Authentication を使用します。</span><span class="sxs-lookup"><span data-stu-id="67b7d-108">You'll use Azure AD, conditional access, and Azure Multi-Factor Authentication.</span></span>

<span data-ttu-id="67b7d-109">このモジュールでは、条件付きアクセスと Azure Multi-Factor Authentication を使用して、Azure portal へのアクセスをセキュリティで保護する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="67b7d-109">In this module, you will learn how to use conditional access along with Azure Multi-Factor Authentication to secure access to the Azure portal.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="67b7d-110">学習の目的</span><span class="sxs-lookup"><span data-stu-id="67b7d-110">Learning objectives</span></span>

<span data-ttu-id="67b7d-111">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="67b7d-111">In this module, you will:</span></span>
- <span data-ttu-id="67b7d-112">テスト ユーザーと使用グループを作成する</span><span class="sxs-lookup"><span data-stu-id="67b7d-112">Create a test user and a use group</span></span>
- <span data-ttu-id="67b7d-113">Multi-Factor Authentication を必要とする条件付きアクセス ポリシーを有効にする</span><span class="sxs-lookup"><span data-stu-id="67b7d-113">Enable a conditional access policy that requires Multi-Factor Authentication</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67b7d-114">前提条件</span><span class="sxs-lookup"><span data-stu-id="67b7d-114">Prerequisites</span></span>

- <span data-ttu-id="67b7d-115">リソース グループやサブスクリプションなど、Azure の基本的な概念に関する知識</span><span class="sxs-lookup"><span data-stu-id="67b7d-115">Knowledge of basic Azure concepts, such as resource groups and subscriptions</span></span>
- <span data-ttu-id="67b7d-116">Azure サブスクリプションへのアクセス</span><span class="sxs-lookup"><span data-stu-id="67b7d-116">Access to an Azure subscription</span></span>
- <span data-ttu-id="67b7d-117">ディレクトリを作成するためのアクセス許可</span><span class="sxs-lookup"><span data-stu-id="67b7d-117">Permissions to create a directory</span></span>