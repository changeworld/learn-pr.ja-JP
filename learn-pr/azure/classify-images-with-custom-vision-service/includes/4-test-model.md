<span data-ttu-id="421f8-101">このモジュールでは後ほど、モデルを使用して、表示する画家を識別する Node.js アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="421f8-101">Later in this module, you will create a Node.js app that uses the model to identify the artist of paintings presented to it.</span></span> <span data-ttu-id="421f8-102">しかし、モデルをテストするために、アプリを作成する必要はありません。ポータル内でご自分のテストを行うことができ、テストする画像を使ってさらにモデルを改良することができます。</span><span class="sxs-lookup"><span data-stu-id="421f8-102">But you don't have to write an app to test the model; you can do your testing in the portal, and you can further refine the model using the images that you test with.</span></span> <span data-ttu-id="421f8-103">このユニットでは、提供されているテスト画像を使用して画家を識別するモデルの機能をテストします。</span><span class="sxs-lookup"><span data-stu-id="421f8-103">In this unit, you will test the model's ability to identify the artist of a painting using test images provided for you.</span></span>

1. <span data-ttu-id="421f8-104">ページの上部にある **[Quick Test]\(クイック テスト\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="421f8-104">Click **Quick Test** at the top of the page.</span></span>

    ![モデルのテスト](../media/4-portal-click-quick-test.png)

1. <span data-ttu-id="421f8-106">**[ローカル ファイルを参照します]** をクリックして、モジュール リソースの "Quick Tests" フォルダーを参照します。</span><span class="sxs-lookup"><span data-stu-id="421f8-106">Click **Browse local files**, and then browse to the "Quick Tests" folder in the module resources.</span></span> <span data-ttu-id="421f8-107">**PicassoTest_01.jpg** を選択して、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="421f8-107">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![ピカソのテスト画像の選択](../media/4-portal-select-test-01.png)

1. <span data-ttu-id="421f8-109">[Quick Test]\(クイック テスト\) ダイアログでテストの結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="421f8-109">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="421f8-110">絵画がピカソである確率とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="421f8-110">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="421f8-111">絵画がレンブラントやポロックである確率とは何ですか? </span><span class="sxs-lookup"><span data-stu-id="421f8-111">What is the probability that it is a Rembrandt or Pollock?</span></span>

1. <span data-ttu-id="421f8-112">"クイック テスト" ダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="421f8-112">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="421f8-113">次に、ページの上部にある **[予測]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="421f8-113">Then click **Predictions** at the top of the page.</span></span>

    ![実行されたテストの表示](../media/4-portal-select-predictions.png)

1. <span data-ttu-id="421f8-115">アップロードしたテスト画像をクリックして、その詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="421f8-115">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="421f8-116">次に、ドロップダウン リストから **[Picasso]** を選択し、**[保存して閉じる]** をクリックすることによって、イメージを "Picasso" としてタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="421f8-116">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="421f8-117">このようにテスト イメージをタグ付けすることによって、追加のトレーニング用イメージをアップロードすることなく、モデルを改良することができます。</span><span class="sxs-lookup"><span data-stu-id="421f8-117">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>

    ![テスト画像のタグ付け](../media/4-tag-test-image.png)

1. <span data-ttu-id="421f8-119">[Quick Test]\(クイック テスト\) フォルダー内の **FlowersTest.jpg** という名前のファイルを使用して、別のクイック テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="421f8-119">Perform another quick test using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="421f8-120">このイメージはピカソ、レンブラント、またはポロックである確率が低いと割り当てられていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="421f8-120">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="421f8-121">このモデルはトレーニングされ、使用する準備ができており、特定の画家ごとに絵画をうまく識別することができます。</span><span class="sxs-lookup"><span data-stu-id="421f8-121">The model is trained and ready to go and appears to be adept at identifying paintings by certain artists.</span></span> <span data-ttu-id="421f8-122">さらに手順を進めて、モデルのインテリジェンスをアプリに組み込みましょう。</span><span class="sxs-lookup"><span data-stu-id="421f8-122">Now let's go a step further and incorporate the model's intelligence into an app.</span></span>