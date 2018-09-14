
## <a name="what-is-microsoft-cognitive-services"></a>Microsoft Cognitive Services とは

Microsoft Cognitive Services は、一連の機械学習アルゴリズムを使用するすべてのユーザー用のサービスとして使用できます。 アプリをゼロからのこのインテリジェンスを構築する代わりに視覚、音声、言語、知識、および検索のこれらのサービスを使用できます。 各サービスを無料で試すことができます。 サービスをアプリに統合する場合は、サインアップする有料サブスクリプション。 このモデルでは、Computer Vision API に注目を注目します。

> [!TIP]
> すべての Cognitive Services の一覧を表示するには、チェック アウト、 [Cognitive Services ディレクトリ](https://azure.microsoft.com/services/cognitive-services/directory/)します。 

## <a name="what-is-the-computer-vision-api"></a>Computer Vision API とは何ですか。

Computer Vision API では、画像を処理して洞察を返すアルゴリズムを提供します。 たとえば、かわかりますイメージを成熟したコンテンツは、イメージのすべての面の検索に使用できますか。 支配的なとアクセント カラーを見積もりのイメージのコンテンツを分類する完全な英語の文を使用してイメージを記述するなどの他の機能もあります。 さらに、大きな画像を効果的に表示するイメージのサムネイルも適切に生成できます。

> [!TIP]
> Computer Vision API は使用可能な多くのリージョンで、世界中でします。 お近くのリージョンを検索するには、次を参照してください。、[リージョンで利用可能な製品](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)します。

Computer Vision API を使用できます。

- 画像をインサイトを分析します。
- 光学式文字認識 (OCR) を使用してイメージから印刷したテキストを抽出します。
- イメージから印刷し、手書きのテキストを認識します。
- 著名人およびランドマークを認識します。
- ビデオを分析します。 
- イメージのサムネイルを生成します。 

## <a name="how-to-call-the-computer-vision-api"></a>Computer Vision API を呼び出す方法

クライアント ライブラリまたは REST API を直接使用するアプリケーションでは、コンピューター ビジョンを呼び出します。 このモジュールで REST API を呼び出します。 呼び出しを行うには。

1. API アクセス キーを取得します。

    Computer Vision サービス アカウントにサインアップするときに、アクセス キーが割り当てられます。 ヘッダーには、キーを渡す必要がある**すべて**要求。 

1. API への POST 呼び出しを行う

    次のように、URL を書式設定:**リージョン**.api.cognitive.microsoft.com/vision/v2.0/**リソース**/**[パラメーター]** 

    - **リージョン**-などで、アカウントを作成したリージョン*westus*します。
    - **リソース**-Computer Vision リソースなどを呼び出している`analyze`、 `describe`、 `generateThumbnail`、 `ocr`、 `models`、 `recognizeText`、`tag`します。

    Raw 画像のバイナリまたはイメージの URL として処理するイメージを指定することができます。

    要求ヘッダーには、この API にアクセスを提供するサブスクリプション キーを含める必要があります。

1. 応答を解析します。

    応答は、Computer Vision API が JSON ペイロードとして、イメージの詳細が、その情報を保持します。

Computer Vision サブスクリプションで作成したとします、`westus`リージョン、API にアクセスするサブスクリプション キーがあります。 キーの架空の値を与えてみましょう`xxxx-xxxxx-xxxx-xxxx`します。 イメージがあるは、サムネイルを生成するようになりました`http://example.com/images/test.jpg`します。 使用して`generateThumbnail`要求は、次のようになります。

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

このモジュールで、Azure CLI でのすべての演習実行し、統合された Cloud Shell を使用します。 それではこのセットアップについて、もう少しです。

## <a name="what-is-the-azure-cli"></a>Azure CLI とは

Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。 このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使用して使用できます。

> [!IMPORTANT]
> 現在使用できる Azure CLI ツールには、Azure CLI 1.0 と Azure CLI 2.0 の 2 つのバージョンがあります。 ここでは最新バージョンである Azure CLI 2.0 を使用します。レガシ スクリプトを実行する場合を除いて、こちらを使用することをお勧めします。 Azure CLI 1.0 は `azure` コマンドで始まり、Azure CLI 2.0 は `az` コマンドで始まります。

## <a name="az-cognitiveservices-commands"></a>az cognitiveservices コマンド

Azure CLI を含む、 `cognitiveservices` azure Cognitive Services アカウントを管理するコマンド。 特定のタスクを実行するためのサブコマンドがいくつかあります。 最も一般的なものを次に示します。

| サブコマンド | 説明 |
|-------------|-------------|
| `list` | 利用可能な Azure Cognitive Services アカウントを一覧表示します。 |
| `account show` | Azure Cognitive Services アカウントの詳細を取得します。 |
| `account create` | Azure Cognitive Services アカウントを作成します。 |
| `account delete` | Azure Cognitive Services アカウントを削除します。 |
| `keys list` | Azure Cognitive Services アカウントのキーの一覧を取得します。 |

Azure CLI を使用して、Cognitive Services を作成してみましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Cognitive Services アカウントを作成する

API アクセス キー、Computer Vision API を呼び出す必要があります。 アクセス キーを取得するには、Computer Vision API の Cognitive Services アカウントを必要です。 使用して`az cognitiveservices create`サブスクリプションでアカウントを作成します。

 コマンドは、`az cognitiveservices create`リソース グループで、Cognitive Services アカウントを作成するために使用します。  このコマンドを呼び出すときに、次の 5 つのパラメーターを指定する必要があります。

> [!Tip]
> Azure CLI パラメーターのほとんどのフラグは、1 つの文字に省略できます。 たとえば、私たちとでした`-l`の代わりに`--location`します。 長い形式は、わかりやすくするために使用されます。

| パラメーター | 説明 |
|-----------|-------------|
| `resource-group` | Cognitive services アカウントを所有するリソース グループ。 このサンド ボックスの対話型セッションで使用する<rgn>[サンド ボックス リソース グループ名]</rgn> |
| `kind` | Cognitive services アカウントの API の名前。 |
| `location` | 場所、または、この API の呼び出しを作成する領域。 |
| `name` | Cognitive service アカウントの名前。 |
| `sku` | Cognitive services アカウントの Sku。|

1. Azure Cloud Shell で次のコマンドを実行します。

> [!Tip]
> このモジュールでの Azure CLI コマンドは、水平方向のスクロール量を削減する複数行のコマンドとして記述されます。 自由にする場合、これらのコマンドを単一行のコマンドに変換します。 

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--location westus2 \
--name ComputerVisionService \
--sku F0 \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

ここでの cognitive services アカウントを作成した、**コンピューター** API。 無料の sku を選択した*F0*と、アカウントの名前付き**ComputerVisionService**します。 リソース グループが、アカウントが所有する**<rgn>[サンド ボックス リソース グループ名]</rgn>** で設定した場所から API を呼び出すことと、`--location`パラメーター。 この例では、westus2 からの呼び出しを行うことが可能にします。 その他の任意のリージョン間呼び出しを行うと、エラーが発生します。

含む JSON 応答を受け取りますコマンドでは、cognitive services アカウントの作成が完了すると、 **provisioningState**プロパティに設定**Succeeded**します。 正常な応答の外観の例を示します。

<!-- TODO: find out the default location! -->

```json
{
  "endpoint": "https://westus2.api.cognitive.microsoft.com/vision/v2.0",
  "etag": "\"5c0070e5-0000-0000-0000-5b98cafa0000\"",
  "id": "/subscriptions/ae49d8aa-de86-418a-ba8c-4e9f3add2620/resourceGroups/ComputerVisionRG/providers/Microsoft.CognitiveServices/accounts/ComputerVisionService",
  "internalId": "437c9519bead47e6ba4b8e0842c1f76f",
  "kind": "ComputerVision",
  "location": "westus2",
  "name": "ComputerVisionService",
  "provisioningState": "Succeeded",
  "resourceGroup": "ComputerVisionRG",
  "sku": {
    "name": "F0",
    "tier": null
  },
  "tags": null,
  "type": "Microsoft.CognitiveServices/accounts"
}
```

> [!NOTE]
> 既に Azure サブスクリプションを使用して設定 Computer Vision cognitive services アカウントがある場合、 **F0**次のコマンドを実行する前に、Sku、次のメッセージが表示されます。
>
> `Only one free account is allowed for account type 'ComputerVision'` 
>
> このモジュールにそのアカウントを使用または、Sku を変更する必要があります、`az cognitiveservices account create`コマンドを*S1*または別の Sku。

## <a name="get-an-access-key"></a>アクセス キーを取得する

行ってから、アカウントが正常に作成は、サブスクリプション キーを取得またはアクセス キーは、このアカウントのできます。

1. Azure Cloud Shell で次のコマンドを実行します。

```azurecli
az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

上記のコマンドと呼ばれる、cognitive services アカウントに関連付けられているキーを返します**ComputerVisionService**、特定のリソース グループによって所有されています。 2 つのキーを返します - 1 つが予備のキー。 キーは、最初のキーのすべての呼び出し、API を使用する変数に保存しますように、覚えにくい。

2.  Azure Cloud Shell で次のコマンドを実行します。

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```

Azure CLI 2.0 を使用して、`--query`コマンドの結果に対して JMESPath クエリを実行する引数。 JMESPath は、CLI の出力からデータを選択して表示できるようにする、JSON 用のクエリ言語です。 これらのクエリは、JSON 出力で、表示の書式設定する前に実行されます。
`--query` 引数は、Azure CLI のすべてのコマンドでサポートされています。 

この例でにキーの一覧方法をクエリに結果を出力し、"key1"という名前のエントリの**tsv**形式。 この形式は、文字列値を囲む引用符を削除します。 結果を変数に代入**キー**します。

> [!IMPORTANT]
> ここに、モジュールでは、このキーを使用するための変数に保存することをお勧めします。 値を紛失した、または変数が設定されていない、設定するには、もう一度コマンドを実行します。  

3. を、キーの値を表示するには、Azure Cloud Shell で、次のコマンドを実行します。

```azurecli
echo $key
```

アカウントとキー、API にいくつかの呼び出しを実行する時間を勧めします。