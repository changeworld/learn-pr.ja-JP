<span data-ttu-id="d6a79-101">このユニットでは、Artworks アプリを使用して、分類のために画像を Custom Vision Service に送信します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-101">In this unit, you will use the Artworks app to submit images to the Custom Vision Service for classification.</span></span> <span data-ttu-id="d6a79-102">このアプリは、Custom Vision Prediction API の [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) メソッドへの呼び出しから返された JSON 情報を使用して、画像がピカソ、レンブラント、ポロック、またはそのいずれでもない人物による絵画を表しているかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-102">The app uses the JSON information returned from calls to the Custom Vision Prediction API's [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) method to tell you whether an image represents a painting by Picasso, Rembrandt, Pollock, or none of the above.</span></span> <span data-ttu-id="d6a79-103">また、イメージに割り当てられた分類が正しい確率も示します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-103">It also shows the probability that the classification assigned to the image is correct.</span></span>

1. <span data-ttu-id="d6a79-104">Artworks アプリで **[参照(...)]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d6a79-104">Click the **Browse (...)** button in the Artworks app.</span></span>

    ![Artworks アプリでローカル画像を参照する](../media/6-app-click-browse.png)

1. <span data-ttu-id="d6a79-106">モジュール リソースの "Quick Tests" フォルダーを参照します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-106">Browse to the "Quick Tests" folder in the module resources.</span></span> <span data-ttu-id="d6a79-107">**PicassoTest_02.jpg** という名前のファイルを選択し、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d6a79-107">Select the file named **PicassoTest_02.jpg**, and then click **Open**.</span></span>

1. <span data-ttu-id="d6a79-108">**[予測]** ボタンをクリックして、イメージを Custom Vision Service に送信します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-108">Click the **Predict** button to submit the image to the Custom Vision Service.</span></span>

    ![画像を Custom Vision Service に送信する](../media/6-app-click-predict.png)

1. <span data-ttu-id="d6a79-110">アプリでその絵がピカソの作品として識別されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-110">Confirm that the app identifies the painting as a Picasso.</span></span>

    ![画像をピカソとして分類する](../media/6-app-prediction-01.png)

1. <span data-ttu-id="d6a79-112">**RembrandtTest_01.jpg** と **PollockTest_01.jpg** に対して手順 1. から 3. を繰り返し、アプリでレンブラントおよびポロックの絵画が識別できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-112">Repeat steps 1 through 3 for **RembrandtTest_01.jpg** and **PollockTest_01.jpg**, and confirm that the app can identify paintings by Rembrandt and Pollock.</span></span>

    ![画像をレンブラントとして分類する](../media/6-app-prediction-02.png)

1. <span data-ttu-id="d6a79-114">**VanGoghTest_01.png** と **VanGoghTest_02.png** に対して手順 1. から 3. を繰り返し、アプリでヴァン・ゴッホのこれらの名画がピカソやレンブラント、ポロックの絵画として識別されないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d6a79-114">Repeat steps 1 through 3 for **VanGoghTest_01.png** and **VanGoghTest_02.png**, and confirm that the app does not identify these Van Gogh masterworks as paintings by Picasso, Rembrandt, or Pollock.</span></span>

    ![ピカソでもレンブラントでもポロックでもない](../media/6-app-prediction-03.png)

1. <span data-ttu-id="d6a79-116">ご覧のように、アプリから Prediction API を使用すると、Custom Vision Service ポータルを使用する場合と同じくらいの信頼性が得られ、はるかに楽しく行えます。</span><span class="sxs-lookup"><span data-stu-id="d6a79-116">As you can see, using the Prediction API from an app is as reliable as it is through the Custom Vision Service portal — and way more fun!</span></span> <span data-ttu-id="d6a79-117">さらに、ポータルの予測ページに移動すると、アプリを介してアップロードした各画像もそこに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d6a79-117">What's more, if you go to the Predictions page in the portal, you'll find each of the images uploaded via the app shown there as well.</span></span>

    ![Custom Vision Service に送信された画像](../media/6-portal-all-predictions.png)

<span data-ttu-id="d6a79-119">自由にご自身の画像を使用してテストしたり、アーティストの識別や、画像がピカソ、レンブラント、またはポロックの作品ではないことの識別についてモデルの熟練度を測定したりしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="d6a79-119">Feel free to test with images of your own and gauge the model's adeptness at identifying artists or determining that an image is not a Picasso, Rembrandt, or Pollock.</span></span> <span data-ttu-id="d6a79-120">ゴッホも認識するようにトレーニングする場合は、ゴッホの絵画をいくつかアップロードして、それらに "Van Gogh" (ヴァン ゴッホ) とタグ付け、モデルを再トレーニングします。</span><span class="sxs-lookup"><span data-stu-id="d6a79-120">If you'd like to train it to recognize Van Goghs too, upload some Van Gogh paintings, tag them with "Van Gogh," and retrain the model.</span></span> <span data-ttu-id="d6a79-121">トレーニングの手間をいとわなければ、追加できるインテリジェンスに制限はありません。</span><span class="sxs-lookup"><span data-stu-id="d6a79-121">There is no limit to the intelligence you can add if you're willing to do the training.</span></span> <span data-ttu-id="d6a79-122">一般的に、トレーニングするイメージが多いほどモデルはスマートになります。</span><span class="sxs-lookup"><span data-stu-id="d6a79-122">And remember that in general, the more images you train with, the smarter the model will be.</span></span>