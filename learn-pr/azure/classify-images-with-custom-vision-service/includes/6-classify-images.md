### <a name="exercise-6-use-the-app-to-classify-images"></a><span data-ttu-id="f4333-101">演習 6: アプリを使用してイメージを分類する</span><span class="sxs-lookup"><span data-stu-id="f4333-101">Exercise 6: Use the app to classify images</span></span>

<span data-ttu-id="f4333-102">この演習では、Artworks アプリを使用して、イメージを分類のために Custom Vision Service に送信します。</span><span class="sxs-lookup"><span data-stu-id="f4333-102">In this exercise, you will use the Artworks app to submit images to the Custom Vision Service for classification.</span></span> <span data-ttu-id="f4333-103">このアプリは、Custom Vision Prediction API の [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) メソッドへの呼び出しから返された JSON 情報を使用して、イメージがピカソ、レンブラント、ポロック、またはそのいずれでもない人物による絵画を表しているかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="f4333-103">The app uses the JSON information returned from calls to the Custom Vision Prediction API's [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) method to tell you whether an image represents a painting by Picasso, Rembrandt, Pollock, or none of the above.</span></span> <span data-ttu-id="f4333-104">また、イメージに割り当てられた分類が正しい確率も示します。</span><span class="sxs-lookup"><span data-stu-id="f4333-104">It also shows the probability that the classification assigned to the image is correct.</span></span>

1. <span data-ttu-id="f4333-105">Artworks アプリで **[参照(...)]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4333-105">Click the **Browse (...)** button in the Artworks app.</span></span> 

    ![Artworks アプリでローカル イメージを参照する](../images/app-click-browse.png)

    <span data-ttu-id="f4333-107">"Artworks アプリでローカル イメージを参照する"__</span><span class="sxs-lookup"><span data-stu-id="f4333-107">_Browsing for local images in the Artworks app_</span></span> 

1. <span data-ttu-id="f4333-108">ラボ リソースの "Quick Tests" フォルダーを参照します。</span><span class="sxs-lookup"><span data-stu-id="f4333-108">Browse to the "Quick Tests" folder in the lab resources.</span></span> <span data-ttu-id="f4333-109">**PicassoTest_02.jpg** という名前のファイルを選択し、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4333-109">Select the file named **PicassoTest_02.jpg**, and then click **Open**.</span></span>

1. <span data-ttu-id="f4333-110">**[予測]** ボタンをクリックして、イメージを Custom Vision Service に送信します。</span><span class="sxs-lookup"><span data-stu-id="f4333-110">Click the **Predict** button to submit the image to the Custom Vision Service.</span></span>

    ![イメージを Custom Vision Service に送信する](../images/app-click-predict.png)

    <span data-ttu-id="f4333-112">"イメージを Custom Vision Service に送信する"__</span><span class="sxs-lookup"><span data-stu-id="f4333-112">_Submitting the image to the Custom Vision Service_</span></span> 

1. <span data-ttu-id="f4333-113">アプリでその絵がピカソの作品として識別されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f4333-113">Confirm that the app identifies the painting as a Picasso.</span></span>

    ![イメージをピカソとして分類する](../images/app-prediction-01.png)

    <span data-ttu-id="f4333-115">"イメージをピカソとして分類する"__</span><span class="sxs-lookup"><span data-stu-id="f4333-115">_Classifying an image as a Picasso_</span></span> 

1. <span data-ttu-id="f4333-116">**RembrandtTest_01.jpg** と **PollockTest_01.jpg** に対して手順 1 から 3 を繰り返して、アプリがレンブラントおよびポロックの絵画を識別できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f4333-116">Repeat steps 1 through 3 for **RembrandtTest_01.jpg** and **PollockTest_01.jpg** and confirm that the app can identify paintings by Rembrandt and Pollock.</span></span>

    ![イメージをレンブラントとして分類する](../images/app-prediction-02.png)

    <span data-ttu-id="f4333-118">"イメージをレンブラントとして分類する"__</span><span class="sxs-lookup"><span data-stu-id="f4333-118">_Classifying an image as a Rembrandt_</span></span> 

1. <span data-ttu-id="f4333-119">**VanGoghTest_01.png** と **VanGoghTest_02.png** に対して手順 1 から 3 を繰り返して、アプリがゴッホのこれらの名画をピカソやレンブラント、ポロックの絵画として識別しないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f4333-119">Repeat steps 1 through 3 for **VanGoghTest_01.png** and **VanGoghTest_02.png** and confirm that the app does not identify these Van Gogh masterworks as paintings by Picasso, Rembrandt, or Pollock.</span></span>

    ![ピカソでもレンブラントでもポロックでもない](../images/app-prediction-03.png)

    <span data-ttu-id="f4333-121">"ピカソでもレンブラントでもポロックでもない"__</span><span class="sxs-lookup"><span data-stu-id="f4333-121">_Not a Picasso, Rembrandt, or Pollock_</span></span> 

1. <span data-ttu-id="f4333-122">ご覧のように、アプリから Prediction API を使用すると、Custom Vision Service ポータルを使用する場合と同じくらいの信頼性が得られ、より楽しく行えます。</span><span class="sxs-lookup"><span data-stu-id="f4333-122">As you can see, using the Prediction API from an app is as reliable as through the Custom Vision Service portal — and way more fun!</span></span> <span data-ttu-id="f4333-123">さらに、ポータルの予測ページに移動すると、アプリを使用してアップロードされた各イメージもそこに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f4333-123">What's more, if you go to the Predictions page in the portal, you'll find each of the images uploaded via the app shown there as well.</span></span>
 
    ![Custom Vision Service に送信されたイメージ](../images/portal-all-predictions.png)

    <span data-ttu-id="f4333-125">"Custom Vision Service に送信されたイメージ"__</span><span class="sxs-lookup"><span data-stu-id="f4333-125">_image submitted to the Custom Vision Service_</span></span> 

<span data-ttu-id="f4333-126">ご自分のイメージを使用して、アーティストを識別したり、イメージがピカソ、レンブラント、またはポロックの作品ではないことの識別についてモデルの熟練度を自由にテストしたり、測定したりしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="f4333-126">Feel free to test with images of your own and gauge the model's adeptness at identifying artists or determining that an image is not a Picasso, Rembrandt, or Pollock.</span></span> <span data-ttu-id="f4333-127">ゴッホも認識するようにトレーニングする場合は、ゴッホの絵画をいくつかアップロードして、それらに "Van Gogh" (ヴァン ゴッホ) とタグ付けして、モデルを再トレーニングします。</span><span class="sxs-lookup"><span data-stu-id="f4333-127">If you'd like to train it to recognize Van Goghs, too, upload some Van Gogh paintings, tag them with "Van Gogh," and retrain the model.</span></span> <span data-ttu-id="f4333-128">トレーニングの手間をいとわなければ、追加できるインテリジェンスに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="f4333-128">There is no limit to the intelligence you can add if you're willing to do the training.</span></span> <span data-ttu-id="f4333-129">一般的に、トレーニングするイメージが多いほどモデルはスマートになります。</span><span class="sxs-lookup"><span data-stu-id="f4333-129">And remember that in general, the more images you train with, the smarter the model will be.</span></span>