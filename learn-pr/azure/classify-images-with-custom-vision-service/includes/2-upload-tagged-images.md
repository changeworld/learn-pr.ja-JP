### <a name="exercise-2-upload-tagged-images"></a><span data-ttu-id="b56b0-101">演習 2: タグを付けた画像をアップロードする</span><span class="sxs-lookup"><span data-stu-id="b56b0-101">Exercise 2: Upload tagged images</span></span>

<span data-ttu-id="b56b0-102">この演習では、ピカソ、ポロック、レンブラントの有名な絵画の画像を Artworks プロジェクトに追加し、Custom Vision Service が画家の区別を学習できるように画像にタグを付けます。</span><span class="sxs-lookup"><span data-stu-id="b56b0-102">In this exercise, you will add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project, and tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>
  
1. <span data-ttu-id="b56b0-103">**[Add images]\(画像の追加\)** をクリックして画像をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-103">Click **Add images** to add images to the project.</span></span>

    ![Artworks プロジェクトへの画像の追加](../images/portal-click-add-images.png)

    <span data-ttu-id="b56b0-105">"Artworks プロジェクトへの画像の追加"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-105">_Adding images to the Artworks project_</span></span> 
 
1. <span data-ttu-id="b56b0-106">**[Browse local files]\(ローカル ファイルの参照\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-106">Click **Browse local files**.</span></span>

    ![ローカル画像の参照](../images/portal-click-browse-local-files.png)

    <span data-ttu-id="b56b0-108">"ローカル画像の参照"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-108">_Browsing for local images_</span></span> 
 
1. <span data-ttu-id="b56b0-109">[このラボに付属するリソース](https://a4r.blob.core.windows.net/public/cvs-resources.zip)の "Artists\Picasso" フォルダーを参照し、フォルダー内のすべてのファイルを選択して、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-109">Browse to the "Artists\Picasso" folder in the [resources that accompany this lab](https://a4r.blob.core.windows.net/public/cvs-resources.zip), select all of the files in the folder, and click **Open**.</span></span>

    ![画像の選択](../images/fe-browse-picasso-01.png)

    <span data-ttu-id="b56b0-111">"画像の選択"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-111">_Selecting an image_</span></span> 
 
1. <span data-ttu-id="b56b0-112">**[Add some tags...]\(タグの追加...\)** ボックスに「絵画」と入力します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-112">Type "painting" (without quotation marks) into the **Add some tags...** box.</span></span> <span data-ttu-id="b56b0-113">**[+]** をクリックして画像にタグを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="b56b0-113">Then click **+** to assign the tag to the images.</span></span>

    ![画像への "絵画" タグの追加](../images/portal-add-tags-01.png)

    <span data-ttu-id="b56b0-115">"画像への "絵画" タグの追加"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-115">_Adding a "painting" tag to the images_</span></span> 

1. <span data-ttu-id="b56b0-116">手順 4 を繰り返し、"ピカソ" タグを画像に追加します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-116">Repeat Step 4 to add a "Picasso" tag to the images.</span></span>

1. <span data-ttu-id="b56b0-117">**[Upload 7 files]\(7 ファイルをアップロード\)** をクリックして、画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-117">Click **Upload 7 files** to upload the images.</span></span> <span data-ttu-id="b56b0-118">アップロードが終わったら、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-118">Once the upload has completed, click **Done**.</span></span>

    ![タグを付けた画像のアップロード](../images/upload-picasso-images.png)

    <span data-ttu-id="b56b0-120">"タグを付けた画像のアップロード"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-120">_Uploading tagged images_</span></span> 

1. <span data-ttu-id="b56b0-121">アップロードした画像が、割り当てたタグと共にポータルに表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-121">Confirm that the images you uploaded appear in the portal, along with the tags assigned to them.</span></span>

    ![アップロードされた画像](../images/portal-tagged-01.png)

    <span data-ttu-id="b56b0-123">"アップロードされた画像"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-123">_The uploaded images_</span></span> 

1. <span data-ttu-id="b56b0-124">ピカソの 7 つの画像を使用して、Custom Vision Service はピカソの絵を適切に識別できます。</span><span class="sxs-lookup"><span data-stu-id="b56b0-124">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="b56b0-125">しかし、今すぐモデルをトレーニングした場合、モデルはピカソの絵がどのようなものかだけを理解し、他の画家の絵画を識別することはできません。</span><span class="sxs-lookup"><span data-stu-id="b56b0-125">But if you trained the model right now, it would only understand what a Picasso looks like, and it would not be able to identify paintings by other artists.</span></span>

    <span data-ttu-id="b56b0-126">次の手順では、別の画家の絵画をいくつかアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-126">The next step is to upload some paintings by another artist.</span></span> <span data-ttu-id="b56b0-127">**[Add images]\(画像の追加\)** をクリックして、ラボ リソースの "Artists\Rembrandt" フォルダーにあるすべての画像を選択します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-127">Click **Add images** and select all of the images in the "Artists\Rembrandt" folder in the lab resources.</span></span> <span data-ttu-id="b56b0-128">それらに "絵画" と "レンブラント" ("ピカソ" ではなく) というタグを付けて、プロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-128">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and upload them to the project.</span></span>

    > <span data-ttu-id="b56b0-129">"絵画" タグを追加するとき、再度入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b56b0-129">When you add the tag "painting," you don't have to type it in again.</span></span> <span data-ttu-id="b56b0-130">次に示すように、**[Add some tags...]\(タグの追加...\)** ボックスのドロップダウン リストから選択できます。</span><span class="sxs-lookup"><span data-stu-id="b56b0-130">You can select it from the drop-down list attached to the **Add some tags...** box, as shown below.</span></span> <span data-ttu-id="b56b0-131">"レンブラント" タグを追加するには、「レンブラント」と**入力**して、**[+]** をクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b56b0-131">You **will** have to type "Rembrandt" and click **+** to add a "Rembrandt" tag.</span></span>

    ![既存のタグの選択](../images/select-painting-tag.png)

    <span data-ttu-id="b56b0-133">"既存のタグの選択"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-133">_Selecting an existing tag_</span></span> 

1. <span data-ttu-id="b56b0-134">プロジェクトにピカソの画像と共にレンブラントの画像が表示され、タグの一覧に "レンブラント" と表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b56b0-134">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![ピカソとレンブラントの画像](../images/portal-tagged-02.png)

    <span data-ttu-id="b56b0-136">"ピカソとレンブラントの画像"__</span><span class="sxs-lookup"><span data-stu-id="b56b0-136">_Picasso and Rembrandt images_</span></span> 

1. <span data-ttu-id="b56b0-137">次に、謎めいた画家であるジャクソン ポロックの絵画を追加し、Custom Vision Service がポロックの絵も認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-137">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="b56b0-138">ラボ リソースの "Artists\Pollock" フォルダーですべての画像を選択し、"絵画" と "ポロック" というタグを付けて、プロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-138">Select all of the images in the "Artists\Pollock" folder in the lab resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="b56b0-139">タグを付けた画像をアップロードしたら、次に、これらの画像でモデルをトレーニングし、モデルがピカソとレンブラントとポロックの絵を区別したり、絵がこれらの有名な画家のいずれかの作品であるかどうかを判別したりできるようにします。</span><span class="sxs-lookup"><span data-stu-id="b56b0-139">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>