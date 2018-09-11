### <a name="exercise-2-create-a-knowledge-base-with-microsoft-qna-maker"></a><span data-ttu-id="3095d-101">演習 2: Microsoft QnA Maker でナレッジ ベースを作成する</span><span class="sxs-lookup"><span data-stu-id="3095d-101">Exercise 2: Create a knowledge base with Microsoft QnA Maker</span></span>

<span data-ttu-id="3095d-102">[Azure Cognitive Services](https://www.microsoft.com/cognitive-services/) の一部である [Microsoft QnA Maker](https://www.qnamaker.ai/) は、AI と機械学習によるインテリジェントなアプリをビルドするためのサービスと API のスイートです。</span><span class="sxs-lookup"><span data-stu-id="3095d-102">[Microsoft QnA Maker](https://www.qnamaker.ai/) is part of [Azure Cognitive Services](https://www.microsoft.com/cognitive-services/), which is a suite of services and APIs for building intelligent apps backed by AI and machine learning.</span></span> <span data-ttu-id="3095d-103">ボットがユーザーからのあらゆる質問を予測して応答を提供するためのコードを書くのではなく、ボットを QnA Maker で作成された質問と回答のナレッジ ベースに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="3095d-103">Rather than code a bot to anticipate every question a user might ask and provide a response, you can connect it to a knowledge base of questions and answers created with QnA Maker.</span></span> <span data-ttu-id="3095d-104">一般的な使用シナリオは、「Windows プロダクト キーはどこにありますか」や「Visual Studio Code はどこでダウンロードできますか」といったドメイン固有の質問にボットが回答できるように、よく寄せられる質問からナレッジ ベースを作成することです。</span><span class="sxs-lookup"><span data-stu-id="3095d-104">A common usage scenario is to create a knowledge base from a FAQ so the bot can answer domain-specific questions such as "How do I find my Windows product key" or "Where can I download Visual Studio Code?"</span></span>

<span data-ttu-id="3095d-105">この演習では、QnA Maker を使用して、"スーパー ボウルで最も優勝回数の多い NFL チームはどこですか" や "世界最大の都市はどこですか?" といった質問を含むナレッジ ベースを作成します</span><span class="sxs-lookup"><span data-stu-id="3095d-105">In this exercise, you will use QnA Maker to create a knowledge base containing questions such as "What NFL teams have won the most Super Bowls" and "What is the largest city in the world?"</span></span> <span data-ttu-id="3095d-106">次に、HTTPS エンドポイントを経由してアクセスできるように、ナレッジ ベースを Azure Web アプリにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3095d-106">Then you will deploy the knowledge base in an Azure Web app so it can be accessed via an HTTPS endpoint.</span></span>

1. <span data-ttu-id="3095d-107">ブラウザーで [Microsoft QnA Maker ポータル](https://www.qnamaker.ai/)を開き、自分の Microsoft アカウントでサインインします (まだサインインしていない場合)。</span><span class="sxs-lookup"><span data-stu-id="3095d-107">Open the [Microsoft QnA Maker portal](https://www.qnamaker.ai/) in your browser and sign in with your Microsoft account if you aren't already signed in.</span></span> <span data-ttu-id="3095d-108">次に、ページ上部のメニュー バーで **[ナレッジ ベースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3095d-108">Then click **Create a knowledge base** in the menu bar at the top of the page.</span></span>
 
    ![ナレッジ ベースを作成する](../images/qna-new-kb.png)

    <span data-ttu-id="3095d-110">"ナレッジ ベースを作成する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-110">_Creating a knowledge base_</span></span>

1. <span data-ttu-id="3095d-111">**[Create a QnA service]\(QnA サービスの作成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3095d-111">Click **Create a QnA service**.</span></span>

    ![QnA サービスを作成する](../images/create-kb-1.png)

    <span data-ttu-id="3095d-113">"QnA サービスを作成する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-113">_Creating a QnA service_</span></span>

1. <span data-ttu-id="3095d-114">**[名前]** ボックスに "qa-factbot-kb" などの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="3095d-114">Enter a name such as "qa-factbot-kb" into the **Name** box.</span></span> <span data-ttu-id="3095d-115">この名前は、Azure 内で一意である必要があるため、名前の横、*および*ブレードの下の方にある **[アプリ名]** ボックスに緑色のチェック マークが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3095d-115">This name must be unique within Azure, so make sure a green check mark appears next to it *and* in the **App name** box further down the blade.</span></span> <span data-ttu-id="3095d-116">**[リソース グループ]** の下で **[既存のものを使用]** を選択し、[演習 1](#Exercise1) で Azure Web アプリ ボットをデプロイしたときに作成した "factbot-rg" リソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="3095d-116">Select **Use existing** under **Resource group** and select the "factbot-rg" resource group that you created when you deployed the Azure Web App Bot in [Exercise 1](#Exercise1).</span></span> <span data-ttu-id="3095d-117">価格レベルとして **[F0]** と **[F]** を選択します </span><span class="sxs-lookup"><span data-stu-id="3095d-117">Select **F0** and **F** as the pricing tiers.</span></span> <span data-ttu-id="3095d-118">(どちらも無料で、ボットを試すには最適なレベルです)。両方の場所のドロップダウンで最も近い場所を選択してから、ブレードの下部にある **[作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3095d-118">(Both are free tiers that are ideal for experimenting with bots.) Select the location nearest you in both location drop-downs, and then click the **Create** button at the bottom of the blade.</span></span>

    ![QnA サービスを作成する](../images/new-qna-maker-service.png)

    <span data-ttu-id="3095d-120">"QnA サービスを作成する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-120">_Creating a QnA service_</span></span>

1. <span data-ttu-id="3095d-121">ポータルの左側にあるリボンで **[リソース グループ]** をクリックして、"factbot-rg" リソース グループを開きます。</span><span class="sxs-lookup"><span data-stu-id="3095d-121">Click **Resource groups** in the ribbon on the left side of the portal and open the "factbot-rg" resource group.</span></span> <span data-ttu-id="3095d-122">ブレードの上部の "デプロイ中" が、QnA のサービスとそれに関連付けられているリソースが正常にデプロイされたことを示す "成功" に変わるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="3095d-122">Wait until "Deploying" changes to "Succeeded" at the top of the blade indicating that the QnA service and the resources associated with it were successfully deployed.</span></span> <span data-ttu-id="3095d-123">デプロイの状態を更新するには、もう一度ブレードの上部の **[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3095d-123">Once more, you can click **Refresh** at the top of the blade to refresh the deployment status.</span></span>

    ![デプロイに成功](../images/resource-group-master-2.png)

    <span data-ttu-id="3095d-125">"デプロイに成功"__</span><span class="sxs-lookup"><span data-stu-id="3095d-125">_Successful deployment_</span></span>

1. <span data-ttu-id="3095d-126">QnA Maker ポータルで [[ナレッジ ベースの作成]](https://www.qnamaker.ai/Create) ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="3095d-126">Return to the [Create a knowledge base](https://www.qnamaker.ai/Create) page in the QnA Maker portal.</span></span> <span data-ttu-id="3095d-127">**Azure QnA サービス**の下で、手順 3 で指定した名前の QnA サービスを選択します。</span><span class="sxs-lookup"><span data-stu-id="3095d-127">Under **Azure QnA service**, select the QnA service whose name you specified in Step 3.</span></span> <span data-ttu-id="3095d-128">次に、ナレッジ ベースに "Factbot ナレッジ ベース" などの名前を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="3095d-128">Then assign the knowledge base a name such as "Factbot Knowledge Base."</span></span>

    ![ナレッジ ベースに名前を付ける](../images/create-kb-2-3.png)

    <span data-ttu-id="3095d-130">"ナレッジ ベースに名前を付ける"__</span><span class="sxs-lookup"><span data-stu-id="3095d-130">_Naming the knowledge base_</span></span>

1. <span data-ttu-id="3095d-131">QnA Maker のナレッジ ベースに手動で質問と回答を入力することも、オンラインのよく寄せられる質問またはローカル ファイルから質問と回答をインポートすることもできます。</span><span class="sxs-lookup"><span data-stu-id="3095d-131">You can enter questions and answers into a QnA Maker knowledge base manually, or you can import them from online FAQs or local files.</span></span> <span data-ttu-id="3095d-132">サポートされている形式には、タブ区切りテキスト ファイル、Microsoft Word 文書、Excel スプレッドシート、PDF ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3095d-132">Supported formats include tab-delimited text files, Microsoft Word documents, Excel spreadsheets, and PDF files.</span></span>

    <span data-ttu-id="3095d-133">試してみるには、[ここをクリック](https://topcs.blob.core.windows.net/public/bots-resources.zip)して、**Factbot.tsv** という名前のテキスト ファイルを含む ZIP ファイルをダウンロードし、ファイルをご使用のコンピューターにコピーします。</span><span class="sxs-lookup"><span data-stu-id="3095d-133">To demonstrate, [click here](https://topcs.blob.core.windows.net/public/bots-resources.zip) to download a zip file containing a text file named **Factbot.tsv**, and copy the file to your computer.</span></span> <span data-ttu-id="3095d-134">次に、QnA Maker ポータルで下にスクロールして、**[+ ファイルの追加]** をクリックして、**Factbot.tsv** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3095d-134">Then scroll down in the QnA Maker portal, click **+ Add file**, and select **Factbot.tsv**.</span></span> <span data-ttu-id="3095d-135">このファイルには、20 の質問と回答がタブ区切り形式で含まれています。</span><span class="sxs-lookup"><span data-stu-id="3095d-135">This file contains 20 questions and answers in tab-delimited format.</span></span>

    ![ナレッジ ベースを追加する](../images/create-kb-4.png)

    <span data-ttu-id="3095d-137">"ナレッジ ベースを追加する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-137">_Populating the knowledge base_</span></span>

1. <span data-ttu-id="3095d-138">ページの下部の **[Create your KB]\(KB の作成\)** をクリックして、ナレッジ ベースが作成されるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="3095d-138">Click **Create your KB** at the bottom of the page and wait for the knowledge base to be created.</span></span> <span data-ttu-id="3095d-139">通常は 1 分以内に終わります。</span><span class="sxs-lookup"><span data-stu-id="3095d-139">It should take less than a minute.</span></span>

    ![ナレッジ ベースを作成する](../images/create-kb-5.png)

    <span data-ttu-id="3095d-141">"ナレッジ ベースを作成する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-141">_Creating the knowledge base_</span></span>

1. <span data-ttu-id="3095d-142">**Factbot.tsv** からインポートされた質問と回答がナレッジ ベースに表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3095d-142">Confirm that the questions and answers imported from **Factbot.tsv** appear in the knowledge base.</span></span> <span data-ttu-id="3095d-143">**[Save and train]\(保存してトレーニング\)** をクリックして、トレーニングが完了するのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="3095d-143">Then click **Save and train** and wait for training to complete.</span></span>

    ![ナレッジ ベースをトレーニングする](../images/save-and-train.png)

    <span data-ttu-id="3095d-145">"ナレッジ ベースをトレーニングする"__</span><span class="sxs-lookup"><span data-stu-id="3095d-145">_Training the knowledge base_</span></span>

1. <span data-ttu-id="3095d-146">**[Save and train]\(保存してトレーニング\)** ボタンの右にある **[テスト]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3095d-146">Click the **Test** button to the right of the **Save and train** button.</span></span> <span data-ttu-id="3095d-147">メッセージ ボックスに「こんにちは」と入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3095d-147">Type "Hi" into the message box and press **Enter**.</span></span> <span data-ttu-id="3095d-148">次に示すように、応答が "QnA Factbot へようこそ" となることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3095d-148">Confirm that the response is "Welcome to the QnA Factbot," as shown below.</span></span>

    ![ナレッジ ベースをテストする](../images/test-kb.png)

    <span data-ttu-id="3095d-150">"ナレッジ ベースをテストする"__</span><span class="sxs-lookup"><span data-stu-id="3095d-150">_Testing the knowledge base_</span></span>

1. <span data-ttu-id="3095d-151">メッセージ ボックスに、「最も売れた本は何ですか?」と入力し、</span><span class="sxs-lookup"><span data-stu-id="3095d-151">Type "What book has sold the most copies?"</span></span> <span data-ttu-id="3095d-152">**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3095d-152">into the message box and press **Enter**.</span></span> <span data-ttu-id="3095d-153">応答はどのようになるでしょう。</span><span class="sxs-lookup"><span data-stu-id="3095d-153">What is the response?</span></span>

1. <span data-ttu-id="3095d-154">もう一度 **[テスト]** ボタンをクリックして、[テスト] パネルを折りたたみます。</span><span class="sxs-lookup"><span data-stu-id="3095d-154">Click the **Test** button again to collapse the Test panel.</span></span> <span data-ttu-id="3095d-155">次に、ページ上部のメニューで **[発行]** をクリックして、ページの下部にある **[発行]** ボタンをクリックしてナレッジ ベースを発行します。</span><span class="sxs-lookup"><span data-stu-id="3095d-155">Then click **Publish** in the menu at the top of the page, and click the **Publish** button at the bottom of the page to publish the knowledge base.</span></span> <span data-ttu-id="3095d-156">*発行*すると、ナレッジ ベースが HTTPS エンドポイントで使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="3095d-156">*Publishing* makes the knowledge base available at an HTTPS endpoint.</span></span>

    ![ナレッジ ベースを発行する](../images/publish-kb.png)

    <span data-ttu-id="3095d-158">"ナレッジ ベースを発行する"__</span><span class="sxs-lookup"><span data-stu-id="3095d-158">_Publishing the knowledge base_</span></span> 

<span data-ttu-id="3095d-159">発行プロセスが完了するまで待機し、QnA サービスのデプロイ完了のメッセージが表示されるのを確認します。</span><span class="sxs-lookup"><span data-stu-id="3095d-159">Wait for the publication process to complete and confirm that you are told the QnA service has been deployed.</span></span> <span data-ttu-id="3095d-160">これで、Azure Web アプリで独自のナレッジ ベースがホストされるようになったので、次の手順では、それを使用できるボットをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3095d-160">With the knowledge base now hosted in an Azure Web App of its own, the next step is to deploy a bot that can use it.</span></span>