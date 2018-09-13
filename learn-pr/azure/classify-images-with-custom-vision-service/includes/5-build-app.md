Microsoft Custom Vision Service の真の力は、[Custom Vision の予測 API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) を使用して、開発者がそのインテリジェンスを独自のアプリケーションに簡単に組み込むことができることにあります。 このユニットでは、Visual Studio Code を使用して、Artwork という名前のアプリを変更し、前の演習でビルドおよびトレーニングを行ったモデルを使用します。

1. ご利用のシステムに Node.js がインストールされていない場合は、 https://nodejs.org に移動して使用しているオペレーティング システム用の最新の LTS バージョンをインストールします。

   > Node.js がインストールされているかどうかが不明な場合は、コマンド プロンプトまたはターミナル ウィンドウを開き、「**node -v**」と入力します。 Node.js のバージョン番号が表示されない場合は、Node.js はインストールされていません。 インストールされている Node.js のバージョンが 6.0 よりも古い場合は、最新バージョンをダウンロードしてインストールすることを強くお勧めします。

1. ご利用のワークステーションに Visual Studio Code がインストールされていない場合は、 http://code.visualstudio.com に移動して今すぐインストールします。

1. Visual Studio Code を起動して、**[ファイル]** メニューから **[フォルダーを開く]** を選択します。 後続のダイアログで、モジュール リソースに含まれている "Client\Artworks" フォルダーを選択します。

    ![Artworks フォルダーの選択](../media-draft/5-fe-select-folder.png)

1. Visual Studio Code で、**[表示]** > **[統合ターミナル]** を使用し、統合ターミナル ウィンドウを開きます。 次に、統合ターミナルで次のコマンドを実行し、アプリに必要なパッケージを読み込みます。

    ```
    npm install
    ```

1. Custom Vision Service ポータル内の Artwork プロジェクトに戻り、**[パフォーマンス]**、**[既定に設定]** の順にクリックして、モデルの最新のイテレーションが既定のイテレーションになっていることを確認します。 

    ![既定のイテレーションの指定](../media-draft/5-portal-make-default.png)

1. アプリを実行して使用し、Custom Vision Service を呼び出すには、エンドポイントと認証情報を含めるように、事前にアプリを変更する必要があります。 そのために、**[Prediction URL]\(予測 URL\)** をクリックします。

    ![予測 URL の情報を表示](../media-draft/5-portal-prediction-url.png)

1. 後続のダイアログに次の 2 つの URL が一覧されます。1 つは URL を使用して画像をアップロードするための URL、もう 1 つはローカルの画像をアップロードするための URL です。 イメージ ファイル用の予測 API URL をクリップボードにコピーします。 

    ![予測 API URL のコピー](../media-draft/5-copy-prediction-url.png)

1. Visual Studio Code に戻り、**[predict.js]** をクリックしてコード エディターで開きます。

    ![predict.js を開く](../media-draft/5-vs-predict-file.png)

1. 3 行目の "PREDICTION_ENDPOINT" をクリップボードの URL に置き換えます。

    ![予測 API URL の追加](../media-draft/5-vs-prediction-endpoint.png)

1. Custom Vision Service ポータルに戻り、予測 API キーをクリップボードにコピーします。 

    ![予測 API キーのコピー](../media-draft/5-copy-prediction-key.png)

1. Visual Studio Code に戻り、**predict.js** の 4 行目の "PREDICTION_KEY"をクリップボードの API キーに置き換えます。

    ![予測 API キーの追加](../media-draft/5-vs-prediction-key.png)

1. **predict.js** まで下へスクロールして、34 行目から始まるコードのブロックを確認します。 これは、AJAX を使用して Custom Vision Service を呼び出すコードです。 Custom Vision の予測 API の使用は、REST エンドポイントへのシンプルな認証済みの投稿を作成するのと同じくらい簡単です。

    ![予測 API を呼び出す](../media-draft/5-vs-code-block.png)

1. Visual Studio Code の統合ターミナルに戻り、次のコマンドを実行してアプリを開始します。

    ```
    npm start
    ```

1. Artworks アプリが開始され、次のようなウィンドウが表示されることを確認します。

    ![Artworks アプリ](../media-draft/5-app-startup.png)

Artworks は、Node.js と [Electron](https://electron.atom.io/) で記述されたクロスプラットフォーム アプリです。 そのため、Windows、macOS、Linux 上で同様に実行することができます。 次の演習では、このアプリを使用して、描いた画家によってイメージを分類します。