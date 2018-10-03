[!INCLUDE [0-vm-note](0-vm-note.md)]

<span data-ttu-id="36f3a-101">このユニットでは、前に作成した QnA Maker のナレッジ ベースにボットを接続して、ボットがインテリジェントな会話を実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-101">In this unit, you will connect your bot to the QnA Maker knowledge base you built earlier so that the bot can carry on an intelligent conversation.</span></span> <span data-ttu-id="36f3a-102">ナレッジ ベースに接続するには、QnA Maker ポータルからいくつかの情報を取得し、それを Azure portal にコピーし、ボット コードを更新してから Azure にボットを再デプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="36f3a-102">Connecting to the knowledge base involves retrieving some information from the QnA Maker portal, copying it into the Azure portal, updating the bot code, and then redeploying the bot to Azure.</span></span>

1. <span data-ttu-id="36f3a-103">VM ブラウザー内の https://www.qnamaker.ai で QnA Maker ポータルに戻り、右上隅にあるアカウント ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-103">Return to the QnA Maker portal at https://www.qnamaker.ai in the VM browser and select the account button in the upper-right corner.</span></span>
1. <span data-ttu-id="36f3a-104">ドロップダウン メニューから **[エンドポイント キーの管理]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-104">Select **Manage endpoint keys** from the menu that drops down.</span></span>
1. <span data-ttu-id="36f3a-105">**[表示]** を選択して**プライマリ** エンドポイント キーを表示し、**[コピー]** を選択してそれをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-105">Select **Show** to show the **Primary** endpoint key and **Copy** to copy it to the clipboard.</span></span> <span data-ttu-id="36f3a-106">それをテキスト ファイルに貼り付けて、すぐに簡単に取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-106">Paste it into a text file, so you can easily retrieve it in a moment.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36f3a-107">ブラウザーの設定によっては、このユニットを完了するには、サード パーティの Cookie を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="36f3a-107">Depending on your browser settings, you may need to allow third-party cookies to complete this unit.</span></span>

1. <span data-ttu-id="36f3a-108">ページの上部にあるメニューで **[マイ ナレッジ ベース]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-108">Select **My knowledge bases** in the menu at the top of the page.</span></span>
1. <span data-ttu-id="36f3a-109">次に、先ほど作成したナレッジ ベースの **[コードの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-109">Then, select **View Code** for the knowledge base that you created earlier.</span></span>

1. <span data-ttu-id="36f3a-110">1 行目からナレッジ ベース ID、2 行目からホスト名をコピーします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-110">Copy the knowledge base ID from the first line and the host name from the second line.</span></span> <span data-ttu-id="36f3a-111">それらもテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-111">Paste them into a text file, as well.</span></span> <span data-ttu-id="36f3a-112">その後、ダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-112">Then, close the dialog.</span></span> <span data-ttu-id="36f3a-113">コピーするホスト名に "https://" プレフィックスを**含めないでください**。</span><span class="sxs-lookup"><span data-stu-id="36f3a-113">**Do not** include the "https://" prefix in the host name that you copy.</span></span>

    ![エンドポイントのナレッジ ベース ID とホスト名が強調表示されているサンプルの HTTP 要求を表示している QnA Maker ポータルのスクリーンショット。](../media/6-copy-endpoint-info.png)

1. <span data-ttu-id="36f3a-115">Azure portal で Web アプリ ボットに戻ります。</span><span class="sxs-lookup"><span data-stu-id="36f3a-115">Return to the Web App Bot in the Azure portal.</span></span> <span data-ttu-id="36f3a-116">左にあるメニューで **[アプリケーション設定]** を選択し、"QnAKnowledgebaseId"、"QnAAuthKey"、"QnAEndpointHostName" という名前のアプリケーション設定が見つかるまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-116">Select **Application settings** in the menu on the left, and scroll down until you find application settings named "QnAKnowledgebaseId," "QnAAuthKey," and "QnAEndpointHostName."</span></span> <span data-ttu-id="36f3a-117">取得したばかりのナレッジ ベース ID およびホスト名と、前に取得したエンドポイント キーを、これらのフィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-117">Paste the knowledge base ID and host name you just obtained and the endpoint key obtained previously into these fields.</span></span> <span data-ttu-id="36f3a-118">その後、上部にある **[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-118">Then, select **Save** at the top.</span></span>

    ![ボット ブレードと、アプリケーション設定のメニュー項目と該当する設定キーが強調表示されたアプリケーション設定の詳細を示している Azure portal のスクリーンショット。](../media/6-enter-app-settings.png)

1. <span data-ttu-id="36f3a-120">**Visual Studio Code** に戻り、**app.js** の内容を以下のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-120">Return to **Visual Studio Code** and replace the contents of **app.js** with the code below.</span></span> <span data-ttu-id="36f3a-121">その後、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-121">Then, save the file.</span></span>

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
        appPassword: process.env.MicrosoftAppPassword,
        openIdMetadata: process.env.BotOpenIdMetadata
    });

    // Listen for messages from users
    server.post('/api/messages', connector.listen());

    var tableName = 'botdata';
    var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
    var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

    // Create your bot with a function to receive messages from the user
    var bot = new builder.UniversalBot(connector);
    bot.set('storage', tableStorage);

    // Recognizer and and Dialog for preview QnAMaker service
    var previewRecognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey
    });

    var basicQnAMakerPreviewDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [previewRecognizer],
        defaultMessage: 'No match! Try changing the query terms!',
        qnaThreshold: 0.3
    }
    );

    bot.dialog('basicQnAMakerPreviewDialog', basicQnAMakerPreviewDialog);

    // Recognizer and and Dialog for GA QnAMaker service
    var recognizer = new builder_cognitiveservices.QnAMakerRecognizer({
        knowledgeBaseId: process.env.QnAKnowledgebaseId,
        authKey: process.env.QnAAuthKey || process.env.QnASubscriptionKey, // Backward compatibility with QnAMaker (Preview)
        endpointHostName: process.env.QnAEndpointHostName
    });

    var basicQnAMakerDialog = new builder_cognitiveservices.QnAMakerDialog({
        recognizers: [recognizer],
        defaultMessage: "I'm not quite sure what you're asking. Please ask your question again.",
        qnaThreshold: 0.3
    });

    bot.dialog('basicQnAMakerDialog', basicQnAMakerDialog);

    bot.dialog('/', //basicQnAMakerDialog);
        [
            function (session) {
                var qnaKnowledgebaseId = process.env.QnAKnowledgebaseId;
                var qnaAuthKey = process.env.QnAAuthKey || process.env.QnASubscriptionKey;
                var endpointHostName = process.env.QnAEndpointHostName;

                // QnA Subscription Key and KnowledgeBase Id null verification
                if ((qnaAuthKey == null || qnaAuthKey == '') || (qnaKnowledgebaseId == null || qnaKnowledgebaseId == ''))
                    session.send('Please set QnAKnowledgebaseId, QnAAuthKey and QnAEndpointHostName (if applicable) in App Settings. Learn how to get them at https://aka.ms/qnaabssetup.');
                else {
                    if (endpointHostName == null || endpointHostName == '')
                        // Replace with Preview QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerPreviewDialog');
                    else
                        // Replace with GA QnAMakerDialog service
                        session.replaceDialog('basicQnAMakerDialog');
                }
            }
        ]);
    ```

    > [!NOTE]
    > <span data-ttu-id="36f3a-122">30 行目の `QnAMakerDialog` インスタンスを作成するための呼び出し。</span><span class="sxs-lookup"><span data-stu-id="36f3a-122">The call to create a `QnAMakerDialog` instance on line 30.</span></span> <span data-ttu-id="36f3a-123">これにより、Azure Bot Service でビルドされたボットと、Microsoft QnA Maker で構築されたナレッジ ベースを統合するダイアログが作成されます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-123">This creates a dialog that integrates a bot built with the Azure Bot Service with a knowledge base built via Microsoft QnA Maker.</span></span>

1. <span data-ttu-id="36f3a-124">Visual Studio Code のアクティビティ バーで **[ソース管理]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-124">Select the **Source Control** button in the activity bar in Visual Studio Code.</span></span>
1. <span data-ttu-id="36f3a-125">**app.js** ファイルの上にマウス カーソルを移動し、[__+__] ボタンを選択して次のコミット向けにそのファイルの変更を行います。</span><span class="sxs-lookup"><span data-stu-id="36f3a-125">Hover over the **app.js** file and select the __+__ button to stage that file's changes for the next commit.</span></span>
1. <span data-ttu-id="36f3a-126">メッセージ ボックスに「ナレッジ ベースに接続済み」と入力し、チェック マークを選択して変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-126">Type "Connected to knowledge base" into the message box, and select the check mark to commit your changes.</span></span>

    > [!Warning]
    > <span data-ttu-id="36f3a-127">**package.json** ファイルへの変更がある場合は、それらがコミットに含まれていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="36f3a-127">If you see changes to a **package.json** file ensure you do NOT include them in your commit.</span></span> <span data-ttu-id="36f3a-128">コミットには、**app.js** への変更のみが含まれるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="36f3a-128">Your commit should only include your changes to **app.js**.</span></span>

1. <span data-ttu-id="36f3a-129">次に、省略記号 (__[...]__) を選択し、**[ブランチの発行]** コマンドを使用してこれらの変更をリモート リポジトリと Azure Web アプリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="36f3a-129">Then, select the ellipsis (__...__) button and use the **Publish Branch** command to push these changes to the remote repository and the Azure Web App.</span></span>

1. <span data-ttu-id="36f3a-130">Azure portal で Web アプリ ボットに戻り、左側にある **[Test in Web Chat]\(Web チャットでのテスト\)** を選択してテスト コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="36f3a-130">Return to the web app bot in the Azure portal, and select **Test in Web Chat** on the left to open the test console.</span></span> <span data-ttu-id="36f3a-131">チャット ウィンドウの下部にあるボックスに、「世界で最も普及しているソフトウェア プログラミング言語は何ですか?」</span><span class="sxs-lookup"><span data-stu-id="36f3a-131">Type "What's the most popular software programming language in the world?"</span></span> <span data-ttu-id="36f3a-132">と入力し、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-132">into the box at the bottom of the chat window and press **Enter**.</span></span> <span data-ttu-id="36f3a-133">ボットが応答することを確認します。</span><span class="sxs-lookup"><span data-stu-id="36f3a-133">Confirm that the bot responds.</span></span>

<span data-ttu-id="36f3a-134">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="36f3a-134">Congratulations!</span></span> <span data-ttu-id="36f3a-135">お客様のボットがナレッジ ベースに接続され、質問に応答できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="36f3a-135">Your bot is connected to the knowledge base and can respond to questions.</span></span>
