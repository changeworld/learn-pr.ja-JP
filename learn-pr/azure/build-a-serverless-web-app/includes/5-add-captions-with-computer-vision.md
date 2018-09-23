現時点では、このアプリケーションは画像をアップロードして表示できる機能ギャラリーです。 このモジュールでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした画像のキャプションを生成し、Azure Cosmos DB 内の画像メタデータと共にそのキャプションを保存する方法を学習します。

## <a name="create-a-computer-vision-account"></a>Computer Vision アカウントを作成する

Microsoft Cognitive Services は、開発者がよりインテリジェントなアプリケーションを開発するためのサービスのコレクションです。 Computer Vision API は、高度なアルゴリズムを使用して画像を処理し、各画像に関する情報を返す、サーバーレス サービスです。 Free レベルでは、1 か月あたり最大 5,000 回の API 呼び出しを実行できます。

リソース グループ内で一意の名前を持つ、**ComputerVision** という種類の新しい Cognitive Services アカウントを作成します。 Free レベルの場合は、**F0** を SKU として使用します。 Computer Vision の既存のアカウントが既にある場合は、Standard アカウント (S1) の作成が必要になる場合があり、これによってコストが発生することがあります。

```azurecli
az cognitiveservices account create \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <computer vision account name> \
    --kind ComputerVision \
    --sku F0 \
    --location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location -o tsv)
```

データ同意通知に同意するように求められたら、`y` を入力し、`Enter` を押します。 コマンドが完了するまでに 1 〜 2 分かかる場合があります。

## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Computer Vision の URL およびキー用の関数アプリ設定を作成する

Computer Vision API を呼び出すには、URL とキーが必要です。

1. Computer Vision API のキーと URL を取得し、Bash 変数に保存します。

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query key1 --output tsv)
    export COMP_VISION_URL=$(az cognitiveservices account show -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query endpoint --output tsv)
    ```

1. それぞれ **COMP_VISION_KEY** と **COMP_VISION_URL** という名前のアプリ設定を、関数アプリで作成します。

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```

## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a>ResizeImage 関数から Computer Vision API を呼び出す

次の手順では、**ResizeImage** 関数を変更して Computer Vision API を呼び出し、アップロードした各画像について説明して、その説明を Azure Cosmos DB に保存します。

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。

1. 関数アプリを開きます。

::: zone pivot="csharp"

3. 左側のナビゲーションを使用し、**ResizeImage** 関数を検索して、そのコード ウィンドウを開きます。

1. コードを [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) ファイルの内容で置き換えます。 このコードでは、`HttpClient` を使用して Computer Vision API を呼び出し、その結果を Azure Cosmos DB に保存しています。

1. コード ウィンドウの下の **[ログ]** をクリックしてログ パネルを展開します。

1. **[保存]** をクリックします。 関数が確実にエラーのない状態で正常に保存されていることをログ パネルで確認します。

::: zone-end

::: zone pivot="javascript"

2. この関数では、Computer Vision API への HTTP 呼び出しを行うために、npm の `axios` パッケージが必要です。 npm パッケージをインストールするには、左側のナビゲーションで関数アプリの名前をクリックし、**[プラットフォーム機能]** をクリックします。

1. **[コンソール]** をクリックしてコンソール ウィンドウを表示します。

1. コンソールで `npm install axios` コマンドを実行します。 操作が完了するまで数分かかることがあります。

1. 左側のナビゲーションで関数名 (**ResizeImage**) をクリックし、関数を表示します。 **index.js** ファイルのすべてを、[**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) ファイルの内容で置き換えます。

1. コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。

1. **[保存]** をクリックします。 関数が確実にエラーのない状態で正常に保存されていることをログ パネルで確認します。

::: zone-end

## <a name="test-the-application"></a>アプリケーションをテストする

1. ブラウザーでアプリケーションを開きます。

1. 画像ファイルを選択してアップロードします。

1. 数秒後に、新しい画像のサムネイルがこのページに表示されます。 画像をポイントすると、Computer Vision によって生成された説明が表示されます。

## <a name="summary"></a>まとめ

このユニットでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした各画像用のキャプションを自動的に生成する方法を学習しました。 次は、Azure App Service 認証を使用して、アプリケーションに認証を追加する方法を学習します。