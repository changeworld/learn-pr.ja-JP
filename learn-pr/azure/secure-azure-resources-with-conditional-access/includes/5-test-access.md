<span data-ttu-id="3a192-101">前の演習では、Azure AD の基本を理解しました。</span><span class="sxs-lookup"><span data-stu-id="3a192-101">In the previous exercises, we gathered a basic understanding of Azure AD.</span></span> <span data-ttu-id="3a192-102">ディレクトリを作成し、ユーザーとグループを作成した後、Azure portal へのアクセス時に Azure Multi-Factor Authentication を要求する条件付きアクセス規則を作成しました。</span><span class="sxs-lookup"><span data-stu-id="3a192-102">We created a directory, created a user and group, and then created a conditional access rule that requires Azure Multi-Factor Authentication when accessing the Azure portal.</span></span>

<span data-ttu-id="3a192-103">次に、リソースにアクセスできるかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="3a192-103">Now, we'll test if we can access our resources.</span></span>

## <a name="test-access-to-resources"></a><span data-ttu-id="3a192-104">リソースへのアクセスをテストする</span><span class="sxs-lookup"><span data-stu-id="3a192-104">Test access to resources</span></span>

<span data-ttu-id="3a192-105">サインインしたユーザーが、MyApps ポータルを使用してすべての SaaS アプリケーションにアクセスすることをあなたは知っています。</span><span class="sxs-lookup"><span data-stu-id="3a192-105">You know that your users will sign in and access all their SaaS applications using the MyApps portal, so this is what we'll test.</span></span>

1. <span data-ttu-id="3a192-106">InPrivate ブラウザー ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="3a192-106">Open an InPrivate browser window.</span></span>

1. <span data-ttu-id="3a192-107">https://myapps.microsoft.com を参照します。</span><span class="sxs-lookup"><span data-stu-id="3a192-107">Browse to https://myapps.microsoft.com.</span></span>

1. <span data-ttu-id="3a192-108">ユニット 3 で作成したユーザーとしてサインインします。</span><span class="sxs-lookup"><span data-stu-id="3a192-108">Sign in as the user that we created in Unit 3.</span></span>

   * <span data-ttu-id="3a192-109">多要素認証を必要とせず、ポータルにサインインできたことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="3a192-109">Notice that you're signed in to the portal without requiring Multi-Factor Authentication.</span></span>

1. <span data-ttu-id="3a192-110">同じブラウザー ウィンドウで https://portal.azure.com を参照します。</span><span class="sxs-lookup"><span data-stu-id="3a192-110">In the same browser window, browse to https://portal.azure.com.</span></span>

   * <span data-ttu-id="3a192-111">今度は、アカウントの安全を確保するために追加の情報入力が要求されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3a192-111">Notice that you're now required to provide more information to keep your account secure.</span></span> <span data-ttu-id="3a192-112">この要求は、作成した条件付きアクセス ポリシーによって、Azure Multi-Factor Authentication が適用されたことによるものです。</span><span class="sxs-lookup"><span data-stu-id="3a192-112">This interrupt is Azure Multi-Factor Authentication kicking in because of the conditional access policy we created.</span></span> <span data-ttu-id="3a192-113">この時点で作業を終了し、ブラウザー ウィンドウを閉じることができます。</span><span class="sxs-lookup"><span data-stu-id="3a192-113">You can stop at this point and close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="3a192-114">まとめ</span><span class="sxs-lookup"><span data-stu-id="3a192-114">Summary</span></span>

<span data-ttu-id="3a192-115">このユニットでは、Azure PowerShell を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="3a192-115">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="3a192-116">次のユニットでは、カスタム ロールを作成して独自のアクセス許可を定義する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3a192-116">In the next unit, you look at how to create a custom role and define your own permissions.</span></span>