<span data-ttu-id="5f12b-101">このユニットでは、前に作成した Computer Vision API サービスを使用してソース画像からサムネイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-101">In this unit, you will generate thumbnails from a source image with the Computer Vision API service that we created previously.</span></span>

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a><span data-ttu-id="5f12b-102">Computer Vision API で画像からサムネイルを生成する</span><span class="sxs-lookup"><span data-stu-id="5f12b-102">Generate a thumbnail from an image with Computer Vision API</span></span>

<span data-ttu-id="5f12b-103">`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="5f12b-104">そのコマンドの出力を `key` 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="5f12b-105">`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="5f12b-106">異なるパラメーターを API に提供し、ニーズに応じた適切なサムネイルを生成できます。</span><span class="sxs-lookup"><span data-stu-id="5f12b-106">Different parameters can be provided to the API to generate the proper thumbnail for your needs.</span></span> <span data-ttu-id="5f12b-107">`width` と `height` は必須であり、特定の画像に必要なサイズを API に通知します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-107">`width` and `height` are required and will tell the API which size you need for a specific image.</span></span> <span data-ttu-id="5f12b-108">最後に、`smartCropping` パラメーターを指定すると、画像内の関心領域を分析してサムネイル内にそれらを維持することで、よりスマートなトリミングが生成されます。</span><span class="sxs-lookup"><span data-stu-id="5f12b-108">Finally, the `smartCropping` parameter generates smarter cropping by analyzing the region of interest in your image to keep it within the thumbnail.</span></span> <span data-ttu-id="5f12b-109">たとえば、スマート トリミングを有効にすると、画像が要求したものと同じ比率ではない場合でも、トリミングされたプロフィール画像では、画像フレーム内の人の顔が保たれます。</span><span class="sxs-lookup"><span data-stu-id="5f12b-109">As an example, with smart cropping enabled, a cropped profile picture would keep someone's face within the picture frame even when the picture isn't in the same ratio as the one that we asked.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a><span data-ttu-id="5f12b-110">サムネイルのダウンロード</span><span class="sxs-lookup"><span data-stu-id="5f12b-110">Downloading the thumbnail</span></span>

<span data-ttu-id="5f12b-111">生成されたサムネイルは、`cloud-shell-storage-<region>` という名前のリソース グループ内の Azure Cloud Shell ストレージ アカウントにあります。</span><span class="sxs-lookup"><span data-stu-id="5f12b-111">The generated thumbnail will be found in your Azure Cloud Shell storage account within a resource group named `cloud-shell-storage-<region>`.</span></span>

1. <span data-ttu-id="5f12b-112">自動的に生成されたストレージ アカウントに移動します。</span><span class="sxs-lookup"><span data-stu-id="5f12b-112">Get into the automatically generated storage account.</span></span>

![画像](../images/storage-account.png)

2. <span data-ttu-id="5f12b-114">ファイル セクションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5f12b-114">Click on the files section.</span></span>

![画像](../images/storage-account-click-on-files.png)

3. <span data-ttu-id="5f12b-116">コンテナーのルートにサムネイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5f12b-116">You will find the thumbnail at the root of the container.</span></span>

![画像](../images/storage-account-thumbnail.png)

4. <span data-ttu-id="5f12b-118">ファイルをクリックしてダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="5f12b-118">Click on the file, and then download it.</span></span>

<span data-ttu-id="5f12b-119">ダウンロード フォルダーから、画像ビューアーで `100x100` ピクセルの画像を開くことができます。</span><span class="sxs-lookup"><span data-stu-id="5f12b-119">From within your download folder, you can open the `100x100`-pixels image with any image viewer.</span></span>