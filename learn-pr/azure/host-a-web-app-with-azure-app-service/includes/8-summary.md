<span data-ttu-id="03544-101">Azure App Service にフル機能の Web アプリケーションを問題なく作成してデプロイしました。</span><span class="sxs-lookup"><span data-stu-id="03544-101">You've successfully created and deployed a full-featured web application to Azure App Service.</span></span>

<span data-ttu-id="03544-102">App Service を使用すると、従来のホスティング オプションと比べて、Web アプリの管理と制御が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="03544-102">App Service simplifies managing and controlling your web app in comparison to traditional hosting options.</span></span> <span data-ttu-id="03544-103">App Service により、Web アプリの実行と管理に要する時間と労力が減り、自動スケーリングや Git 統合などの高度なクラウド機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="03544-103">App Service can help you reduce the time and effort spent running and managing your web app, and provide advanced cloud features such as auto scaling and Git integration.</span></span>

## <a name="clean-up"></a><span data-ttu-id="03544-104">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="03544-104">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="03544-105">このアプリケーションの作業が完了したら、以下の手順に従って、このチュートリアルで作成したすべてのリソースを削除できます。</span><span class="sxs-lookup"><span data-stu-id="03544-105">When you are done working with this application, you can follow the steps below to delete all resources created during the tutorial.</span></span>

<span data-ttu-id="03544-106">ご存知のように、リソース グループは、作成されて Web アプリと結び付けられたすべてのリソースを関連付けるのに便利です。</span><span class="sxs-lookup"><span data-stu-id="03544-106">As you know, a resource group is useful for associating all the resources that are created and related to your web app.</span></span> <span data-ttu-id="03544-107">そのため、Azure でのクリーンアップとはリソース グループを削除することであり、そのグループの下に作成されたすべてのリソースが表示されなくなります。</span><span class="sxs-lookup"><span data-stu-id="03544-107">So, cleaning up after yourself on Azure is a matter of deleting the resource group, and hence, all the resources created under that group disappear.</span></span>

<span data-ttu-id="03544-108">Azure portal を使用してリソース グループを削除する方法を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="03544-108">Let's explore together how you can delete a resource group using the Azure portal:</span></span>

1. <span data-ttu-id="03544-109">[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="03544-109">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="03544-110">ダッシュボードの左側で **[リソース グループ]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03544-110">Click the **Resource groups** menu item on the left-side of the Dashboard.</span></span> <span data-ttu-id="03544-111">**[リソース グループ]** ページに、Azure portal で作成されているすべてのリソース グループが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="03544-111">The **Resource groups** page lists all the resource groups that you have created in the Azure portal.</span></span>

1. <span data-ttu-id="03544-112">ユニット 2 で作成したリソース グループの名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="03544-112">Select the resource group name that you created in Unit #2.</span></span> <span data-ttu-id="03544-113">ポータルの表示がアプリ ページに変わります。</span><span class="sxs-lookup"><span data-stu-id="03544-113">The portal navigates you to the app page.</span></span>

1. <span data-ttu-id="03544-114">Azure portal の表示が **[リソース グループ]** ページに変わります。</span><span class="sxs-lookup"><span data-stu-id="03544-114">The Azure portal navigates you to the **Resource group** page.</span></span> <span data-ttu-id="03544-115">そこでは、このモジュールの間にこのリソース グループの下に作成したすべてのリソースの一覧を見ることができます。</span><span class="sxs-lookup"><span data-stu-id="03544-115">There, you can see a list of all the resources that you've created during this module under this resource group.</span></span>

1. <span data-ttu-id="03544-116">ページの上部にある **[リソース グループの削除]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="03544-116">Click the **Delete resource group** link at the top of the page.</span></span>

1. <span data-ttu-id="03544-117">リソース グループの名前を入力することでその削除を確認するよう求められます。</span><span class="sxs-lookup"><span data-stu-id="03544-117">Azure verifies that you really want to delete this resource group by asking you to type the name of it.</span></span> <span data-ttu-id="03544-118">先に進むには、リソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="03544-118">To proceed, type the name of the resource group.</span></span>

1. <span data-ttu-id="03544-119">確認ウィンドウの下部にある **[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03544-119">Click the **Delete** button at the bottom of the confirmation window.</span></span>

1. <span data-ttu-id="03544-120">リソース グループのすべてのリソースが削除されるのに数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="03544-120">Azure takes a few seconds to delete all the resources of the resource group.</span></span>
