<span data-ttu-id="9c4ed-101">絵文字の画像を Face API に渡しても、感情を取得することはできません。Face API は人間ではないからです。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-101">We can’t pass an image of the emoji to the Face API to get it’s emotion because, well because it’s not human.</span></span> <span data-ttu-id="9c4ed-102">そのため、絵文字ごとに代理の人間、つまり私が必要でした。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-102">So for each emoji, I needed a human proxy, me.</span></span>

<span data-ttu-id="9c4ed-103">私は、各絵文字を "_正確に_" まねしながら、自分の写真を撮りました。そしてその画像の "_感情ポイント_" を絵文字の代用品として使用しました。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-103">I took pictures of myself _accurately_ mimicking each emoji, and used the _emotional point_ for that image as the proxy for the emoji.</span></span> <span data-ttu-id="9c4ed-104">さらにおもしろくするために、チームからも人を選んで、次のように絵文字に関連付けました。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-104">To keep things interesting I also chose people from my team and associated them with emojis as well, like so:</span></span>

![チームの文字](/media-drafts/team.jpg)

<span data-ttu-id="9c4ed-106">目がハートになっている絵文字 (😍) には、私の妻の写真を選びました ❤️。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-106">For the emoji of love eyes (😍) I chose a picture of my wife ❤️.</span></span> <span data-ttu-id="9c4ed-107">[スティーブン ホーキング](https://en.wikipedia.org/wiki/Stephen_Hawking)を追悼して、🤔 を表すのは彼の写真にしました。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-107">In memory of [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) I picked a picture of him to represent 🤔.</span></span>

<span data-ttu-id="9c4ed-108">各絵文字の代理画像の一覧は、このチュートリアルに関連付けられているサンプル コードの `bin/proxy-images` フォルダーで見ることができます。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-108">You can see the list of proxy images for each emoji in the `bin/proxy-images` folder in the sample code associated with this tutorial.</span></span>

<span data-ttu-id="9c4ed-109">この章では、Azure Face API を使用できるようにキーを生成してから、Face API を使用して、私の代用画像を使いながらすべての絵文字を調整します。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-109">In this chapter you will generate a key so you can use the Azure Face API and then use the Face API to calibrate all the emojies using proxied images of me.</span></span>

## <a name="generate-an-azure-face-api-key"></a><span data-ttu-id="9c4ed-110">Azure Face API のキーを生成する</span><span class="sxs-lookup"><span data-stu-id="9c4ed-110">Generate an Azure Face API Key</span></span>

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

<span data-ttu-id="9c4ed-111">Azure Face API を使用するには、特殊な認証キーが必要です。 https://azure.microsoft.com/try/cognitive-services/ に移動して、Face API の試用版にサインアップします。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-111">To use the Azure Face API we need a special authentication key, head over to https://azure.microsoft.com/try/cognitive-services/ and signup to trial the Face API.</span></span>

![チームの文字](/media-drafts/4.calibrating-emojis.get-face-api.png)

> <span data-ttu-id="9c4ed-113">TODO: faceAPI を作成してキーを取得するための az コマンドを見つける</span><span class="sxs-lookup"><span data-stu-id="9c4ed-113">TODO: Find az commands to create faceAPI and grab keys</span></span>

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a><span data-ttu-id="9c4ed-114">環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="9c4ed-114">Setup the environment variables</span></span>

<span data-ttu-id="9c4ed-115">調整スクリプトで正しい呼び出しを行うには、Face API の URL とキーを指定する必要があります。それらをスクリプトにハードコーディングするのではなく、環境変数を使用して、アプリケーションを実行することになるターミナルで以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-115">The calibration script needs to know your Face API URL and Key in order to make the correct calls, rather than hardcoding these in the script we are going to use environment varialbes, run these commands in the terminal you expect to run the application in:</span></span>

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a><span data-ttu-id="9c4ed-116">絵文字のいくつかの代理画像を作成する</span><span class="sxs-lookup"><span data-stu-id="9c4ed-116">Create some proxy images for emojis</span></span>

<span data-ttu-id="9c4ed-117">すべての代理画像を用意してありますが、独自に画像を生成してもかまいません。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-117">I've provided all the proxy images myself, but feel free to generate your own!</span></span>

<span data-ttu-id="9c4ed-118">`bin/proxy-images` フォルダー内の絵文字ごとに、絵文字をまねして自分の写真を撮り、その画像で元の画像を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-118">For each emoji in the `bin/proxy-images` folder, take a picture of yourself mimicking that emoji and replace the image with your image.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="9c4ed-119">試してみる</span><span class="sxs-lookup"><span data-stu-id="9c4ed-119">Try it out</span></span>

<span data-ttu-id="9c4ed-120">ここがおもしろいところです。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-120">Now comes the fun part!</span></span> <span data-ttu-id="9c4ed-121">Face API を使用して `bin/proxy-images` の各画像を実行し、"_感情空間_" でのその絵文字の感情ポイントを計算します。次のように実行します。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-121">We are going to run each of the images in the `bin/proxy-images` through the Face API to calculate an emotional point for that emoji in _emotional space_, run:</span></span>

```bash
node bin/calibrate.js
```

<span data-ttu-id="9c4ed-122">このコマンドの出力は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-122">The output of this command should look something like so:</span></span>

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

<span data-ttu-id="9c4ed-123">最初に、処理している絵文字を出力し、最後に、すべての絵文字の `EmotivePoint` を定義する配列をコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-123">It will first print out the emoji's it is processing and then finally print out to the console an array which defines the `EmotivePoint` of all the emoji's.</span></span> <span data-ttu-id="9c4ed-124">これは、`shared/mojis.ts` の配列と同じ形式です。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-124">This is the same format as the array in `shared/mojis.ts`.</span></span>

<span data-ttu-id="9c4ed-125">代理画像の一部を変更した場合は、このスクリプトの出力を `mojis.ts` の該当するセクションにコピーします。</span><span class="sxs-lookup"><span data-stu-id="9c4ed-125">If you changed some of the proxied images then copy the output of this script to the relevant section of `mojis.ts`</span></span>
