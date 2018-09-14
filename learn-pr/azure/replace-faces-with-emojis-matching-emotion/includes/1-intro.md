TheMojifier は、Slack_スラッシュ_よう自分の感情に一致する絵文字をイメージに含まれる peoples 面に代わるコマンド。

![例の画像](/media-drafts/example-mojify-image.png)

カスタム コマンドとして Slack から作業になっています。 ことができますような名前のコマンドは、どのようながというこのモジュールに`mojify`します。

コマンドを実行するには、次のように入力します`/mojify <image to mojify>`:。

![例の画像](/media-drafts/9.slack-type-mojify.png)

Mojifier を行います。

  1.  イメージ内の任意のユーザーの感情を計算します。
  2.  絵文字を一致の感情
  3.  イメージ内の顔に絵文字を置き換えます
  4.  応答として更新されたイメージを Slack に投稿します。

TypeScript となど、いくつかの Azure テクノロジを使用して、Mojifier が書き込まれる[Azure Functions](https://azure.microsoft.com/services/functions/)と[Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)します。 これらの独自のバージョンを作成に使用する_TheMojifier_します。 

> [!NOTE] 
> Mojifier のすべてのコードは[GitHub](https://github.com/microsoftdocs/mslearn-the-mojifier)します。

## <a name="tools-youll-use"></a>使用するツール

### <a name="azure-cognitive-services"></a>Azure Cognitive Services

Azure Cognitive Services では、高レベルの Api をアプリに高度な人工知能 (AI) 機能をすばやく追加する際のセットです。 HTTP 要求を作成する方法がわかっている場合、はず Cognitive Services を使用します。

### <a name="azure-functions"></a>Azure Functions

イベントまたは HTTP 要求に応答できるコード スニペットをホストする azure の機能できます。 Azure は、スケーリングの問題を自動的に処理を使用したのみ支払います。 として Microsoft の学習のあらゆるものを説明します、コストを learning 環境で。
