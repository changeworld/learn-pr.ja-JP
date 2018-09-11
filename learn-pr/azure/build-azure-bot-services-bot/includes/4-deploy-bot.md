<span data-ttu-id="b8645-101">[演習 1](#Exercise1) で Azure Web アプリ ボットを作成したときに、それをホストするために Azure Web アプリがデプロイされました。</span><span class="sxs-lookup"><span data-stu-id="b8645-101">When you created an Azure web app bot in [Exercise 1](#Exercise1), an Azure web app was deployed to host it.</span></span> <span data-ttu-id="b8645-102">しかし、ボットにはいくつかのコードが必要であり、それも Azure Web アプリにデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8645-102">But the bot does require some code, and it still needs to be deployed to the Azure web app.</span></span> <span data-ttu-id="b8645-103">さいわい、コードは Azure Bot Service によって生成されています。</span><span class="sxs-lookup"><span data-stu-id="b8645-103">Fortunately, the code was generated for you by the Azure Bot Service.</span></span> <span data-ttu-id="b8645-104">このユニットでは、Visual Studio Code を使用してそのコードをローカル Git リポジトリに配置して、ローカル リポジトリから、ボットをホストする Azure Web アプリに接続されているリモート リポジトリに変更をプッシュして、ボットを Azure に発行します ([継続的インテグレーション](https://en.wikipedia.org/wiki/Continuous_integration)として知られるプロセス)。</span><span class="sxs-lookup"><span data-stu-id="b8645-104">In this unit, you will use Visual Studio Code to place the code in a local Git repository and publish the bot to Azure by pushing changes from the local repository to a remote repository connected to the Azure web app that hosts the bot — a process known as [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration).</span></span>

1. <span data-ttu-id="b8645-105">ご使用の PC に [Git](https://git-scm.com/) がインストールされていない場合は、 https://git-scm.com/downloads に移動して、ご使用のオペレーティング システムの Git クライアントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8645-105">If [Git](https://git-scm.com/) isn't installed on your PC, go to https://git-scm.com/downloads and install the Git client for your operating system.</span></span> <span data-ttu-id="b8645-106">Git は無料のオープン ソースの分散バージョン管理システムで、Visual Studio Code にシームレスに統合されます。</span><span class="sxs-lookup"><span data-stu-id="b8645-106">Git is a free and open-source distributed version control system, and it integrates seamlessly into Visual Studio Code.</span></span> <span data-ttu-id="b8645-107">Git がインストールされているかどうかが不明な場合は、コマンド プロンプトまたはターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b8645-107">If you aren't sure whether Git is installed, open a Command Prompt or terminal window and execute the following command:</span></span>

    ``` 
    git --version
    ```

    <span data-ttu-id="b8645-108">バージョン番号が表示された場合は、Git クライアントがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="b8645-108">If a version number is displayed, then the Git client is installed.</span></span>

1. <span data-ttu-id="b8645-109">ご使用の PC に Node.js がインストールされていない場合は、 https://nodejs.org/ に移動して最新の LTS バージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8645-109">If Node.js isn't installed on your PC, go to https://nodejs.org/ and install the latest LTS version.</span></span> <span data-ttu-id="b8645-110">Node がインストールされているかどうかを判断するには、コマンド プロンプトまたはターミナル ウィンドウを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b8645-110">You can determine whether Node.js is installed by opening a Command Prompt or terminal window and typing the following command:</span></span>

    ```
    node --version
    ```

    <span data-ttu-id="b8645-111">Node がインストールされている場合は、バージョン番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8645-111">If Node is installed, the version number will be displayed.</span></span>

1. <span data-ttu-id="b8645-112">ご使用の PC に Visual Studio Code がインストールされていない場合は、 https://code.visualstudio.com/ に移動して今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8645-112">If Visual Studio Code isn't installed on your PC, go to https://code.visualstudio.com/ and install it now.</span></span>

1. <span data-ttu-id="b8645-113">ボットのソース コードを保持するため、自分のハード ディスク上の任意の場所に、"Factbot" という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8645-113">Create a folder named "Factbot" in the location of your choice on your hard disk to hold the bot's source code.</span></span>

1. <span data-ttu-id="b8645-114">Azure portal に戻り、"factbot-rg" リソース グループを開きます。</span><span class="sxs-lookup"><span data-stu-id="b8645-114">Return to the Azure portal and open the "factbot-rg" resource group.</span></span> <span data-ttu-id="b8645-115">次に、[演習 1](#Exercise1) で作成した Web アプリ ボットをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-115">Then, click the web app bot you created in [Exercise 1](#Exercise1).</span></span>

    ![Web アプリ ボットを開く](../media-draft/4-open-web-app-bot.png)

1. <span data-ttu-id="b8645-117">左側のメニューで **[ビルド]** をクリックし、**[Download zip file]\(ZIP ファイルのダウンロード\)** をクリックして、ボットのソース コードを含む ZIP ファイルを準備します。</span><span class="sxs-lookup"><span data-stu-id="b8645-117">Click **Build** in the menu on the left, and then click **Download zip file** to prepare a zip file containing the bot's source code.</span></span> <span data-ttu-id="b8645-118">ZIP ファイルの準備ができたら、**[Download zip file]\(ZIP ファイルのダウンロード\)** ボタンをクリックしてダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b8645-118">Once the zip file is prepared, click the **Download zip file** button to download it.</span></span> <span data-ttu-id="b8645-119">ダウンロードが完了したら、ZIP ファイルの内容を手順 4 で作成した "Factbot" フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b8645-119">When the download is complete, copy the contents of the zip file to the "Factbot" folder that you created in Step 4.</span></span>

    ![ソース コードをダウンロードする](../media-draft/4-download-source.png)

1. <span data-ttu-id="b8645-121">引き続き [ビルド] ブレードで、**[Configure continuous deployment]\(継続的デプロイの構成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-121">Still on the "Build" blade, click **Configure continuous deployment**.</span></span> <span data-ttu-id="b8645-122">**[ソースの選択]** に続いて表示されるブレードの上部で **[セットアップ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-122">Click **Setup** at the top of the ensuing blade, followed by **Choose Source**.</span></span> <span data-ttu-id="b8645-123">次に、デプロイ ソースとして **[ローカル Git リポジトリ]** を選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-123">Then, select **Local Git Repository** as the deployment source and click **OK**.</span></span> 

    ![デプロイ ソースとしてローカル Git リポジトリを指定する](../media-draft/4-portal-set-local-git.png)

1. <span data-ttu-id="b8645-125">[デプロイ] ブレードを閉じ、左側のメニューで **[All App service settings]\(すべての App Service の設定\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-125">Close the "Deployments" blade and click **All App service settings** in the menu on the left.</span></span>

1. <span data-ttu-id="b8645-126">**[デプロイ資格情報]** をクリックしてから、ユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="b8645-126">Click **Deployment credentials**, and then enter a user name and password.</span></span> <span data-ttu-id="b8645-127">ユーザー名は Azure 内で一意である必要があるので、"FactbotAdministrator" 以外のユーザー名を入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8645-127">You will probably have to enter a user name other than "FactbotAdministrator" because the name must be unique within Azure.</span></span> <span data-ttu-id="b8645-128">次に、**[保存]** をクリックして、ブレードを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b8645-128">Then, click **Save** and close the blade.</span></span>

    ![デプロイ資格情報を入力する](../media-draft/4-portal-enter-ci-creds.png)

1. <span data-ttu-id="b8645-130">Visual Studio Code を起動し、**[ファイル]** > **[フォルダーを開く]** コマンドを使用して、手順 6 でボットのソース コードをコピーした "Factbot" フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="b8645-130">Start Visual Studio Code and use the **File** > **Open Folder...** command to open the "Factbot" folder that you copied the bot's source code to in Step 6.</span></span>

1. <span data-ttu-id="b8645-131">Visual Studio Code の左側のアクティビティ バーで **[ソース管理]** ボタンをクリックし、上部の **[リポジトリの初期化]** アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-131">Click the **Source Control** button in the activity bar on the left side of Visual Studio Code, and click the **Initialize Repository** icon at the top.</span></span> <span data-ttu-id="b8645-132">その後、続いて表示されるダイアログで、**[リポジトリの初期化]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8645-132">Then, click the **Initialize Repository** button in the ensuing dialog.</span></span>

    ![ローカル Git リポジトリを初期化する](../media-draft/4-vs-init-git-repo.png)

1. <span data-ttu-id="b8645-134">メッセージ ボックスに「最初のコミット」と入力し、</span><span class="sxs-lookup"><span data-stu-id="b8645-134">Type "First commit."</span></span> <span data-ttu-id="b8645-135">チェック マークをクリックして変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="b8645-135">into the message box, and then click the check mark to commit your changes.</span></span>

    ![変更内容をローカル リポジトリにコミットする](../media-draft/4-vs-first-git-commit.png)

1. <span data-ttu-id="b8645-137">Visual Studio Code の **[表示]** メニューから **[統合端末]** を選択し、統合端末を開きます。</span><span class="sxs-lookup"><span data-stu-id="b8645-137">Select **Integrated Terminal** from Visual Studio Code's **View** menu to open an integrated terminal.</span></span> <span data-ttu-id="b8645-138">次に、統合端末で以下のコマンドを実行します。2 か所の BOT_NAME を演習 1 の手順 3 で入力したボット名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8645-138">Then, execute the following command in the integrated terminal, replacing BOT_NAME in two places with the bot name you entered in Exercise 1, Step 3.</span></span>

    ```
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. <span data-ttu-id="b8645-139">ソース コントロール パネルの上部にある省略記号 (3 つのドット) をクリックし、メニューから **[ブランチの発行]** を選択して、ボット コードをローカル リポジトリから Azure にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="b8645-139">Click the ellipsis (the three dots) at the top of the SOURCE CONTROL panel and select **Publish Branch** from the menu to push the bot code from the local repository to Azure.</span></span> <span data-ttu-id="b8645-140">資格情報を求められた場合は、この演習の手順 9 で指定したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="b8645-140">If prompted for credentials, enter the user name and password you specified in Step 9 of this exercise.</span></span>

<span data-ttu-id="b8645-141">ボットは Azure に発行されています。</span><span class="sxs-lookup"><span data-stu-id="b8645-141">Your bot has been published to Azure.</span></span> <span data-ttu-id="b8645-142">しかし、それをここでテストする前に、ローカルで実行して、Visual Studio Code でデバッグする方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="b8645-142">But before you test it there, let's run it locally and learn how to debug it in Visual Studio Code.</span></span>