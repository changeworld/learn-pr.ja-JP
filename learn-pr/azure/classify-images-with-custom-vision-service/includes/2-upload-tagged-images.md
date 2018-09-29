<span data-ttu-id="2ff83-101">このユニットでは、Artworks プロジェクトにピカソ、ポロック、レンブラントの有名な絵の画像を追加します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-101">In this unit, you'll add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project.</span></span> <span data-ttu-id="2ff83-102">Custom Vision Service がある画家を別の画家から区別することを学習できるように、画像にタグを付けます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-102">You'll tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>

1. <span data-ttu-id="2ff83-103">作成した **Artworks** プロジェクトで、サイド パネルの **[タグ]** の右側にあるプラス記号 **[+]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-103">In the **Artworks** project that we created, select the plus sign (**+**) to the right of **Tags** in the side panel.</span></span>

     ![タグ追加ボタンの選択](../media/2-add-tags.png)

1. <span data-ttu-id="2ff83-105">**[Name the tag]\(タグの名前付け\)** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-105">A dialog called **Name the tag** is displayed.</span></span> <span data-ttu-id="2ff83-106">タグ名フィールドに「*絵画*」と入力して、**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-106">Type *painting* into the tag name field and select **Save**.</span></span> <span data-ttu-id="2ff83-107">この操作により、タグ リストに "*絵画*" タグが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-107">This operation creates the tag *painting* in the tag list.</span></span> <span data-ttu-id="2ff83-108">さらに追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2ff83-108">Let's add some more.</span></span> 

1. <span data-ttu-id="2ff83-109">手順 2 を繰り返し、値が "*ピカソ*"、"*ポロック*"、"*レンブラント*" のタグを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-109">Repeat step 2 to add tags with the values *Picasso*, *Pollock*, and *Rembrandt*.</span></span> <span data-ttu-id="2ff83-110">完了すると、タグ リストは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2ff83-110">The tag list will look like the following when you are finished.</span></span>

    ![タグ追加ボタンの選択](../media/2-tag-list.png)

    <span data-ttu-id="2ff83-112">ご覧のように、プロジェクト内の画像のうちこれらの各タグでタグ付けされたものの数は 0 です。</span><span class="sxs-lookup"><span data-stu-id="2ff83-112">As you can see, the number of images in our project that are tagged with each of these tags is zero.</span></span> <span data-ttu-id="2ff83-113">プロジェクトに画像をいくつか追加してタグを割り当ててみましょう。</span><span class="sxs-lookup"><span data-stu-id="2ff83-113">Let's add some images to our project and assign tags.</span></span>

1. <span data-ttu-id="2ff83-114">このモジュールの画像リソースが格納されている [cvs resources.zip](https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip) をダウンロードし、ローカル コンピューターに解凍します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-114">Download [cvs-resources.zip](https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip) which contains image resources for this module and unzip it to your local machine.</span></span> 

1. <span data-ttu-id="2ff83-115">ポータルに戻り、**[Add images]\(画像の追加\)** を選択して、プロジェクトに画像を追加します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-115">Back in the portal, select **Add images** to add images to the project.</span></span>

    ![Artworks プロジェクトへの画像の追加](../media/2-portal-click-add-images.png)

1. <span data-ttu-id="2ff83-117">手順 4 でローカルにダウンロードした **cvs-resources** フォルダーで、"Artists\Picasso" フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-117">In the **cvs-resources** folder that you downloaded locally in step 4, navigate to the "Artists\Picasso" folder.</span></span>

1. <span data-ttu-id="2ff83-118">"Artists\Picasso" 内のすべてのファイルを選択し、**[開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-118">Select all of the files in "Artists\Picasso", and then select **Open**.</span></span>

    ![画像の選択](../media/2-fe-browse-picasso-01.png)

1. <span data-ttu-id="2ff83-120">**[Image upload]\(画像のアップロード\)** ダイアログが開き、アップロードしたすべての画像のサムネイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-120">The **Image upload** dialog appears and displays thumbnails of all the images we are uploading.</span></span> <span data-ttu-id="2ff83-121">**[My Tags]\(マイ タグ\)** フィールドを選択すると、これらの画像を割り当てることができるタグのドロップダウンが開きます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-121">Select the **My Tags** field, which opens a dropdown of the tags you can assign these images.</span></span> 

    ![画像への "絵画" タグの追加](../media/2-upload-picasso-tags.png)

1. <span data-ttu-id="2ff83-123">"*絵画*" タグと "*ピカソ*" タグを選択し、**[Upload 7 files]\(7 個のファイルをアップロード\)** を選択してアップロードを終了します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-123">Select the tags *painting* and *Picasso* and then select **Upload 7 files** to finish the upload.</span></span> 

1. <span data-ttu-id="2ff83-124">アップロードした画像が Artworks プロジェクトに含まれること、およびタグ リストが更新されて "*ピカソ*" と "*絵画*" タグが 7 個の画像に付けられたことが示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-124">Confirm that the images you uploaded are now in the Artworks project, and that the tag list has been updated to show that we've tagged seven images with *Picasso* and *painting*.</span></span>

    ![アップロードされた画像](../media/2-portal-tagged-01.png)

1. <span data-ttu-id="2ff83-126">ピカソの 7 つの画像を使用して、Custom Vision Service はピカソの絵を適切に識別できます。</span><span class="sxs-lookup"><span data-stu-id="2ff83-126">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="2ff83-127">しかし、今すぐモデルをトレーニングした場合、モデルはピカソの絵がどのようなものかだけを理解し、他の画家の絵画を識別することはできません。</span><span class="sxs-lookup"><span data-stu-id="2ff83-127">But if you trained the model right now, it would only understand what a Picasso looks like, and it wouldn't be able to identify paintings by other artists.</span></span> <span data-ttu-id="2ff83-128">次の手順では、別の画家の絵画をいくつかアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2ff83-128">The next step is to upload some paintings by another artist.</span></span> 

1. <span data-ttu-id="2ff83-129">**[Add images]\(画像の追加\)** を選択して、モジュール リソースの "Artists\Rembrandt" フォルダーにあるすべての画像を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-129">Select **Add images** and select all of the images in the "Artists\Rembrandt" folder in the module resources.</span></span> <span data-ttu-id="2ff83-130">それらに "絵画" と "レンブラント" ("ピカソ" ではなく) というタグを付け、**[Upload 6 files]\(6 個のファイルをアップロード\)** を選択してプロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2ff83-130">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and select **Upload 6 files** to upload them to the project.</span></span>

    ![アップロードされた画像](../media/2-upload-rembrandt.png)

1. <span data-ttu-id="2ff83-132">プロジェクトにピカソの画像と共にレンブラントの画像が表示され、タグの一覧に "レンブラント" と表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ff83-132">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![ピカソとレンブラントの画像](../media/2-portal-tagged-02.png)

1. <span data-ttu-id="2ff83-134">次に、謎めいた画家であるジャクソン ポロックの絵画を追加し、Custom Vision Service がポロックの絵も認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ff83-134">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="2ff83-135">モジュール リソースの "Artists\Pollock" フォルダーですべての画像を選択し、"絵画" と "ポロック" というタグを付けて、プロジェクトにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2ff83-135">Select all of the images in the "Artists\Pollock" folder in the module resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="2ff83-136">タグを付けた画像をアップロードしたら、次に、これらの画像でモデルをトレーニングし、モデルがピカソとレンブラントとポロックの絵を区別したり、絵がこれらの有名な画家のいずれかの作品であるかどうかを判別したりできるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ff83-136">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>