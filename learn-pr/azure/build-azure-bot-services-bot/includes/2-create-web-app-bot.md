<span data-ttu-id="28162-101">ボットを作成する最初のステップは、ボットが Azure でホストされる場所を指定することです。</span><span class="sxs-lookup"><span data-stu-id="28162-101">The first step in creating a bot is to provide a location for the bot to be hosted in Azure.</span></span> <span data-ttu-id="28162-102">Azure App Service の [Web Apps](https://azure.microsoft.com/services/app-service/web/) 機能はボット アプリケーションをホストするのに最適であり、Azure Bot Service はそれを自動的にプロビジョニングするように設計されています。</span><span class="sxs-lookup"><span data-stu-id="28162-102">The [Web Apps](https://azure.microsoft.com/services/app-service/web/) feature of Azure App Service is perfect for hosting bot applications, and the Azure Bot Service is designed to provision them for you.</span></span> <span data-ttu-id="28162-103">この演習では、Azure portal を使用して Azure Web アプリ ボットをプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="28162-103">In this unit, you will use the Azure portal to provision an Azure web app bot.</span></span>

1. <span data-ttu-id="28162-104">ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) を開きます。</span><span class="sxs-lookup"><span data-stu-id="28162-104">Open the [Azure portal](https://portal.azure.com/?azure-portal=true) in your browser.</span></span> <span data-ttu-id="28162-105">サインインを求められたら、Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="28162-105">If you are asked to sign in, do so using your Microsoft account.</span></span>

1. <span data-ttu-id="28162-106">**[+ リソースの作成]**、**[AI + Machine Learning]**、**[Web App Bot]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="28162-106">Click **+ Create a resource**, followed by **AI + Machine Learning**, and then **Web App Bot**.</span></span>
 
    ![新しい Azure Web アプリ ボットの作成](../media-draft/2-new-bot-service.png)

1. <span data-ttu-id="28162-108">**[アプリ名]** ボックスに "qa-factbot" などの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="28162-108">Enter a name, such as "qa-factbot", into the **App name** box.</span></span> <span data-ttu-id="28162-109">"*この名前は Azure 内で一意である必要があるため、名前の横に緑色のチェック マークが表示されることを確認してください。*"</span><span class="sxs-lookup"><span data-stu-id="28162-109">*This name must be unique within Azure, so make sure a green check mark appears next to it.*</span></span> <span data-ttu-id="28162-110">**[リソース グループ]** の **[新規作成]** を選択し、リソース グループ名として「factbot-rg」 と入力します。</span><span class="sxs-lookup"><span data-stu-id="28162-110">Select **Create new** under **Resource group** and enter the resource group name "factbot-rg."</span></span> <span data-ttu-id="28162-111">最寄りの場所を選択し、無料の **F0** 価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="28162-111">Select the location nearest you and select the free **F0** pricing tier.</span></span> <span data-ttu-id="28162-112">次に、**[Bot template]\(ボット テンプレート\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="28162-112">Then, click **Bot template**.</span></span>

    ![Azure Web アプリ ボットの構成](../media-draft/2-portal-start-bot-creation.png)

1. <span data-ttu-id="28162-114">SDK 言語として **[Node.js]** を選択し、テンプレートの種類として **[Question and Answer]\(質問と回答\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="28162-114">Select **Node.js** as the SDK language and **Question and Answer** as the template type.</span></span> <span data-ttu-id="28162-115">次に、ブレードの下部にある **[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="28162-115">Then, click **Select** at the bottom of the blade.</span></span>   
  
    ![言語とテンプレートの選択](../media-draft/2-portal-select-template.png)

1. <span data-ttu-id="28162-117">**[App Service プラン/場所]**、**[新規作成]** の順にクリックし、手順 3 で選択したのと同じリージョンに "qa-factbot-service-plan" のような名前で App Service プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="28162-117">Now, click **App service plan/Location**, followed by **Create New**, and then create an App Service plan named "qa-factbot-service-plan" or something similar in the same region that you selected in Step 3.</span></span> <span data-ttu-id="28162-118">終わったら、[Web App Bot] ブレードの下部にある **[作成]** をクリックして展開を開始します。</span><span class="sxs-lookup"><span data-stu-id="28162-118">Once that's done, click **Create** at the bottom of the "Web App Bot" blade to start the deployment.</span></span> 

1. <span data-ttu-id="28162-119">ポータルの左側のリボンで **[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="28162-119">Click **Resource groups** in the ribbon on the left side of the portal.</span></span> <span data-ttu-id="28162-120">**factbot-rg** をクリックして、Azure Web アプリ ボット用に作成したリソース グループを開きます。</span><span class="sxs-lookup"><span data-stu-id="28162-120">Then, click **factbot-rg** to open the resource group created for the Azure web app bot.</span></span> <span data-ttu-id="28162-121">ブレードの上部の "デプロイ中" が、Azure Web アプリ ボットが正常にデプロイされたことを示す "成功" に変わるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="28162-121">Wait until "Deploying" changes to "Succeeded" at the top of the blade, which indicates that the Azure web app bot was successfully deployed.</span></span> <span data-ttu-id="28162-122">通常、デプロイに必要な時間は 2 分以内です。</span><span class="sxs-lookup"><span data-stu-id="28162-122">Deployment generally requires two minutes or less.</span></span> <span data-ttu-id="28162-123">ブレードの上部にある **[更新]** をときどきクリックして、デプロイの状態を更新します。</span><span class="sxs-lookup"><span data-stu-id="28162-123">Periodically click **Refresh** at the top of the blade to refresh the deployment status.</span></span>

    ![デプロイに成功](../media-draft/2-deployment-succeeded.png)
  
<span data-ttu-id="28162-125">Azure Web アプリ ボットをデプロイするときは、見えないところで多くのことが行われています。</span><span class="sxs-lookup"><span data-stu-id="28162-125">Behind the scenes, a lot happened when the Azure web app bot was deployed.</span></span> <span data-ttu-id="28162-126">ボットが作成されて登録され、それをホストする [Azure Web App](https://azure.microsoft.com/services/app-service/web/) が作成されて、[Microsoft QnA Maker](https://www.qnamaker.ai/) で動作するようにボットが構成されました。</span><span class="sxs-lookup"><span data-stu-id="28162-126">A bot was created and registered, an [Azure web app](https://azure.microsoft.com/services/app-service/web/) was created to host it, and the bot was configured to work with [Microsoft QnA Maker](https://www.qnamaker.ai/).</span></span> <span data-ttu-id="28162-127">次の手順では、QnA Maker を使用して、ボットにインテリジェンスを取り入れるための質問と回答のナレッジ ベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="28162-127">The next step is to use QnA Maker to create a knowledge base of questions and answers to infuse the bot with intelligence.</span></span>