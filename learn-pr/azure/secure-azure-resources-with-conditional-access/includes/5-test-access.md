<span data-ttu-id="88703-101">ここまでの演習で、ディレクトリを作成し、ユーザーとグループを作成した後、Azure portal へのアクセス時に Azure Multi-Factor Authentication を要求する条件付きアクセス規則を作成しました。</span><span class="sxs-lookup"><span data-stu-id="88703-101">In the previous exercises, we created a directory, created a user and group, and then created a conditional access rule that requires Azure Multi-Factor Authentication when accessing the Azure portal.</span></span> <span data-ttu-id="88703-102">次に、リソースにアクセスできるかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="88703-102">Now, we'll test if we can access our resources.</span></span>

## <a name="test-access-to-resources"></a><span data-ttu-id="88703-103">リソースへのアクセスをテストする</span><span class="sxs-lookup"><span data-stu-id="88703-103">Test access to resources</span></span>

<span data-ttu-id="88703-104">サインインしたユーザーが、MyApps ポータルを使用してすべての SaaS アプリケーションにアクセスすることをあなたは知っています。</span><span class="sxs-lookup"><span data-stu-id="88703-104">You know that your users will sign in and access all their SaaS applications using the MyApps portal, so this is what we'll test.</span></span>

1. <span data-ttu-id="88703-105">InPrivate ブラウザー ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="88703-105">Open an InPrivate browser window.</span></span>

1. <span data-ttu-id="88703-106">https://myapps.microsoft.com を参照します。</span><span class="sxs-lookup"><span data-stu-id="88703-106">Browse to https://myapps.microsoft.com.</span></span>

1. <span data-ttu-id="88703-107">ユニット 3 で作成したユーザーとしてサインインします。</span><span class="sxs-lookup"><span data-stu-id="88703-107">Sign in as the user that we created in Unit 3.</span></span>

   * <span data-ttu-id="88703-108">多要素認証を必要とせず、ポータルにサインインできたことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="88703-108">Notice that you're signed in to the portal without requiring Multi-Factor Authentication.</span></span>

1. <span data-ttu-id="88703-109">同じブラウザー ウィンドウで https://portal.azure.com を参照します。</span><span class="sxs-lookup"><span data-stu-id="88703-109">In the same browser window, browse to https://portal.azure.com.</span></span>

   * <span data-ttu-id="88703-110">今度は、アカウントの安全を確保するために追加の情報入力が要求されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88703-110">Notice that you're now required to provide more information to keep your account secure.</span></span> <span data-ttu-id="88703-111">この要求は、作成した条件付きアクセス ポリシーによって、Azure Multi-Factor Authentication が適用されたことによるものです。</span><span class="sxs-lookup"><span data-stu-id="88703-111">This interrupt is Azure Multi-Factor Authentication kicking in because of the conditional access policy we created.</span></span> <span data-ttu-id="88703-112">この時点で作業を終了し、ブラウザー ウィンドウを閉じることができます。</span><span class="sxs-lookup"><span data-stu-id="88703-112">You can stop at this point and close the browser window.</span></span>