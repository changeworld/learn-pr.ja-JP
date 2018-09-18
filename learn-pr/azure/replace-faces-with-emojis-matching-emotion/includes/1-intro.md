<span data-ttu-id="ace1d-101">TheMojifier は、画像に含まれる人の顔を感情と一致する絵文字に置き換える、Slack の "_スラッシュ_" コマンドです。たとえば、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ace1d-101">TheMojifier is a Slack _slash_ command which replaces peoples faces in images with emojis matching their emotion, like so:</span></span>

![画像の例](/media-drafts/example-mojify-image.png)

<span data-ttu-id="ace1d-103">Slack からカスタム コマンドとして機能するように設計されており、コマンド名は自由に変更できますが、このドキュメントでは `mojify` という名前にしています。</span><span class="sxs-lookup"><span data-stu-id="ace1d-103">It's designed to work from Slack as a custom command, you can name the command how you want, for this document I've named it `mojify`.</span></span>

<span data-ttu-id="ace1d-104">コマンドを実行するには、「`/mojify <image to mojify>`」のように入力します。</span><span class="sxs-lookup"><span data-stu-id="ace1d-104">To execute the commmand type `/mojify <image to mojify>`, like so:</span></span>

![画像の例](/media-drafts/9.slack-type-mojify.png)

<span data-ttu-id="ace1d-106">mojifier は、以下の処理を行います。</span><span class="sxs-lookup"><span data-stu-id="ace1d-106">The mojifier then:</span></span>

1.  <span data-ttu-id="ace1d-107">画像内の人々の感情を計算します。</span><span class="sxs-lookup"><span data-stu-id="ace1d-107">Calculates the emotion of any people in the image.</span></span>
2.  <span data-ttu-id="ace1d-108">感情と絵文字を一致させます。</span><span class="sxs-lookup"><span data-stu-id="ace1d-108">Matches emotions to emojis.</span></span>
3.  <span data-ttu-id="ace1d-109">顔を絵文字に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ace1d-109">Replaces the faces with emojis.</span></span>
4.  <span data-ttu-id="ace1d-110">画像を Twitter に返信として投稿します。</span><span class="sxs-lookup"><span data-stu-id="ace1d-110">Posts the image back to Twitter as a reply.</span></span>

<span data-ttu-id="ace1d-111">これは、TypeScript のほかに、[Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)、[Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai) など、いくつかの Azure テクノロジを使用して記述されています</span><span class="sxs-lookup"><span data-stu-id="ace1d-111">It’s written using TypeScript and several Azure technologies including [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) and [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)</span></span>

<span data-ttu-id="ace1d-112">このチュートリアルでは、TheMojifier がどのような方法で作成されたかを説明し、Azure テクノロジを使用して独自の Slack コマンドを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ace1d-112">In this tutorial I’m going to explain how TheMojifier was made and show you how to create your own Slack command using Azure technologies.</span></span>

> <span data-ttu-id="ace1d-113">TODO、これはどうなりますか。</span><span class="sxs-lookup"><span data-stu-id="ace1d-113">TODO, where will this be now?</span></span>
> <span data-ttu-id="ace1d-114">Mojifier のすべてのコードは、[GitHub](https://github.com/jawache/mojifier) で入手できます</span><span class="sxs-lookup"><span data-stu-id="ace1d-114">All the code for Mojifier is available on [GitHub](https://github.com/jawache/mojifier)</span></span>

# <a name="requirements"></a><span data-ttu-id="ace1d-115">要件</span><span class="sxs-lookup"><span data-stu-id="ace1d-115">Requirements</span></span>

<span data-ttu-id="ace1d-116">mojifier をビルドするには、いくつかの Azure サービスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ace1d-116">To build the mojifier, we need to use several Azure services.</span></span>

## <a name="azure-cognitive-services"></a><span data-ttu-id="ace1d-117">Azure Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="ace1d-117">Azure Cognitive Services</span></span>

<span data-ttu-id="ace1d-118">Azure Cognitive Services は、高度な AI 機能をアプリケーションにすばやく追加する際に使用できる、高レベルの API のセットです。</span><span class="sxs-lookup"><span data-stu-id="ace1d-118">Azure Cognitive Services are a set of high-level APIs you can use to add advanced AI functionality into your application quickly.</span></span> <span data-ttu-id="ace1d-119">HTTP 要求を行うことができる場合は、Cognitive Services を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ace1d-119">If you can make an HTTP request, you can use Cognitive Services.</span></span>

[<span data-ttu-id="ace1d-120">詳細情報</span><span class="sxs-lookup"><span data-stu-id="ace1d-120">More info</span></span>](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a><span data-ttu-id="ace1d-121">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="ace1d-121">Azure Functions</span></span>

<span data-ttu-id="ace1d-122">Logic Apps ほど強力なものになると、プログラミング言語の表現力をフルに活用してビジネス ロジックを記述しなければならないことがあります。</span><span class="sxs-lookup"><span data-stu-id="ace1d-122">As powerful as Logic Apps are sometimes you need to write business logic using the full expressiveness of a programming language.</span></span> <span data-ttu-id="ace1d-123">Azure Functions は、イベントや HTTP 要求に応答できるコードのスニペットをホストできるようにするテクノロジであり、Azure がスケーリングのすべての問題を処理します。料金は使用した分だけになります。</span><span class="sxs-lookup"><span data-stu-id="ace1d-123">Azure Functions is a technology that lets you host snippets of code that can respond to events or HTTP requests, Azure handles all of the scaling issues for you and you only pay for what you use.</span></span>

[<span data-ttu-id="ace1d-124">詳細情報</span><span class="sxs-lookup"><span data-stu-id="ace1d-124">More info</span></span>](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
