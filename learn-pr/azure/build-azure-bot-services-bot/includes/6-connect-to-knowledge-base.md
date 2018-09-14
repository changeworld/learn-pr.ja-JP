<span data-ttu-id="0e3e4-101">このユニットでは、前に作成した QnA Maker のナレッジ ベースにボットを接続して、ボットがインテリジェントな会話を実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-101">In this unit, you will connect your bot to the QnA Maker knowledge base you built earlier so the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="0e3e4-102">ナレッジ ベースに接続するには、QnA Maker ポータルからいくつかの情報を取得し、それを Azure portal にコピーし、ボット コードを更新してから Azure にボットを再デプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-102">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="0e3e4-103">[QnA Maker ポータル](https://www.qnamaker.ai/)に戻り、右上隅にある自分の名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-103">Return to the [QnA Maker portal](https://www.qnamaker.ai/) and click your name in the upper-right corner.</span></span> <span data-ttu-id="0e3e4-104">ドロップダウン メニューから **[エンドポイント キーの管理]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-104">Select **Manage endpoint keys** from the menu that drops down.</span></span> <span data-ttu-id="0e3e4-105">**[表示]** をクリックしてプライマリ エンドポイント キーを表示し、**[コピー]** をクリックしてそれをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-105">Click **Show** to show the primary endpoint key, and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="0e3e4-106">次に、それをテキスト ファイルに貼り付けて、すぐに取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-106">Then, paste it into a text file so you can easily retrieve it in a moment.</span></span>

    ![エンドポイント キーをコピーする](../media-draft/6-copy-primary-key.png)

1. <span data-ttu-id="0e3e4-108">ページの上部にあるメニューで **[My knowledge bases]\(マイ ナレッジ ベース\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-108">Click **My knowledge bases** in the menu at the top of the page.</span></span> <span data-ttu-id="0e3e4-109">次に、先ほど作成したナレッジ ベースの **[コードの表示]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-109">Then, click **View Code** for the knowledge base that you created earlier.</span></span>

    ![ナレッジ ベースを開く](../media-draft/6-open-knowledge-base.png)

1. <span data-ttu-id="0e3e4-111">1 行目からナレッジ ベース ID、2 行目からホスト名をコピーします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-111">Copy the knowledge base ID from the first line and the host name from the second line.</span></span> <span data-ttu-id="0e3e4-112">それらもテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-112">Paste them into a text file, as well.</span></span> <span data-ttu-id="0e3e4-113">その後、ダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-113">Then, close the dialog.</span></span> <span data-ttu-id="0e3e4-114">コピーするホスト名に "https://" プレフィックスを**含めないでください**。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-114">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![ナレッジ ベース ID とホスト名をコピーする](../media-draft/6-copy-endpoint-info.png)  

1. <span data-ttu-id="0e3e4-116">Azure portal で Web アプリ ボットに戻ります。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-116">Return to the web app bot in the Azure portal.</span></span> <span data-ttu-id="0e3e4-117">左にあるメニューで **[アプリケーション設定]** をクリックし、"QnAKnowledgebaseId"、"QnAAuthKey"、"QnAEndpointHostName" という名前のアプリケーション設定が見つかるまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-117">Click **Application settings** in the menu on the left and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey," and "QnAEndpointHostName."</span></span> <span data-ttu-id="0e3e4-118">手順 3 で取得したナレッジ ベース ID およびホスト名と、手順 1 で取得したエンドポイント キーをこれらのフィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-118">Paste the knowledge base ID and host name obtained in Step 3 and the endpoint key obtained in Step 1 into these fields.</span></span> <span data-ttu-id="0e3e4-119">その後、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-119">Then, click **Save**.</span></span>

    ![アプリケーション設定を編集する](../media-draft/6-enter-app-settings.png)

1. <span data-ttu-id="0e3e4-121">Visual Studio Code に戻り、**app.js** の内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-121">Return to Visual Studio Code and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="0e3e4-122">その後、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-122">Then, save the file.</span></span>

    ```JavaScript
    var restify = require('restify');
    var builder = require('botbuilder');
    var botbuilder_azure = require("botbuilder-azure");
    var builder_cognitiveservices = require("botbuilder-cognitiveservices");
    
    // Setup Restify Server
    var server = restify.createServer();
    server.listen(process.env.port || process.env.PORT || 3978, function () {
       console.log('%s listening to %s', server.name, server.url); 
    });
      
    // Create chat connector for communicating with the Bot Framework Service
    var connector = new builder.ChatConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword     
    });
    
    // Listen for messages from users 
    server.post('/api/messages', connector.listen());
     
    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);
    
    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId, 
        authKey: process.env.QnAAuthKey,
        endpointHostName: process.env.QnAEndpointHostName
    });
    
    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });
    
    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);
    
    bot.dialog('/',
    [
        function (session) {
            session.replaceDialog('basicQnAMakerDialog');
        }
    ]);
    ```

    <span data-ttu-id="0e3e4-123">30 行目の `QnAMakerDialog` インスタンスを作成するための呼び出しに注目してください。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-123">Note the call to create a `QnAMakerDialog` instance on line 30.</span></span> <span data-ttu-id="0e3e4-124">これにより、Azure Bot Service でビルドされたボットと、Microsoft QnA Maker で構築されたナレッジ ベースを統合するダイアログが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-124">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built Microsoft QnA Maker.</span></span>
 
1. <span data-ttu-id="0e3e4-125">Visual Studio Code のアクティビティ バーで **[ソース管理]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-125">Click the **Source Control** button in the activity bar in Visual Studio Code.</span></span> <span data-ttu-id="0e3e4-126">メッセージ ボックスに「ナレッジ ベースに接続済み」と入力し、チェック マークをクリックして変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-126">Type "Connected to knowledge base" into the message box, and click the check mark to commit your changes.</span></span> <span data-ttu-id="0e3e4-127">次に、省略記号をクリックし、**[ブランチの発行]** コマンドを使用してこれらの変更をリモート リポジトリ (したがって、</span><span class="sxs-lookup"><span data-stu-id="0e3e4-127">Then, click the ellipsis and use the **Publish Branch** command to push these changes to the remote repository (and therefore.</span></span> <span data-ttu-id="0e3e4-128">Azure Web アプリ) にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-128">to the Azure Web App).</span></span>

1. <span data-ttu-id="0e3e4-129">Azure portal で Web アプリ ボットに戻り、左側にある **[Web チャットでのテスト]** をクリックしてテスト コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-129">Return to the web app bot in the Azure portal and click **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="0e3e4-130">チャット ウィンドウの下部にあるボックスに、「世界で最も普及しているソフトウェア プログラミング言語は何ですか?」</span><span class="sxs-lookup"><span data-stu-id="0e3e4-130">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="0e3e4-131">と入力し、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-131">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="0e3e4-132">ボットが次のように応答することを確認します。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-132">Confirm that the bot responds as follows:</span></span>

    ![ボットをテストする](../media-draft/6-portal-testing-chat.png)

<span data-ttu-id="0e3e4-134">ボットがナレッジ ベースに接続されたので、最後の手順はそれを実際にテストすることです。</span><span class="sxs-lookup"><span data-stu-id="0e3e4-134">Now that the bot is connected to the knowledge base, the final step is to test it in the wild.</span></span> <span data-ttu-id="0e3e4-135">Skype でテストを行ったら、より自然なものになるでしょうか?</span><span class="sxs-lookup"><span data-stu-id="0e3e4-135">And what could be wilder than testing it with Skype?</span></span>