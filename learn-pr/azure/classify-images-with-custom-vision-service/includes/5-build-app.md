<span data-ttu-id="8f040-101">Microsoft Custom Vision Service の真の力は、[Custom Vision の予測 API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) を使用して、開発者がそのインテリジェンスを独自のアプリケーションに簡単に組み込むことができることにあります。</span><span class="sxs-lookup"><span data-stu-id="8f040-101">The true power of the Microsoft Custom Vision Service is the ease with which developers can incorporate its intelligence into their own applications using the [Custom Vision Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814).</span></span> <span data-ttu-id="8f040-102">このユニットでは、Visual Studio Code を使用して、Artwork という名前のアプリを変更し、前の演習でビルドおよびトレーニングを行ったモデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="8f040-102">In this unit, you will use Visual Studio Code to modify an app named Artwork to use the model you built and trained in previous exercises.</span></span>

1. <span data-ttu-id="8f040-103">ご利用のシステムに Node.js がインストールされていない場合は、 https://nodejs.org に移動して使用しているオペレーティング システム用の最新の LTS バージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f040-103">If Node.js isn't installed on your system, go to https://nodejs.org and install the latest LTS version for your operating system.</span></span>

   > <span data-ttu-id="8f040-104">Node.js がインストールされているかどうかが不明な場合は、コマンド プロンプトまたはターミナル ウィンドウを開き、「**node -v**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="8f040-104">If you aren't sure whether Node.js is installed, open a command prompt or terminal window and type **node -v**.</span></span> <span data-ttu-id="8f040-105">Node.js のバージョン番号が表示されない場合は、Node.js はインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="8f040-105">If you don't see a Node.js version number, then Node.js isn't installed.</span></span> <span data-ttu-id="8f040-106">インストールされている Node.js のバージョンが 6.0 よりも古い場合は、最新バージョンをダウンロードしてインストールすることを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8f040-106">If a version of Node.js older than 6.0 is installed, we highly recommend that you download and install the latest version.</span></span>

1. <span data-ttu-id="8f040-107">ご利用のワークステーションに Visual Studio Code がインストールされていない場合は、 http://code.visualstudio.com に移動して今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f040-107">If Visual Studio Code isn't installed on your workstation, go to http://code.visualstudio.com and install it now.</span></span>

1. <span data-ttu-id="8f040-108">Visual Studio Code を起動して、**[ファイル]** メニューから **[フォルダーを開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8f040-108">Start Visual Studio Code and select **Open Folder...** from the **File** menu.</span></span> <span data-ttu-id="8f040-109">後続のダイアログで、モジュール リソースに含まれている "Client\Artworks" フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="8f040-109">In the ensuing dialog, select the "Client\Artworks" folder included in the module resources.</span></span>

    ![Artworks フォルダーの選択](../media-draft/5-fe-select-folder.png)

1. <span data-ttu-id="8f040-111">Visual Studio Code で、**[表示]** > **[統合ターミナル]** を使用し、統合ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="8f040-111">Use the **View** > **Integrated Terminal** command to open an integrated terminal window in Visual Studio Code.</span></span> <span data-ttu-id="8f040-112">次に、統合ターミナルで次のコマンドを実行し、アプリに必要なパッケージを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8f040-112">Then execute the following command in the integrated terminal to load the packages required by the app:</span></span>

    ```
    npm install
    ```

1. <span data-ttu-id="8f040-113">Custom Vision Service ポータル内の Artwork プロジェクトに戻り、**[パフォーマンス]**、**[既定に設定]** の順にクリックして、モデルの最新のイテレーションが既定のイテレーションになっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f040-113">Return to the Artwork project in the Custom Vision Service portal, click **Performance**, and then click **Make default** to make sure the latest iteration of the model is the default iteration.</span></span> 

    ![既定のイテレーションの指定](../media-draft/5-portal-make-default.png)

1. <span data-ttu-id="8f040-115">アプリを実行して使用し、Custom Vision Service を呼び出すには、エンドポイントと認証情報を含めるように、事前にアプリを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f040-115">Before you can run the app and use it to call the Custom Vision Service, it must be modified to include endpoint and authorization information.</span></span> <span data-ttu-id="8f040-116">そのために、**[Prediction URL]\(予測 URL\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8f040-116">To that end, click **Prediction URL**.</span></span>

    ![予測 URL の情報を表示](../media-draft/5-portal-prediction-url.png)

1. <span data-ttu-id="8f040-118">後続のダイアログに次の 2 つの URL が一覧されます。1 つは URL を使用して画像をアップロードするための URL、もう 1 つはローカルの画像をアップロードするための URL です。</span><span class="sxs-lookup"><span data-stu-id="8f040-118">The ensuing dialog lists two URLs: one for uploading images via URL, and another for uploading local images.</span></span> <span data-ttu-id="8f040-119">イメージ ファイル用の予測 API URL をクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="8f040-119">Copy the Prediction API URL for image files to the clipboard.</span></span> 

    ![予測 API URL のコピー](../media-draft/5-copy-prediction-url.png)

1. <span data-ttu-id="8f040-121">Visual Studio Code に戻り、**[predict.js]** をクリックしてコード エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="8f040-121">Return to Visual Studio Code and click **predict.js** to open it in the code editor.</span></span>

    ![predict.js を開く](../media-draft/5-vs-predict-file.png)

1. <span data-ttu-id="8f040-123">3 行目の "PREDICTION_ENDPOINT" をクリップボードの URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8f040-123">Replace "PREDICTION_ENDPOINT" in line 3 with the URL on the clipboard.</span></span>

    ![予測 API URL の追加](../media-draft/5-vs-prediction-endpoint.png)

1. <span data-ttu-id="8f040-125">Custom Vision Service ポータルに戻り、予測 API キーをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="8f040-125">Return to the Custom Vision Service portal and copy the Prediction API key to the clipboard.</span></span> 

    ![予測 API キーのコピー](../media-draft/5-copy-prediction-key.png)

1. <span data-ttu-id="8f040-127">Visual Studio Code に戻り、**predict.js** の 4 行目の "PREDICTION_KEY"をクリップボードの API キーに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8f040-127">Return to Visual Studio Code and replace "PREDICTION_KEY" in line 4 of **predict.js** with the API key on the clipboard.</span></span>

    ![予測 API キーの追加](../media-draft/5-vs-prediction-key.png)

1. <span data-ttu-id="8f040-129">**predict.js** まで下へスクロールして、34 行目から始まるコードのブロックを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f040-129">Scroll down in **predict.js** and examine the block of code that begins on line 34.</span></span> <span data-ttu-id="8f040-130">これは、AJAX を使用して Custom Vision Service を呼び出すコードです。</span><span class="sxs-lookup"><span data-stu-id="8f040-130">This is the code that calls out to the Custom Vision Service using AJAX.</span></span> <span data-ttu-id="8f040-131">Custom Vision の予測 API の使用は、REST エンドポイントへのシンプルな認証済みの投稿を作成するのと同じくらい簡単です。</span><span class="sxs-lookup"><span data-stu-id="8f040-131">Using the Custom Vision Prediction API is as easy as making a simple, authenticated POST to a REST endpoint.</span></span>

    ![予測 API を呼び出す](../media-draft/5-vs-code-block.png)

1. <span data-ttu-id="8f040-133">Visual Studio Code の統合ターミナルに戻り、次のコマンドを実行してアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="8f040-133">Return to the integrated terminal in Visual Studio Code and execute the following command to start the app:</span></span>

    ```
    npm start
    ```

1. <span data-ttu-id="8f040-134">Artworks アプリが開始され、次のようなウィンドウが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f040-134">Confirm that the Artworks app starts and displays a window like this one:</span></span>

    ![Artworks アプリ](../media-draft/5-app-startup.png)

<span data-ttu-id="8f040-136">Artworks は、Node.js と [Electron](https://electron.atom.io/) で記述されたクロスプラットフォーム アプリです。</span><span class="sxs-lookup"><span data-stu-id="8f040-136">Artworks is a cross-platform app written with Node.js and [Electron](https://electron.atom.io/).</span></span> <span data-ttu-id="8f040-137">そのため、Windows、macOS、Linux 上で同様に実行することができます。</span><span class="sxs-lookup"><span data-stu-id="8f040-137">As such, it is equally capable of running on Windows, macOS, and Linux.</span></span> <span data-ttu-id="8f040-138">次の演習では、このアプリを使用して、描いた画家によってイメージを分類します。</span><span class="sxs-lookup"><span data-stu-id="8f040-138">In the next exercise, you will use it to classify images by the artists who painted them.</span></span>