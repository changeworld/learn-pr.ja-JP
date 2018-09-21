
## <a name="what-is-microsoft-cognitive-services"></a>Microsoft Cognitive Services とは

Microsoft Cognitive Services は、誰でもサービスとして使用できる機械学習アルゴリズムのセットです。 このインテリジェンスをアプリのためにゼロから構築する代わりに、視覚、音声、言語、知識、検索に対してこれらのサービスを使用できます。 各サービスは無料で試すことができます。 サービスをアプリに統合する場合は、有料のサブスクリプションにサインアップします。 このモデルの Computer Vision API に注目します。

> [!TIP]
> すべての Cognitive Services の一覧を確認するには、「[Cognitive Services のディレクトリ](https://azure.microsoft.com/services/cognitive-services/directory/)」をご覧ください。 

## <a name="what-is-the-computer-vision-api"></a>Computer Vision API とは

Computer Vision API では、画像を処理して分析情報を返すアルゴリズムが提供されます。 たとえば、画像に成人向けコンテンツが含まれるかどうか調べたり、画像からすべての顔を検索するのに使用したりできます。 他にも、ドミナント カラーやアクセント カラーの推定、画像の内容の分類、完全な英語の文による画像の説明などの機能もあります。 さらに、大きな画像を効果的に表示するために画像のサムネイルをインテリジェントに生成することもできます。

> [!TIP]
> Computer Vision API は世界中の多くのリージョンで使用できます。 近くのリージョンを探すには、「[リージョン別の利用可能な製品](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)」をご覧ください。

次の目的に Computer Vision API を使用できます。

- 画像を分析して分析情報を得る
- 光学式文字認識 (OCR) を使用して画像から印刷したテキストを抽出する
- 画像から印刷テキストと手書きテキストを認識する
- 著名人やランドマークを認識する
- 動画を分析する 
- 画像のサムネイルを生成する 

## <a name="how-to-call-the-computer-vision-api"></a>Computer Vision API を呼び出す方法

アプリケーションで Computer Vision を呼び出すには、クライアント ライブラリまたは直接 REST API を使用します。 このモジュールでは REST API を呼び出します。 呼び出しを行うには次のようにします。

1. API アクセス キーを取得する

    Computer Vision サービス アカウントにサインアップすると、アクセス キーを割り当てられます。 **すべての**要求のヘッダーでキーを渡す必要があります。 

1. API に対して POST 呼び出しを行う

    URL の書式を次のように設定します。**<リージョン>**.api.cognitive.microsoft.com/vision/v2.0/**<リソース>**/**<パラメーター>** 

    - **<リージョン>** - アカウントを作成したリージョンです (例: *westus*)。
    - **<リソース>** - 呼び出す Computer Vision リソースです (例: `analyze`、`describe`、`generateThumbnail`、`ocr`、`models`、`recognizeText`、`tag`)。

    処理対象の画像は、生画像バイナリまたは画像の URL として提供できます。

    要求ヘッダーには、この API へのアクセスを提供するサブスクリプション キーを含める必要があります。

1. 応答を解析する

    応答には、Computer Vision API によって画像から検出された分析情報が JSON ペイロードとして保持されています。

`westus` リージョンで Computer Vision サブスクリプションを作成し、API にアクセスするためのサブスクリプション キーを取得したものとします。 キーは架空の値 `xxxx-xxxxx-xxxx-xxxx` にしましょう。 そして、`http://example.com/images/test.jpg` にある画像のサムネイルを生成する必要があります。 `generateThumbnail` を使用すると、要求は次のようになります。

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

このモジュールでは、統合された Cloud Shell を使用して Azure CLI ですべての演習を実行します。 このセットアップについてもう少し調べてみましょう。

## <a name="what-is-the-azure-cli"></a>Azure CLI とは

Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。 このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使用して使用できます。

> [!IMPORTANT]
> 現在使用できる Azure CLI ツールには、Azure CLI 1.0 と Azure CLI 2.0 の 2 つのバージョンがあります。 ここでは最新バージョンである Azure CLI 2.0 を使用します。レガシ スクリプトを実行する場合を除いて、こちらを使用することをお勧めします。 Azure CLI 1.0 は `azure` コマンドで始まり、Azure CLI 2.0 は `az` コマンドで始まります。

## <a name="az-cognitiveservices-commands"></a>az cognitiveservices のコマンド

Azure CLI には、Azure で Cognitive Services アカウントを管理するための `cognitiveservices` コマンドが含まれます。 特定のタスクを実行するためのサブコマンドがいくつかあります。 最も一般的なものを次に示します。

| サブコマンド | 説明 |
|-------------|-------------|
| `list` | 利用可能な Azure Cognitive Services アカウントを一覧表示します。 |
| `account show` | Azure Cognitive Services アカウントの詳細を取得します。 |
| `account create` | Azure Cognitive Services アカウントを作成します。 |
| `account delete` | Azure Cognitive Services アカウントを削除します。 |
| `keys list` | Azure Cognitive Services アカウントのキーを一覧表示します。 |

Azure CLI を使用して、Cognitive Services を作成してみましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Cognitive Services アカウントを作成する

Computer Vision API を呼び出すには、API アクセス キーが必要です。 アクセス キーを取得するには、Computer Vision API 用の Cognitive Services アカウントが必要です。 サブスクリプションでアカウントを作成するには、`az cognitiveservices create` を使用します。

 リソース グループに Cognitive Services アカウントを作成するには、`az cognitiveservices create` コマンドを使用します。  このコマンドを呼び出すときは、次の 5 つのパラメーターを指定する必要があります。

> [!Tip]
> Azure CLI のパラメーターで使用するほとんどのフラグは、1 文字に省略できます。 たとえば、`--location` の代わりに `-l` と指定できます。 長い形式は、わかりやすくするために使用されます。

| パラメーター | 説明 |
|-----------|-------------|
| `resource-group` | Cognitive Services アカウントを所有するリソース グループです。 この対話型サンドボックス セッションでは、<rgn>[サンドボックス リソース グループ名]</rgn> を使用します |
| `kind` | Cognitive Services アカウントの API 名です。 |
| `name` | Cognitive Services アカウントの名前です。 |
| `sku` | Cognitive Services アカウントの SKU です。|
| `location` | この API の呼び出しを行う場所つまりリージョンです。 次の一覧からいずれかの場所を選択します。 |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

Azure Cloud Shell で次のコマンドを実行します。 `[location]` は近くの場所に置き換えてください。

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> 選択した場所を憶えておいてください。 API に対するすべての呼び出しは、このリージョンから行います。

**Computer Vision** API 用の Cognitive Services アカウントを作成しました。 *S1* SKU を選択し、アカウントの名前を **ComputerVisionService** にしました。 このアカウントはリソース グループ **<rgn>[サンドボックス リソース グループ名]</rgn>** によって所有されており、`--location` パラメーターで設定した場所から API を呼び出します。 

コマンドで Cognitive Services アカウントの作成が完了すると、**provisioningState** プロパティが **Succeeded** に設定された JSON 応答を受け取ります。

## <a name="get-an-access-key"></a>アクセス キーを取得する

アカウントが正常に作成されると、そのアカウントのサブスクリプション キーつまりアクセス キーを取得できます。

1. Azure Cloud Shell で次のコマンドを実行します。

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    ```
    
    上のコマンドでは、特定のリソース グループによって所有されている **ComputerVisionService** という名前の Cognitive Services アカウントに関連付けられているキーが返されます。 2 つのキーが返されますが、1 つは予備のキーです。 キーは憶えにくいので、1 番目のキーを変数に格納し、API のすべての呼び出しにそれを使用します。

2.  Azure Cloud Shell で次のコマンドを実行します。

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    Azure CLI 2.0 では、`--query` 引数を使用して、コマンドの結果に対して JMESPath クエリを実行します。 JMESPath は、CLI の出力からデータを選択して表示できるようにする、JSON 用のクエリ言語です。 これらのクエリは、表示書式設定の前に、JSON 出力に対して実行されます。
    `--query` 引数は、Azure CLI のすべてのコマンドでサポートされています。 
    
    この例では、"key1" という名前のエントリに対するキーの一覧をクエリし、結果を **tsv** の形式に出力します。 この形式では、文字列値を囲む引用符が削除されます。 結果を変数 **key** に代入します。
    
    > [!IMPORTANT]
    > モジュール全体でこのキーを使用するので、変数に保存するのはよい方法です。 値を紛失したり、変数が設定されなくなったときは、コマンドをもう一度実行して設定します。  

3. キーの値を表示するには、Azure Cloud Shell で次のコマンドを実行します。

    ```azurecli
    echo $key
    ```

アカウントとキーが用意できたので、いよいよ API の呼び出しを実行します。