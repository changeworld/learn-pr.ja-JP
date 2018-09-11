記述するアプリケーション コードと同様に、ボット コードへの変更は、ローカルにテストしてデバッグを行ってから、運用環境にデプロイする必要があります。 ボットのデバッグを支援するため、Microsoft では [Bot Framework Emulator](https://emulator.botframework.com/) を提供しています。 この演習では、Visual Studio Code とエミュレーターを使用して、ボットをデバッグする方法を学習します。

1. Microsoft Bot Framework Emulator をインストールしていない場合は、ここで少し時間を取ってインストールします。 https://emulator.botframework.com/ からダウンロードできます。 Windows、macOS、および Linux 用のバージョンが使用できます。

1. Visual Studio Code の統合ターミナルで次のコマンドを実行して、[Restify](http://restify.com/) をインストールします。これは RESTful Web サービスをビルドおよび使用するためによく使用されている Node.js パッケージです。

    ```
    npm install restify
    ```

1. 次のコマンドにこの手順を繰り返して、[Microsoft Bot Framework Bot Builder SDK for Node.js](https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-quickstart) をインストールします。

    ```
    npm install botbuilder
    npm install botbuilder-azure
    npm install botbuilder-cognitiveservices
    ```

1. Visual Studio Code のアクティビティ バーで **[エクスプローラー]** ボタンをクリックします。 次に、**[app.js]** を選択してコード エディターで開きます。 このファイルには、ボットを駆動するコードが含まれています。このコードは、Azure Bot Service によって生成され、Azure portal からダウンロードされたものです。

    ![app.js を開く](../media-draft/5-vs-select-index-js.png)

1. **app.js** の内容を次のコードに置き換えた後に、ファイルを保存します。

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

1. 左側の余白をクリックして、20 行目、25 行目、および 30 行目にブレークポイントを設定します。
 
    ![app.js にブレークポイントを追加する](../media-draft/5-vs-add-breakpoints.png)

1. アクティビティ バーで **[デバッグ]** ボタンをクリックしてから緑色の矢印をクリックして、デバッグ セッションを開始します。 デバッグ コンソールに "http://localhost:3978/api/messages でボットのエンドポイントをテスト" が表示されることを確認します。
 
    ![デバッガーを起動する](../media-draft/5-vs-launch-debugger.png)

1. ボット コードは現在ローカルで実行されています。 Bot Framework Emulator を起動し、**[新しいボット構成を作成する]** をクリックします。 前の手順でデバッグ コンソールに表示されたボット名とボット URL を入力します。 次に、**[Save and connect]\(保存して接続\)** をクリックして、任意の場所に構成ファイルを保存します。

    > 今後は、[My Bots]\(マイ ボット\) の下のボット名をクリックするだけで、ボットに再接続することができます。

    ![ボットに接続する](../media-draft/5-new-bot-configuration.png)

1. エミュレーターの下部にあるボックスに「こんにちは」と入力して、**Enter** キーを押します。 Visual Studio Code が **app.js** の 20 行目で中断することを確認します。 次に、Visual Studio Code のデバッグ ツールバーで **[続行]** ボタンをクリックしてエミュレーターに戻り、ボットの応答を表示します。
 
    ![デバッガーで続行する](../media-draft/5-continue-debugging.png)

1. ボットから一連の質問がされます。 ブレークポイントにヒットするたびに、Visual Studio Code でそれらの質問に答え、**[続行]** をクリックします。 完了したら、デバッグ ツールバーの **[停止]** ボタンをクリックして、デバッグ セッションを終了します。

この時点で、完全に機能するボットが作成され、ボットを Microsoft Bot Framework Emulator でローカルに実行することでデバッグする方法を学習しました。 次の手順では、発行したナレッジ ベースにボットを接続して、ボットをよりインテリジェントにします。