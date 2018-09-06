### <a name="exercise-4-test-the-model"></a><span data-ttu-id="d0779-101">演習 4: モデルをテストする</span><span class="sxs-lookup"><span data-stu-id="d0779-101">Exercise 4: Test the model</span></span>

<span data-ttu-id="d0779-102">[演習 5](../5-build-app.yml) では、モデルを使用し、画家を識別して表示する Node.js アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0779-102">In [Exercise 5](../5-build-app.yml), you will create a Node.js app that uses the model to identify the artist of paintings presented to it.</span></span> <span data-ttu-id="d0779-103">しかし、モデルをテストするために、アプリを作成する必要はありません。ポータル内でご自分のテストを行うことができ、テストするイメージを使ってさらにモデルを改良することができます。</span><span class="sxs-lookup"><span data-stu-id="d0779-103">But you don't have to write an app to test the model; you can do your testing in the portal, and you can further refine the model using the images that you test with.</span></span> <span data-ttu-id="d0779-104">この演習では、モデルの機能をテストし、ユーザーのために提供されているテスト イメージを使用して画家を識別します。</span><span class="sxs-lookup"><span data-stu-id="d0779-104">In this exercise, you will test the model's ability to identify the artist of a painting using test images provided for you.</span></span>

1. <span data-ttu-id="d0779-105">ページの上部にある **[Quick Test]\(クイック テスト\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0779-105">Click **Quick Test** at the top of the page.</span></span>
 
    ![モデルのテスト](../images/portal-click-quick-test.png)

    <span data-ttu-id="d0779-107">"モデルのテスト"__</span><span class="sxs-lookup"><span data-stu-id="d0779-107">_Testing the model_</span></span> 

1. <span data-ttu-id="d0779-108">**[ローカル ファイルを参照します]** をクリックして、ラボ リソースの "Quick Tests" フォルダーを参照します。</span><span class="sxs-lookup"><span data-stu-id="d0779-108">Click **Browse local files**, and then browse to the "Quick Tests" folder in the lab resources.</span></span> <span data-ttu-id="d0779-109">**PicassoTest_01.jpg** を選択して、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0779-109">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![ピカソのテスト イメージの選択](../images/portal-select-test-01.png)

    <span data-ttu-id="d0779-111">_ピカソのテスト イメージの選択_</span><span class="sxs-lookup"><span data-stu-id="d0779-111">_Selecting a Picasso test image_</span></span> 

1. <span data-ttu-id="d0779-112">"クイック テスト" ダイアログでテストの結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d0779-112">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="d0779-113">絵画がピカソである確率とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="d0779-113">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="d0779-114">絵画がレンブラントやポロックである確率とは何ですか? </span><span class="sxs-lookup"><span data-stu-id="d0779-114">What is the probability that it is a Rembrandt or Pollock?</span></span>

1. <span data-ttu-id="d0779-115">"クイック テスト" ダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="d0779-115">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="d0779-116">次に、ページの上部にある **[予測]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d0779-116">Then click **Predictions** at the top of the page.</span></span>
 
    ![実行されたテストの表示](../images/portal-select-predictions.png)

    <span data-ttu-id="d0779-118">_実行されたテストの表示_</span><span class="sxs-lookup"><span data-stu-id="d0779-118">_Viewing the tests that have been performed_</span></span> 

1. <span data-ttu-id="d0779-119">アップロードしたテスト イメージをクリックして、その詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="d0779-119">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="d0779-120">次に、ドロップダウン リストから **[Picasso]** を選択し、**[保存して閉じる]** をクリックすることによって、イメージを "Picasso" としてタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="d0779-120">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="d0779-121">このようにテスト イメージをタグ付けすることによって、追加のトレーニング用イメージをアップロードすることなく、モデルを改良することができます。</span><span class="sxs-lookup"><span data-stu-id="d0779-121">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>
 
    ![テスト イメージのタグ付け](../images/tag-test-image.png)

    <span data-ttu-id="d0779-123">"テスト イメージのタグ付け"__</span><span class="sxs-lookup"><span data-stu-id="d0779-123">_Tagging the test image_</span></span> 

1. <span data-ttu-id="d0779-124">"Quick Test" フォルダー内の **FlowersTest.jpg** という名前のファイルを使用して、別のクイック テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="d0779-124">Perform another quick test using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="d0779-125">このイメージはピカソ、レンブラント、またはポロックである確率が低いと割り当てられていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d0779-125">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="d0779-126">このモデルはトレーニングされ、使用する準備ができており、特定の画家ごとに絵画をうまく識別することができます。</span><span class="sxs-lookup"><span data-stu-id="d0779-126">The model is trained and ready to go and appears to be adept at identifying paintings by certain artists.</span></span> <span data-ttu-id="d0779-127">さらに手順を進めて、モデルのインテリジェンスをアプリに組み込みましょう。</span><span class="sxs-lookup"><span data-stu-id="d0779-127">Now let's go a step further and incorporate the model's intelligence into an app.</span></span>