[!INCLUDE [0-vm-note](0-vm-note.md)]

このユニットでは、前に作成した QnA Maker のナレッジ ベースにボットを接続して、ボットがインテリジェントな会話を実行できるようにします。 ナレッジ ベースに接続するには、QnA Maker ポータルからいくつかの情報を取得し、それを Azure portal にコピーし、ボット コードを更新してから Azure にボットを再デプロイする必要があります。

1. VM ブラウザー内の https://www.qnamaker.ai で QnA Maker ポータルに戻り、右上隅にあるアカウント ボタンを選択します。
1. ドロップダウン メニューから **[エンドポイント キーの管理]** を選択します。
1. **[表示]** を選択して**プライマリ** エンドポイント キーを表示し、**[コピー]** を選択してそれをクリップボードにコピーします。 それをテキスト ファイルに貼り付けて、すぐに簡単に取得できるようにします。

    > [!NOTE]
    > ブラウザーの設定によっては、このユニットを完了するには、サード パーティの Cookie を許可する必要があります。

1. ページの上部にあるメニューで **[マイ ナレッジ ベース]** を選択します。
1. 次に、先ほど作成したナレッジ ベースの **[コードの表示]** を選択します。

1. 1 行目からナレッジ ベース ID、2 行目からホスト名をコピーします。 それらもテキスト ファイルに貼り付けます。 その後、ダイアログを閉じます。 コピーするホスト名に "https://" プレフィックスを**含めないでください**。

    ![エンドポイントのナレッジ ベース ID とホスト名が強調表示されているサンプルの HTTP 要求を表示している QnA Maker ポータルのスクリーンショット。](../media/6-copy-endpoint-info.png)

1. Azure portal で Web アプリ ボットに戻ります。 左にあるメニューで **[アプリケーション設定]** を選択し、"QnAKnowledgebaseId"、"QnAAuthKey"、"QnAEndpointHostName" という名前のアプリケーション設定が見つかるまで下にスクロールします。 取得したばかりのナレッジ ベース ID およびホスト名と、前に取得したエンドポイント キーを、これらのフィールドに貼り付けます。 その後、上部にある **[保存]** を選択します。

    ![ボット ブレードと、アプリケーション設定のメニュー項目と該当する設定キーが強調表示されたアプリケーション設定の詳細を示している Azure portal のスクリーンショット。](../media/6-enter-app-settings.png)

1. **Visual Studio Code** に戻り、**app.js** の内容を以下のコードに置き換えます。 その後、ファイルを保存します。

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
    > 30 行目の `QnAMakerDialog` インスタンスを作成するための呼び出し。 これにより、Azure Bot Service でビルドされたボットと、Microsoft QnA Maker で構築されたナレッジ ベースを統合するダイアログが作成されます。

1. Visual Studio Code のアクティビティ バーで **[ソース管理]** ボタンを選択します。
1. **app.js** ファイルの上にマウス カーソルを移動し、[__+__] ボタンを選択して次のコミット向けにそのファイルの変更を行います。
1. メッセージ ボックスに「ナレッジ ベースに接続済み」と入力し、チェック マークを選択して変更をコミットします。

    > [!Warning]
    > **package.json** ファイルへの変更がある場合は、それらがコミットに含まれていないことを確認してください。 コミットには、**app.js** への変更のみが含まれるようにする必要があります。

1. 次に、省略記号 (__[...]__) を選択し、**[ブランチの発行]** コマンドを使用してこれらの変更をリモート リポジトリと Azure Web アプリにプッシュします。

1. Azure portal で Web アプリ ボットに戻り、左側にある **[Test in Web Chat]\(Web チャットでのテスト\)** を選択してテスト コンソールを開きます。 チャット ウィンドウの下部にあるボックスに、「世界で最も普及しているソフトウェア プログラミング言語は何ですか?」 と入力し、**Enter** キーを押します。 ボットが応答することを確認します。

お疲れさまでした。 お客様のボットがナレッジ ベースに接続され、質問に応答できるようになりました。
