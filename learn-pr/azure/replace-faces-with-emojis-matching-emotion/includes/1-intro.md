TheMojifier は、画像に含まれる人の顔を感情と一致する絵文字に置き換える、Slack の "_スラッシュ_" コマンドです。たとえば、次のようになります。

![画像の例](/media-drafts/example-mojify-image.png)

Slack からカスタム コマンドとして機能するように設計されており、コマンド名は自由に変更できますが、このドキュメントでは `mojify` という名前にしています。

コマンドを実行するには、「`/mojify <image to mojify>`」のように入力します。

![画像の例](/media-drafts/9.slack-type-mojify.png)

mojifier は、以下の処理を行います。

1.  画像内の人々の感情を計算します。
2.  感情と絵文字を一致させます。
3.  顔を絵文字に置き換えます。
4.  画像を Twitter に返信として投稿します。

これは、TypeScript のほかに、[Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)、[Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai) など、いくつかの Azure テクノロジを使用して記述されています

このチュートリアルでは、TheMojifier がどのような方法で作成されたかを説明し、Azure テクノロジを使用して独自の Slack コマンドを作成する方法を示します。

> TODO、これはどうなりますか。
> Mojifier のすべてのコードは、[GitHub](https://github.com/jawache/mojifier) で入手できます

# <a name="requirements"></a>要件

mojifier をビルドするには、いくつかの Azure サービスを使用する必要があります。

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Azure Cognitive Services は、高度な AI 機能をアプリケーションにすばやく追加する際に使用できる、高レベルの API のセットです。 HTTP 要求を行うことができる場合は、Cognitive Services を使用することができます。

[詳細情報](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a>Azure Functions

Logic Apps ほど強力なものになると、プログラミング言語の表現力をフルに活用してビジネス ロジックを記述しなければならないことがあります。 Azure Functions は、イベントや HTTP 要求に応答できるコードのスニペットをホストできるようにするテクノロジであり、Azure がスケーリングのすべての問題を処理します。料金は使用した分だけになります。

[詳細情報](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
