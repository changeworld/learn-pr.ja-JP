<span data-ttu-id="d8393-101">このユニットでは、ピカソ、ポロック、レンブラントの有名な絵画の画像を Artworks プロジェクトに追加し、Custom Vision Service が画家の区別を学習できるように画像にタグを付けます。</span><span class="sxs-lookup"><span data-stu-id="d8393-101">In this unit, you will add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project, and tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>

1. <span data-ttu-id="d8393-102">**[Add images]\(画像の追加\)** をクリックして画像をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="d8393-102">Click **Add images** to add images to the project.</span></span>

    ![Artworks プロジェクトへの画像の追加](../media/2-portal-click-add-images.png)

1. <span data-ttu-id="d8393-104">**[Browse local files]\(ローカル ファイルの参照\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8393-104">Click **Browse local files**.</span></span>

    ![ローカル画像の参照](../media/2-portal-click-browse-local-files.png)

    <span data-ttu-id="d8393-106">_"ローカル画像の参照"_</span><span class="sxs-lookup"><span data-stu-id="d8393-106">_Browsing for local images_</span></span>

1. <span data-ttu-id="d8393-107">[このモジュールに付属するリソース](https://a4r.blob.core.windows.net/public/cvs-resources.zip)の "Artists\Picasso" フォルダーを参照し、フォルダー内のすべてのファイルを選択して、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8393-107">Browse to the "Artists\Picasso" folder in the [resources that accompany this module](https://a4r.blob.core.windows.net/public/cvs-resources.zip), select all of the files in the folder, and click **Open**.</span></span>

    ![画像の選択](../media/2-fe-browse-picasso-01.png)

1. <span data-ttu-id="d8393-109">**[Add some tags...]\(タグの追加...\)** ボックスに「絵画」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d8393-109">Type "painting" (without quotation marks) into the **Add some tags...** box.</span></span> <span data-ttu-id="d8393-110">**[+]** をクリックして画像にタグを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="d8393-110">Then click **+** to assign the tag to the images.</span></span>

    ![画像への "絵画" タグの追加](../media/2-portal-add-tags-01.png)

1. <span data-ttu-id="d8393-112">手順 4 を繰り返し、"ピカソ" タグを画像に追加します。</span><span class="sxs-lookup"><span data-stu-id="d8393-112">Repeat Step 4 to add a "Picasso" tag to the images.</span></span>

1. <span data-ttu-id="d8393-113">**[Upload 7 files]\(7 ファイルをアップロード\)** をクリックして、画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="d8393-113">Click **Upload 7 files** to upload the images.</span></span> <span data-ttu-id="d8393-114">アップロードが完了したら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8393-114">Once the upload has finished, click **Done**.</span></span>

    ![タグを付けた画像のアップロード](../media/2-upload-picasso-images.png)

1. <span data-ttu-id="d8393-116">アップロードした画像が、割り当てたタグと共にポータルに表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d8393-116">Confirm that the images you uploaded appear in the portal, along with the tags assigned to them.</span></span>

    ![アップロードされた画像](../media/2-portal-tagged-01.png)

1. <span data-ttu-id="d8393-118">ピカソの 7 つの画像を使用して、Custom Vision Service はピカソの絵を適切に識別できます。</span><span class="sxs-lookup"><span data-stu-id="d8393-118">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="d8393-119">しかし、今すぐモデルをトレーニングした場合、モデルはピカソの絵がどのようなものかだけを理解し、他の画家の絵画を識別することはできません。</span><span class="sxs-lookup"><span data-stu-id="d8393-119">But if you trained the model right now, it would only understand what a Picasso looks like, and it would not be able to identify paintings by other artists.</span></span>

    <span data-ttu-id="d8393-120">次の手順では、別の画家の絵画をいくつかアップロードします。</span><span class="sxs-lookup"><span data-stu-id="d8393-120">The next step is to upload some paintings by another artist.</span></span> <span data-ttu-id="d8393-121">**[Add images]\(画像の追加\)** をクリックして、モジュール リソースの "Artists\Rembrandt" フォルダーにあるすべての画像を選択します。</span><span class="sxs-lookup"><span data-stu-id="d8393-121">Click **Add images** and select all of the images in the "Artists\Rembrandt" folder in the module resources.</span></span> <span data-ttu-id="d8393-122">それらに "絵画" と "レンブラント" ("ピカソ" ではなく) というタグを付けて、プロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="d8393-122">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and upload them to the project.</span></span>

    > <span data-ttu-id="d8393-123">"絵画" タグを追加するとき、再度入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d8393-123">When you add the tag "painting," you don't have to type it in again.</span></span> <span data-ttu-id="d8393-124">次に示すように、**[Add some tags...]\(タグの追加...\)** ボックスのドロップダウン リストから選択できます。</span><span class="sxs-lookup"><span data-stu-id="d8393-124">You can select it from the drop-down list attached to the **Add some tags...** box, as shown below.</span></span> <span data-ttu-id="d8393-125">"レンブラント" タグを追加するには、「レンブラント」と**入力**して、**[+]** をクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8393-125">You **will** have to type "Rembrandt" and click **+** to add a "Rembrandt" tag.</span></span>

    ![既存のタグの選択](../media/2-select-painting-tag.png)

1. <span data-ttu-id="d8393-127">プロジェクトにピカソの画像と共にレンブラントの画像が表示され、タグの一覧に "レンブラント" と表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d8393-127">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![ピカソとレンブラントの画像](../media/2-portal-tagged-02.png)

1. <span data-ttu-id="d8393-129">次に、謎めいた画家であるジャクソン ポロックの絵画を追加し、Custom Vision Service がポロックの絵も認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d8393-129">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="d8393-130">モジュール リソースの "Artists\Pollock" フォルダーですべての画像を選択し、"絵画" と "ポロック" というタグを付けて、プロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="d8393-130">Select all of the images in the "Artists\Pollock" folder in the module resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="d8393-131">タグを付けた画像をアップロードしたら、次に、これらの画像でモデルをトレーニングし、モデルがピカソとレンブラントとポロックの絵を区別したり、絵がこれらの有名な画家のいずれかの作品であるかどうかを判別したりできるようにします。</span><span class="sxs-lookup"><span data-stu-id="d8393-131">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>