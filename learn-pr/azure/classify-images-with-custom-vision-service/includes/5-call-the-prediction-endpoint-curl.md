前の演習では、Custom Vision Service ポータルの**クイック テスト**機能を使用して、トレーニング済みのモデルをテストしました。 これは、テスト画像をいくつか使ってモデルの精度を簡単に確認する優れた方法です。 次に、HTTP 経由でモデルの予測エンドポイントを呼び出してみましょう。

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Custom Vision Service ポータルで *Artworks** プロジェクトに戻り、**[Performance]\(パフォーマンス\)** タブを選択します。

    ![[Performance]\(パフォーマンス\) タブの選択](../media/5-performance-tab.png)

1. **[Make default]\(既定にする\)** を選択して、モデルの最新のイテレーションを既定のイテレーションにします。

1. **[Prediction URL]\(予測 URL\)** を選択します。 呼び出しを行うのに必要な情報のダイアログ ボックスが表示されます。 

    ![予測 URL の情報を表示](../media/5-portal-prediction-url.png)

    ダイアログ ボックスで示されているように、予測エンドポイントを呼び出して画像の URL を渡すことできます。 生の画像を要求の本文でエンドポイントに渡すこともできます。

    このダイアログ ボックスの 3 つの情報を書き留めておきます。
     - **[Prediction-Key]\(予測キー\)**: すべての要求のヘッダーとしてこのキーを設定する必要があります。 このキーによってエンドポイントへのアクセス権が提供されます。
    - **[Request URL]\(要求 URL\)**: ダイアログには 2 つの異なる URL が表示されます。 画像の URL を送信する場合は、末尾が `/url` の最初の URL を使用します。 要求の本文で生の画像を送信する場合は、末尾が `/image` の 2 番目の URL を使用しします。
    - **[Content-Type]\(コンテンツ タイプ\)**: 生の画像を送信する場合は、要求の本文に画像のバイナリ表現を設定し、コンテンツ タイプを `application/octet-stream` に設定します。 画像の URL を送信する場合は、本文に JSON として URL を設定し、コンテンツ タイプを `application/json` に設定します。
    

3. **[How to use the Prediction API]\(予測 API の使用方法\)** ダイアログから最初の URL と `Prediction-Key` の値をコピーして保存します。 

> [!TIP]
> **cURL** は、ファイルを送受信するために使用できるコマンドライン ツールです。 このツールは、Linux、macOS、および Windows 10 に含まれており、その他のほとんどのオペレーティング システムでもダウンロードできます。 cURL では、HTTP、HTTPS、FTP、FTPS、SFTP、LDAP、TELNET、SMTP、POP3 などのさまざまなプロトコルをサポートします。詳細については、次のリンクを参照してください。
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/> 
> 
> cURL はサンドボックスの Azure Cloud Shell に既にインストールされています。 そこでこの演習では cURL を使用し、エンドポイントへの HTTP 呼び出しを行います。

2. Cloud Shell で次のコマンドを実行します。 **[endpoint-URL]** は、最後の手順で保存した URL に置き換えます。 **[Prediction-Key]** は、最後の手順で保存した `Prediction-Key` の値に置き換えます。 

    ```azurecli
    curl [endpoint-URL] \
    -H "Prediction-Key: [Prediction-Key]" \
    -H "Content-Type: application/json" \
    -d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/VanGoghTest_02.jpg'}" \
    | jq '.'
    ```

    コマンドが完了すると、次のスクリーンショットのような JSON 応答が表示されます。 API は、モデル内のすべてのタグに対する確率を返します。 ご覧のように、`"painting"` **tagName** の値に 1 に近い確率が設定されているこの画像は、確かに絵画です。 ただし、モデルのトレーニングに使用したどの画家の絵画でもありません。 

    ![テスト用のピカソの画像のサムネイル](../media/5-prediction-json.png) 

3. 上の要求本文内の URL を次の表にある URL に置き換え、さらに予測してみてください。 

    |イメージ  | URL  |
    |---------|---------|
    |![テスト用のピカソの画像のサムネイル](../media/picasso-test-02-thumb.jpg)     | `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PicassoTest_02.jpg`        |
    |![テスト用のレンブラントの画像のサムネイル](../media/rembrandt-test-01-thumb.jpg)     |  `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/RembrandtTest_01.jpg`       |
    |![テスト用のポロックの画像のサムネイル](../media/pollock-test-01-thumb.jpg)  |   `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PollockTest_01.jpg`     |
   

この予測エンドポイントは、想定どおりに動作しています。 API の呼び出しは、予測キーと画像の URL を指定してエンドポイントに HTTP 要求を行うだけの簡単なものです。