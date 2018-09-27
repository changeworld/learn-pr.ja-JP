<span data-ttu-id="9e730-101">モデルのトレーニングが済んだので、今度はモデルをテストします。</span><span class="sxs-lookup"><span data-stu-id="9e730-101">Now that we've trained our model, it's time to test it.</span></span> <span data-ttu-id="9e730-102">モデルに新しい画像を渡して、どれくらい正しく分類されるか確認します。</span><span class="sxs-lookup"><span data-stu-id="9e730-102">We'll give the model new images and see how well it classifies it.</span></span>

1. <span data-ttu-id="9e730-103">ページの上部にある **[Quick Test]\(クイック テスト\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9e730-103">Click **Quick Test** at the top of the page.</span></span>

    ![モデルのテスト](../media/4-portal-click-quick-test.png)

1. <span data-ttu-id="9e730-105">**[ローカル ファイルを参照します]** をクリックして、前にダウンロードしたモジュール リソース フォルダー内の "Quick Tests" フォルダーを参照します。</span><span class="sxs-lookup"><span data-stu-id="9e730-105">Click **Browse local files**, and then browse to the "Quick Tests" folder in the module resources folder you download previously.</span></span> <span data-ttu-id="9e730-106">**PicassoTest_01.jpg** を選択して、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9e730-106">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![ピカソのテスト画像の選択](../media/4-portal-select-test-01.png)

1. <span data-ttu-id="9e730-108">[Quick Test]\(クイック テスト\) ダイアログでテストの結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="9e730-108">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="9e730-109">絵画がピカソである確率はどれくらいですか。</span><span class="sxs-lookup"><span data-stu-id="9e730-109">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="9e730-110">レンブラントやポロックである確率はどれくらいですか。</span><span class="sxs-lookup"><span data-stu-id="9e730-110">What is the probability that it's a Rembrandt or Pollock?</span></span>

    ![ピカソのテスト画像の選択](../media/4-quick-test-result.png)

1. <span data-ttu-id="9e730-112">[Quick Test]\(クイック テスト\) ダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="9e730-112">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="9e730-113">次に、ページの上部にある **[予測]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9e730-113">Then click **Predictions** at the top of the page.</span></span>

    ![実行されたテストの表示](../media/4-portal-select-predictions.png)

1. <span data-ttu-id="9e730-115">アップロードしたテスト画像をクリックして、その詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="9e730-115">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="9e730-116">次に、ドロップダウン リストから **[Picasso]** を選択し、**[保存して閉じる]** をクリックすることによって、イメージを "Picasso" としてタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="9e730-116">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="9e730-117">このようにテスト イメージをタグ付けすることによって、追加のトレーニング用イメージをアップロードすることなく、モデルを改良することができます。</span><span class="sxs-lookup"><span data-stu-id="9e730-117">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>

    ![テスト画像のタグ付け](../media/4-tag-test-image.png)

1. <span data-ttu-id="9e730-119">別のクイック テストを実行し、今度は [Quick Test]\(クイック テスト\) フォルダー内の **FlowersTest.jpg** という名前のファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="9e730-119">Run another quick test, this time using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="9e730-120">この画像にはピカソ、レンブラント、またはポロックである確率として低い値が割り当てられることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9e730-120">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="9e730-121">このモデルはトレーニングされ、使用する準備ができており、特定の画家ごとに絵画をうまく識別することができます。</span><span class="sxs-lookup"><span data-stu-id="9e730-121">The model is trained and ready to go and appears to be skilled at identifying paintings by certain artists.</span></span> <span data-ttu-id="9e730-122">HTTP 経由で予測エンドポイントを呼び出して、何が起こるか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="9e730-122">Let's call the prediction endpoint over HTTP and see what happens.</span></span>