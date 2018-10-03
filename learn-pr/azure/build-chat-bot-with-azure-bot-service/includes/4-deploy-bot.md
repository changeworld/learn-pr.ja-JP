[!INCLUDE [0-vm-note](0-vm-note.md)]

<span data-ttu-id="b11a9-101">Azure Web アプリ ボットを作成したときに、それをホストするために Azure Web アプリがデプロイされました。</span><span class="sxs-lookup"><span data-stu-id="b11a9-101">When you created an Azure web app bot, an Azure web app was deployed to host it.</span></span> <span data-ttu-id="b11a9-102">しかし、ボットにはいくつかのコードが必要であり、それも Azure Web アプリにデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b11a9-102">But the bot does require some code, and it still needs to be deployed to the Azure web app.</span></span> <span data-ttu-id="b11a9-103">さいわい、コードは Azure Bot Service によって生成されています。</span><span class="sxs-lookup"><span data-stu-id="b11a9-103">Fortunately, the code was generated for you by the Azure Bot Service.</span></span> <span data-ttu-id="b11a9-104">このユニットでは、Visual Studio Code を使用してそのコードをローカル Git リポジトリに配置して、ローカル リポジトリから、ボットをホストする Azure Web アプリに接続されているリモート リポジトリに変更をプッシュして、ボットを Azure に発行します ([継続的インテグレーション](https://wikipedia.org/wiki/Continuous_integration)として知られるプロセス)。</span><span class="sxs-lookup"><span data-stu-id="b11a9-104">In this unit, you will use Visual Studio Code to place the code in a local Git repository and publish the bot to Azure by pushing changes from the local repository to a remote repository connected to the Azure web app that hosts the bot — a process known as [continuous integration](https://wikipedia.org/wiki/Continuous_integration).</span></span>

1. <span data-ttu-id="b11a9-105">ボットのソース コードを保持するため、自分のハード ディスク上の任意の場所に、"Factbot" という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-105">Create a folder named "Factbot" in the location of your choice on your hard disk to hold the bot's source code.</span></span>

1. <span data-ttu-id="b11a9-106">VM ブラウザーで Azure portal に戻り、事前に作成した演習のリソース グループを開きます。</span><span class="sxs-lookup"><span data-stu-id="b11a9-106">Return to the Azure portal in the VM browser and open the pre-created exercise resource group.</span></span> <span data-ttu-id="b11a9-107">次に、前の演習で作成した Web アプリ ボットを選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-107">Then, select the Web App Bot you created in the prior exercise.</span></span>

1. <span data-ttu-id="b11a9-108">左側のメニューで **[ビルド]** を選択し、**[Download Bot source code]\(ボットのソース コードのダウンロード\)** を選択して、ボットのソース コードを含む ZIP ファイルを準備します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-108">Select **Build** in the menu on the left, then select **Download Bot source code** to prepare a zip file containing the bot's source code.</span></span> <span data-ttu-id="b11a9-109">ZIP ファイルの準備ができたら、**[Download Bot source code]\(ボットのソース コードのダウンロード\)** ボタンを選択してダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b11a9-109">Once the zip file is prepared, select the **Download Bot source code** button to download it.</span></span> <span data-ttu-id="b11a9-110">ダウンロードが完了したら、ZIP ファイルのコンテンツを先ほど作成した "Factbot" フォルダーに展開します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-110">When the download is complete, extract the contents of the zip file to the "Factbot" folder that you created earlier.</span></span>

1. <span data-ttu-id="b11a9-111">Azure portal で Web アプリ ボットの [Build]\(ビルド\) ブレードに戻り、**[Configure continuous deployment]\(継続的デプロイの構成\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-111">Back in the Web App Bot's Build blade in the Azure portal, select **Configure continuous deployment**.</span></span>

1. <span data-ttu-id="b11a9-112">**[ソースの選択]** に続いて表示される **[デプロイ]** ブレードの上部で **[セットアップ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-112">Select **Setup** at the top of the **Deployments** blade, followed by **Choose Source**.</span></span>

1. <span data-ttu-id="b11a9-113">次に、デプロイ ソースとして **[ローカル Git リポジトリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-113">Then, select **Local Git Repository** as the deployment source.</span></span>

1. <span data-ttu-id="b11a9-114">次に、**[接続のセットアップ]** を選択し、ユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-114">Next, select **Setup connection** and enter a username and password.</span></span> <span data-ttu-id="b11a9-115">ユーザー名は Azure 内で一意である必要があるので、"FactbotAdministrator" 以外のユーザー名を入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b11a9-115">You will probably have to enter a user name other than "FactbotAdministrator" because the name must be unique within Azure.</span></span> <span data-ttu-id="b11a9-116">次に、**[OK]** を選択して **[デプロイ オプション]** ブレードに戻り、もう一度 **[OK]** を選択して **[デプロイ]** ブレードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b11a9-116">Then, select **OK** to return to the **Deployment option** blade and **OK** again to return to the **Deployments** blade.</span></span>

    ![新しいボットが表示された Azure portal のスクリーンショット。App Service ブレードに表示された [デプロイ資格情報] 画面。[デプロイ資格情報] メニュー項目と[保存] ボタンが強調表示されています。](../media/4-portal-enter-ci-creds.png)

1. <span data-ttu-id="b11a9-118">デプロイ システムのプロビジョニング中に、**[デプロイ]** ブレードを閉じ、左側のメニューで **[すべてのアプリ サービスの設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-118">While the deployment system is provisioning, close the **Deployments** blade, and select **All App service settings** in the menu on the left.</span></span>

1. <span data-ttu-id="b11a9-119">**Visual Studio Code** を起動し、**[ファイル]** > **[フォルダーを開く]** を使用して、ボットのソース コードをコピーした "Factbot" フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="b11a9-119">Start **Visual Studio Code**, and use the **File** > **Open Folder...** command to open the "Factbot" folder where you copied the bot's source code.</span></span>

1. <span data-ttu-id="b11a9-120">Visual Studio Code の左側にあるアクティビティ バーで **[ソース管理]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-120">Select the **Source Control** button in the activity bar on the left side of Visual Studio Code.</span></span>

1. <span data-ttu-id="b11a9-121">上部にある **[リポジトリの初期化]** アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-121">Select the **Initialize Repository** icon at the top.</span></span>

1. <span data-ttu-id="b11a9-122">ダイアログで **[リポジトリの初期化]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-122">Select the **Initialize Repository** button in the dialog.</span></span>

1. <span data-ttu-id="b11a9-123">メッセージ ボックスに「最初のコミット」と</span><span class="sxs-lookup"><span data-stu-id="b11a9-123">Type "First commit."</span></span> <span data-ttu-id="b11a9-124">入力します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-124">into the message box.</span></span>

1. <span data-ttu-id="b11a9-125">確認を求められたら、すべてのファイルをステージングする変更をコミットするチェック マークを選択します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-125">Select the check mark to commit your changes, staging all the files when prompted.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b11a9-126">ID が Git で設定されていないことに関する Git エラーが発生した場合は、コマンド プロンプトを起動し、次のコマンドを実行します。必要に応じて、プレースホルダーのメール アドレスと名前をご自分のものに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="b11a9-126">If you get a Git error about not having your identity set in Git, launch a Command Prompt and run the following commands, replacing the placeholder email and name values if you wish.</span></span> <span data-ttu-id="b11a9-127">その後、コミット ボタンを再度押します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-127">Then retry the commit button.</span></span>
    >
    > ```bash
    > git config --global user.email "Lab User"
    > git config --global user.name "LabUser#######@learn"
    > ```

1. <span data-ttu-id="b11a9-128">Visual Studio Code の **[表示]** メニューから **[ターミナル]** を選択し、統合ターミナルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b11a9-128">Select **Terminal** from Visual Studio Code's **View** menu to open an integrated terminal.</span></span>

1. <span data-ttu-id="b11a9-129">統合ターミナルで次のコマンドを実行します。次の 2 か所の BOT_NAME を演習 1 で入力したボット名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b11a9-129">Execute the following command in the integrated terminal, replacing BOT_NAME in the following two places with the bot name you entered in Exercise 1.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b11a9-130">Git リモートの完全な URL は、App Service リソースの **[Git クローン URL]** の下の **[概要]** セクションでも確認できます。</span><span class="sxs-lookup"><span data-stu-id="b11a9-130">The full Git remote URL can also be found in the App Service resource's **Overview** section under **Git clone url**.</span></span>

    ```bash
    git remote add qna-factbot https://BOT_NAME.scm.azurewebsites.net:443/BOT_NAME.git
    ```

1. <span data-ttu-id="b11a9-131">**[ソース コントロール]** パネルに戻り、[ソース コントロール] パネルの上部にある省略記号 (3 つのドット) をクリックし、メニューから **[ブランチの発行]** を選択して、ボット コードをローカル リポジトリから Azure にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="b11a9-131">Return to the **Source Control** panel and select the ellipsis (the three dots) at the top of the SOURCE CONTROL panel, and select **Publish Branch** from the menu to push the bot code from the local repository to Azure.</span></span> <span data-ttu-id="b11a9-132">資格情報を求められた場合は、この演習の手順 9 で指定したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-132">If prompted for credentials, enter the username and password you specified in Step 9 of this exercise.</span></span>

<span data-ttu-id="b11a9-133">ボットは Azure に発行されています。</span><span class="sxs-lookup"><span data-stu-id="b11a9-133">Your bot has been published to Azure.</span></span> <span data-ttu-id="b11a9-134">しかし、それをここでテストする前に、ローカルで実行して、Visual Studio Code でデバッグする方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="b11a9-134">But before you test it there, let's run it locally and learn how to debug it in Visual Studio Code.</span></span>
