[!INCLUDE [0-vm-note](0-vm-note.md)]

記述するアプリケーション コードと同様に、ボット コードへの変更は、ローカルにテストしてデバッグを行ってから、運用環境にデプロイする必要があります。 ボットのデバッグを支援するため、Microsoft では Bot Framework Emulator を提供しています。 このユニットでは、Visual Studio Code とエミュレーターを使用して、ボットをデバッグする方法を学習します。

1. Visual Studio Code の統合ターミナルで次のコマンドを実行して、[Restify](http://restify.com/) をインストールします。これは RESTful Web サービスをビルドおよび使用するためによく使用されている Node.js パッケージです。

    ```bash
    npm install restify
    ```

1. 次のコマンドにこの手順を繰り返して、[Microsoft Bot Framework Bot Builder SDK for Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart) をインストールします。

    ```bash
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Visual Studio Code のアクティビティ バーで **[エクスプローラー]** ボタンを選択します。 
1. **[app.js]** を選択してコード エディターで開きます。 このファイルには、ボットを駆動するコードが含まれています。このコードは、Azure Bot Service によって生成され、Azure portal からダウンロードされたものです。

1. **app.js** の内容を次のコードに置き換えて、ファイルを保存します。

    ```JavaScript
    "use strict";
    var builder = require("botbuilder");
    var botbuilder_azure = require("botbuilder-azure");

    var useEmulator = true;
    var userName = "";
    var yearsCoding = "";
    var selectedLanguage = "";

    var connector = useEmulator ? new builder.ChatConnector() : new botbuilder_azure.BotServiceConnector({
        appId: process.env.MicrosoftAppId,
        appPassword: process.env.MicrosoftAppPassword
    });

    var bot = new builder.UniversalBot(connector);

    bot.dialog('/', [

    function (session) {
        builder.Prompts.text(session, "Hello, and welcome to QnA Factbot! What's your name?");
    },

    function (session, results) {
        userName = results.response;
        builder.Prompts.number(session, "Hi " + userName + ", how many years have you been writing code?");
    },

    function (session, results) {
        yearsCoding = results.response;
        builder.Prompts.choice(session, "What language do you love the most?", ["C#", "Python", "Node.js", "Visual FoxPro"]);
    },

    function (session, results) {
        selectedLanguage = results.response.entity;

        session.send("Okay, " + userName + ", I think I've got it:" +
            " You've been writing code for " + yearsCoding + " years," +
            " and prefer to use " + selectedLanguage + ".");
    }]);

    var restify = require('restify');
    var server = restify.createServer();

    server.listen(3978, function() {
        console.log('test bot endpoint at http://localhost:3978/api/messages');
    });

    server.post('/api/messages', connector.listen());
    ```

1. 左側の余白を選択して、20 行目、25 行目、および 30 行目にブレークポイントを設定します (`builder.Prompts...` の呼び出し)。

1. アクティビティ バーで **[デバッグ]** ボタンを選択してから、緑色の矢印の **[デバッグの開始]** ボタンを選択してデバッグ セッションを開始します。 デバッグ コンソールに "http://localhost:3978/api/messages でボットのエンドポイントをテスト" が表示されることを確認します。

    ![デバッグのナビゲーション項目とデバッグ セッションを開始するために使用されるデバッグ再生ボタンが強調表示されたデバッグ システムを表示している Visual Studio Code のスクリーンショット。](../media/5-vs-launch-debugger.png)

    ボット コードは現在ローカルで実行されています。

1. [スタート] メニューから **Bot Framework Emulator** を起動します。

1. **[Enter your endpoint URL]\(エンドポイント URL を入力してください\)** フィールドを選択します。 前の手順でデバッグ コンソールに表示されたボット名とボット URL を入力します。

1. [Microsoft アプリ ID]、[Microsoft アプリ パスワード]、[ロケール] のフィールドは空のままにして、**[接続]** を選択します。

1. 次に、**[Save and connect]\(保存して接続\)** を選択して、任意の場所に構成ファイルを保存します。

    >[!NOTE]
    > 今後は、[My Bots]\(マイ ボット\) の下のボット名を選択するだけで、ボットに再接続することができます。

    ![[保存] と接続ボタンが強調表示された新しいボット構成画面を示している Bot Framework Emulator のスクリーンショット。](../media/5-new-bot-configuration.png)

1. エミュレーターの下部にあるボックスに「こんにちは」と入力して、**Enter** キーを押します。 Visual Studio Code が **app.js** の 20 行目で中断することを確認します。 次に、Visual Studio Code のデバッグ ツールバーで **[続行]** ボタンを選択してエミュレーターに戻り、ボットの応答を表示します。

1. ボットから一連の質問が表示されます。 ブレークポイントにヒットするたびに、Visual Studio Code でそれらの質問に答え、**[続行]** を選択します。 完了したら、デバッグ ツールバーの **[停止]** ボタンを選択して、デバッグ セッションを終了します。

この時点で、完全に機能するボットが作成され、ボットを Microsoft Bot Framework Emulator でローカルに実行することでデバッグする方法を学習しました。 次の手順では、発行したナレッジ ベースにボットを接続して、ボットをよりインテリジェントにします。