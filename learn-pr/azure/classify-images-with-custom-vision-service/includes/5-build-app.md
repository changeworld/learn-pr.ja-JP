### <a name="exercise-5-create-a-nodejs-app-that-uses-the-model"></a><span data-ttu-id="b54f2-101">演習 5: モデルを使用する Node.js アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="b54f2-101">Exercise 5: Create a Node.js app that uses the model</span></span>

<span data-ttu-id="b54f2-102">Microsoft Custom Vision Service の真の力は、[Custom Vision の予測 API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) を使用して、開発者がそのインテリジェンスを独自のアプリケーションに簡単に組み込むことができることにあります。</span><span class="sxs-lookup"><span data-stu-id="b54f2-102">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="b54f2-103">この演習では、Visual Studio Code を使用して、Artwork という名前のアプリを変更し、前の演習でビルドおよびトレーニングを行ったモデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-103">In this exercise, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="b54f2-104">ご利用のシステムに Node.js がインストールされていない場合は、 https://nodejs.org に移動して使用しているオペレーティング システム用の最新の LTS バージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-104">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

    > <span data-ttu-id="b54f2-105">Node.js がインストールされているかどうかが不明な場合は、コマンド プロンプトまたはターミナル ウィンドウを開き、「**node -v**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-105">If you aren't sure whether Node.js is installed, open a Command Prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="b54f2-106">Node.js のバージョン番号が表示されない場合は、Node.js はインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="b54f2-106">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="b54f2-107">インストールされている Node.js のバージョンが 6.0 よりも古い場合は、最新バージョンをダウンロードしてインストールすることを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-107">If a version of Node.js older than 6.0 is installed, it is highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="b54f2-108">ご利用のワークステーションに Visual Studio Code がインストールされていない場合は、 http://code.visualstudio.com に移動して今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-108">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="b54f2-109">Visual Studio Code を起動して、**[ファイル]** メニューから **[フォルダーを開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-109">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="b54f2-110">後続のダイアログで、ラボ リソースに含まれている "Client\Artworks" フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-110">In the ensuing dialog, select the "Client\Artworks" folder included in the lab resources.</span></span>

    ![Artworks フォルダーの選択](../images/fe-select-folder.png)

    <span data-ttu-id="b54f2-112">_Artworks フォルダーの選択_</span><span class="sxs-lookup"><span data-stu-id="b54f2-112">_Selecting the Artworks folder_</span></span> 

1. <span data-ttu-id="b54f2-113">Visual Studio Code で、**[表示]** > **[統合ターミナル]** を使用し、統合ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-113">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="b54f2-114">次に、統合ターミナルで次のコマンドを実行し、アプリに必要なパッケージを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-114">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```
    npm install
    ```

1. <span data-ttu-id="b54f2-115">Custom Vision Service ポータル内の Artwork プロジェクトに戻り、**[パフォーマンス]**、**[既定に設定]** の順にクリックして、モデルの最新のイテレーションが既定のイテレーションになっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-115">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span> 

    ![既定のイテレーションの指定](../images/portal-make-default.png)

    <span data-ttu-id="b54f2-117">_既定のイテレーションの指定_</span><span class="sxs-lookup"><span data-stu-id="b54f2-117">_Specifying the default iteration_</span></span> 

1. <span data-ttu-id="b54f2-118">アプリを実行して使用し、Custom Vision Service を呼び出すには、エンドポイントと認証情報を含めるように、事前にアプリを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b54f2-118">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="b54f2-119">そのために、**[Prediction URL]\(予測 URL\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-119">To that end, click **Prediction URL**.</span></span>

    ![予測 URL の情報を表示](../images/portal-prediction-url.png)

    <span data-ttu-id="b54f2-121">_予測 URL の情報を表示_</span><span class="sxs-lookup"><span data-stu-id="b54f2-121">_Viewing Prediction URL information_</span></span> 

1. <span data-ttu-id="b54f2-122">後続のダイアログに次の 2 つの URL が一覧されます。1 つは URL を使用してイメージをアップロードするための URL、もう 1 つはローカルのイメージをアップロードするための URL です。</span><span class="sxs-lookup"><span data-stu-id="b54f2-122">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="b54f2-123">イメージ ファイル用の予測 API URL をクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-123">Copy the Prediction API URL for image files to the clipboard.</span></span> 

    ![予測 API URL のコピー](../images/copy-prediction-url.png)

    <span data-ttu-id="b54f2-125">_予測 API URL のコピー_</span><span class="sxs-lookup"><span data-stu-id="b54f2-125">_Copying the Prediction API URL_</span></span> 

1. <span data-ttu-id="b54f2-126">Visual Studio Code に戻り、**[predict.js]** をクリックしてコード エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-126">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![predict.js を開く](../images/vs-predict-file.png)

    <span data-ttu-id="b54f2-128">"predict.js を開く"__</span><span class="sxs-lookup"><span data-stu-id="b54f2-128">_Opening predict.js_</span></span> 

1. <span data-ttu-id="b54f2-129">3 行目の "PREDICTION_ENDPOINT" をクリップボードの URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-129">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![予測 API URL の追加](../images/vs-prediction-endpoint.png)

    <span data-ttu-id="b54f2-131">_予測 API URL の追加_</span><span class="sxs-lookup"><span data-stu-id="b54f2-131">_Adding the Prediction API URL_</span></span> 

1. <span data-ttu-id="b54f2-132">Custom Vision Service ポータルに戻り、予測 API キーをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b54f2-132">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span> 

    ![予測 API キーのコピー](../images/copy-prediction-key.png)

    <span data-ttu-id="b54f2-134">_予測 API キーのコピー_</span><span class="sxs-lookup"><span data-stu-id="b54f2-134">_Copying the Prediction API key_</span></span> 

1. <span data-ttu-id="b54f2-135">Visual Studio Code に戻り、**predict.js** の 4 行目の "PREDICTION_KEY"をクリップボードの API キーに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-135">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![予測 API キーの追加](../images/vs-prediction-key.png)

    <span data-ttu-id="b54f2-137">"予測 API キーの追加"__</span><span class="sxs-lookup"><span data-stu-id="b54f2-137">_Adding the Prediction API key_</span></span> 

1. <span data-ttu-id="b54f2-138">**predict.js** まで下へスクロールして、34 行目から始まるコードのブロックを確認します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-138">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="b54f2-139">これは、AJAX を使用して Custom Vision Service を呼び出すコードです。</span><span class="sxs-lookup"><span data-stu-id="b54f2-139">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="b54f2-140">Custom Vision の予測 API の使用は、REST エンドポイントへのシンプルな認証済みの投稿を作成するのと同じくらい簡単です。</span><span class="sxs-lookup"><span data-stu-id="b54f2-140">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![予測 API を呼び出す](../images/vs-code-block.png)

    <span data-ttu-id="b54f2-142">"予測 API を呼び出す"__</span><span class="sxs-lookup"><span data-stu-id="b54f2-142">_Making a call to the Prediction API_</span></span> 

1. <span data-ttu-id="b54f2-143">Visual Studio Code の統合ターミナルに戻り、次のコマンドを実行してアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-143">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```
    npm start
    ```

1. <span data-ttu-id="b54f2-144">Artworks アプリが開始され、次のようなウィンドウが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-144">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![Artworks アプリ](../images/app-startup.png)

    <span data-ttu-id="b54f2-146">"Artworks アプリ"__</span><span class="sxs-lookup"><span data-stu-id="b54f2-146">_The Artworks app_</span></span> 

<span data-ttu-id="b54f2-147">Artworks は、Node.js と [Electron](https://electron.atom.io/) で記述されたクロスプラットフォーム アプリです。</span><span class="sxs-lookup"><span data-stu-id="b54f2-147">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="b54f2-148">そのため、Windows、macOS、Linux 上で同様に実行することができます。</span><span class="sxs-lookup"><span data-stu-id="b54f2-148">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="b54f2-149">次の演習では、このアプリを使用して、描いた画家によってイメージを分類します。</span><span class="sxs-lookup"><span data-stu-id="b54f2-149">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>