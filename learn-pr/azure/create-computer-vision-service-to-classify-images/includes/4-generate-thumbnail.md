<span data-ttu-id="c27a1-101">製品の現場の販売業者は自分が補充している店の棚の画像をスキャンし、アップロードします。</span><span class="sxs-lookup"><span data-stu-id="c27a1-101">Frontline distributors of your products scan and upload images of store shelves they're restocking.</span></span> <span data-ttu-id="c27a1-102">会社の開発主任であるあなたは、その画像のサムネイル作成を担当します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-102">As a lead developer at your company, you are responsible for creating thumbnails of the images.</span></span> <span data-ttu-id="c27a1-103">営業チームのために作成するオンライン レポートでそのサムネイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-103">The thumbnails are used in the online reports you create for the sales team.</span></span> <span data-ttu-id="c27a1-104">最近、レポートの画像がぼやけている、製品が正面中央から撮影されていないことが多い、大きなレポートをスキャンするのが難しいと営業マネージャーに言われました。</span><span class="sxs-lookup"><span data-stu-id="c27a1-104">Recently, the sales manager said that the images in the report are blurry and often don't have the product front and center, making it difficult to scan the large report.</span></span> <span data-ttu-id="c27a1-105">この状況はあなたが改善しなければなりません。</span><span class="sxs-lookup"><span data-stu-id="c27a1-105">It's up to you to improve the situation.</span></span>

<span data-ttu-id="c27a1-106">Computer Vision API のサムネイル生成機能を試すことにします。</span><span class="sxs-lookup"><span data-stu-id="c27a1-106">You decide to try the thumbnail generation feature of the Computer Vision API.</span></span> <span data-ttu-id="c27a1-107">自分で作成したサイズ変更機能よりはおそらく良い仕事をするでしょう。</span><span class="sxs-lookup"><span data-stu-id="c27a1-107">Perhaps it can do a better job than the resizing function you wrote.</span></span>

<span data-ttu-id="c27a1-108">Computer Vision では最初に高品質のサムネイルを生成し、画像内のオブジェクトを分析して関心領域 (ROI) を決定します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-108">Computer Vision first generates a high-quality thumbnail and then analyzes the objects within the image to determine the region of interest (ROI).</span></span> <span data-ttu-id="c27a1-109">Computer Vision では次に、関心領域の要件に合わせて画像がトリミングされます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-109">Computer Vision then crops the image to fit the requirements of the region of interest.</span></span> <span data-ttu-id="c27a1-110">生成されたサムネイルは、必要に応じて、元の画像とは異なる縦横比で表示できます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-110">The generated thumbnail can be presented using an aspect ratio that is different from the aspect ratio of the original image, depending on your needs.</span></span> <span data-ttu-id="c27a1-111">実際の動作を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="c27a1-111">Let's see it in action.</span></span>

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a><span data-ttu-id="c27a1-112">サムネイルを生成する Computer Vision API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="c27a1-112">Calling the Computer Vision API to generate a thumbnail</span></span>

<span data-ttu-id="c27a1-113">`generateThumbnail` 操作により、ユーザーが指定した幅と高さでサムネイル画像が作成されます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-113">The `generateThumbnail` operation creates a thumbnail image with the user-specified width and height.</span></span> <span data-ttu-id="c27a1-114">このサービスでは既定で、画像が分析され、関心領域 (ROI) が特定され、ROI に基づいてスマート トリミング座標が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-114">By default, the service analyzes the image, identifies the region of interest (ROI), and generates smart cropping coordinates based on the ROI.</span></span> <span data-ttu-id="c27a1-115">スマート トリミングは、入力画像とは異なる縦横比を指定するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="c27a1-115">Smart cropping helps when you specify an aspect ratio that differs from aspect ratio of the input image.</span></span> <span data-ttu-id="c27a1-116">要求 URL の形式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c27a1-116">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>`

<span data-ttu-id="c27a1-117">異なるパラメーターを API に与え、求められるサムネイルを生成できます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-117">Different parameters can be provided to the API to generate the proper thumbnail for your needs.</span></span> <span data-ttu-id="c27a1-118">`width` パラメーターと `height` パラメーターは必須です。</span><span class="sxs-lookup"><span data-stu-id="c27a1-118">The `width` and `height` parameters are required.</span></span> <span data-ttu-id="c27a1-119">この 2 つのパラメーターにより、特定の画像に必要なサイズが API に伝えられます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-119">They tell the API which size you need for a specific image.</span></span> <span data-ttu-id="c27a1-120">`smartCropping` パラメーターにより、画像内の関心領域を分析してサムネイル内に維持することで、よりスマートにトリミングされます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-120">The `smartCropping` parameter generates smarter cropping by analyzing the region of interest in your image to keep it within the thumbnail.</span></span> <span data-ttu-id="c27a1-121">たとえば、スマート トリミングを有効にすると、画像の縦横比が異なるとき、トリミングされたプロフィール画像では、人の顔が写真のフレーム内に収まります。</span><span class="sxs-lookup"><span data-stu-id="c27a1-121">As an example, with smart cropping enabled, a cropped profile picture would keep someone's face within the picture frame even when the picture has a different aspect ratio.</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a><span data-ttu-id="c27a1-122">サムネイルの生成</span><span class="sxs-lookup"><span data-stu-id="c27a1-122">Generate a thumbnail</span></span>

<span data-ttu-id="c27a1-123">この例では次の画像を使用しますが、同じコマンドを試すとき、他の画像の URL を自由に指定できます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-123">We'll use the following image in this example, but you're free to try the same command with URLs to other images.</span></span>

![緑の芝生に白いかわいい犬が座っている写真。](../media/4-dog.png)

1. <span data-ttu-id="c27a1-125">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-125">Execute the following commands in Azure Cloud Shell.</span></span> <span data-ttu-id="c27a1-126">コマンド内の `<region>` を、使用する Cognitive Services アカウントのリージョンに置き換える</span><span class="sxs-lookup"><span data-stu-id="c27a1-126">Replace `<region>` in the command with the region of your cognitive services account</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

<span data-ttu-id="c27a1-127">この例では、100x100 のサムネイルを作成するようにサービスに指示します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-127">In this example, we ask the service to create a thumbnail that is 100x100.</span></span> <span data-ttu-id="c27a1-128">スマート トリミングが有効になっています。</span><span class="sxs-lookup"><span data-stu-id="c27a1-128">Smart cropping is enabled.</span></span> <span data-ttu-id="c27a1-129">正しい応答にはサムネイル画像のバイナリが含まれます。thumbnail.jpg と呼ばれているファイルにそれを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-129">A successful response contains the thumbnail image binary, which we write to a file called thumbnail.jpg.</span></span>

> [!CAUTION]
> <span data-ttu-id="c27a1-130">`-o` パラメーターにより、出力がファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-130">The `-o` parameter redirects the output to a file.</span></span> <span data-ttu-id="c27a1-131">このファイルは常に上書きされます。そのため、このコマンドを試す間、複数のサムネイルを手元に置くならファイル名を変更してください。</span><span class="sxs-lookup"><span data-stu-id="c27a1-131">The file is always overwritten, so if you want to keep around  more than one thumbnail while trying this command, change the file name.</span></span>

## <a name="view-the-generated-thumbnail"></a><span data-ttu-id="c27a1-132">生成されたサムネイルを表示する</span><span class="sxs-lookup"><span data-stu-id="c27a1-132">View the generated thumbnail</span></span>

<span data-ttu-id="c27a1-133">生成されたサムネイルは、Azure Cloud Shell ストレージ アカウントに置かれます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-133">The generated thumbnail will be found in your Azure Cloud Shell storage account.</span></span> <span data-ttu-id="c27a1-134">このファイルには **thumbnail.jpg** という名前を付けました。</span><span class="sxs-lookup"><span data-stu-id="c27a1-134">We named the file **thumbnail.jpg**.</span></span>

<span data-ttu-id="c27a1-135">Microsoft Learn の Cloud Shell にはファイルをダウンロードする機能はありませんが、Azure portal からサムネイルをダウンロードするには、この手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="c27a1-135">The Cloud Shell in Microsoft Learn doesn't have the ability to download files, but you can follow these instructions to download the thumbnail through the Azure portal.</span></span>

1. <span data-ttu-id="c27a1-136">**thumbnail.jpg** ファイルがホーム フォルダーに置かれていることを確認するには、Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-136">Execute the following commands in Azure Cloud Shell to confirm that the file **thumbnail.jpg** exists in your home folder.</span></span>

    ```azurecli
    cd ~
    ls -l
    ```



1. <span data-ttu-id="c27a1-137">次のコマンドを実行して、`thumbnail.jpg` を clouddrive フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-137">Execute the following command to move `thumbnail.jpg` into the clouddrive folder.</span></span>

    ```azurecli
    mv ~/thumbnail.jpg ~/clouddrive
    ```
1. <span data-ttu-id="c27a1-138">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="c27a1-138">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>
1. <span data-ttu-id="c27a1-139">ポータル ダッシュ ボードの **[すべてのリソース]** パネルで、 名前が `cloudshell` で始まるストレージ アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-139">In the **All resources** panel of the portal dashboard, select the storage account with name beginning with `cloudshell`.</span></span>
1. <span data-ttu-id="c27a1-140">[ストレージ アカウント] パネルで、**[Storage Explorer]**、**[FILE SHARES]** を選択し、名前が **cloudshellfiles**\* で始まるそのコレクション内の共有ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-140">In the Storage account panel, select **Storage Explorer**, then **FILE SHARES** and then the file share in that collection with the name beginning with \*\*cloudshellfiles\*\*\*.</span></span>
1. <span data-ttu-id="c27a1-141">*thunbnail.jpg* ファイルを選択し、上部メニューから**ダウンロード**して画像を参照します。</span><span class="sxs-lookup"><span data-stu-id="c27a1-141">Select the *thunbnail.jpg* file and then **Download** from the top menu to see the image.</span></span>

<span data-ttu-id="c27a1-142">`generateThumbnail` 操作は高性能なサムネイル生成機能であり、サムネイルで画像の関心領域 (ROI) を維持できます。</span><span class="sxs-lookup"><span data-stu-id="c27a1-142">The `generateThumbnail` operation is a powerful thumbnail generator that is capable of keeping the Region Of Interest (ROI) of an image in the thumbnail.</span></span>

<span data-ttu-id="c27a1-143">`generateThumbnails` 操作の詳細については、「[Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)」 (サムネイルの取得) リファレンス ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c27a1-143">For more information about the `generateThumbnails` operation, see the [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) reference documentation.</span></span>