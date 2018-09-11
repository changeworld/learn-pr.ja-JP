### <a name="exercise-2-connect-to-the-data-science-vm"></a><span data-ttu-id="ef869-101">演習 2: Data Science VM に接続する</span><span class="sxs-lookup"><span data-stu-id="ef869-101">Exercise 2: Connect to the Data Science VM</span></span>

<span data-ttu-id="ef869-102">この演習では、前の演習で作成した VM にある Ubuntu デスクトップにリモートで接続します。</span><span class="sxs-lookup"><span data-stu-id="ef869-102">In this exercise, you will connect remotely to the Ubuntu desktop in the VM that you created in the previous exercise.</span></span> <span data-ttu-id="ef869-103">この操作を行うには、Linux 用のライトウェイト デスクトップ環境である、[Xfce](https://xfce.org/) をサポートするクライアントが必要です。</span><span class="sxs-lookup"><span data-stu-id="ef869-103">To do so, you need a client that supports [Xfce](https://xfce.org/), which is a lightweight desktop environment for Linux.</span></span> <span data-ttu-id="ef869-104">背景について、および DSVM に接続できるさまざまな方法の概要については、「[Linux データ サイエンス仮想マシンにアクセスする方法](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#how-to-access-the-data-science-virtual-machine-for-linux)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef869-104">For background, and for an overview of the various ways you can connect to a DSVM, see [How to access the Data Science Virtual Machine for Linux ](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#how-to-access-the-data-science-virtual-machine-for-linux).</span></span>

1. <span data-ttu-id="ef869-105">まだ Xfce クライアントをインストールしていない場合は、この演習を続ける前に、[X2Go クライアント](https://wiki.x2go.org/doku.php/download:start)をダウンロードしてインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="ef869-105">If you don't already have an Xfce client installed, download the [X2Go client](https://wiki.x2go.org/doku.php/download:start) and install it before continuing with this exercise.</span></span> <span data-ttu-id="ef869-106">X2Go は無料であり、Windows や OS X などのさまざまなオペレーティング システム上で動作するオープンソースの Xfce ソリューションです。この演習の手順では X2Go を使用することを想定していますが、Xfce をサポートする任意のクライアントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ef869-106">X2Go is a free and open-source Xfce solution that works on a variety of operating systems, including Windows and OS X. The instructions in this exercise assume you are using X2Go, but you may use any client that supports Xfce.</span></span>

1. <span data-ttu-id="ef869-107">Azure portal 内の "data-science-rg" リソース グループに戻ります。</span><span class="sxs-lookup"><span data-stu-id="ef869-107">Return to the "data-science-rg" resource group in the Azure portal.</span></span> <span data-ttu-id="ef869-108">"data-science-vm" リソースをクリックしてポータル内で開きます。</span><span class="sxs-lookup"><span data-stu-id="ef869-108">Click the "data-science-vm" resource to open it in the portal.</span></span>

    ![Data Science VM を開く](../images/open-data-science-vm.png)

    <span data-ttu-id="ef869-110">"Data Science VM を開く"__</span><span class="sxs-lookup"><span data-stu-id="ef869-110">_Opening the Data Science VM_</span></span>

1. <span data-ttu-id="ef869-111">VM 用に示された IP アドレスの上にマウス ポインターを合わせて、表示された **[コピー]** ボタンをクリックし、IP アドレスをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="ef869-111">Hover over the IP address shown for the VM and click the **Copy** button that appears to copy the IP address to the clipboard.</span></span>

    ![VM の IP アドレスのコピー](../images/copy-ip-address.png)

    <span data-ttu-id="ef869-113">"VM の IP アドレスのコピー"__</span><span class="sxs-lookup"><span data-stu-id="ef869-113">_Copying the VM's IP address_</span></span>

1. <span data-ttu-id="ef869-114">X2Go クライアントを起動し、クリップボードの IP アドレスと前の演習で指定したユーザー名を使用して Data Science VM に接続します。</span><span class="sxs-lookup"><span data-stu-id="ef869-114">Start the X2Go client and connect to the Data Science VM using the IP address on the clipboard and the user name you specified in the previous exercise.</span></span> <span data-ttu-id="ef869-115">ポート **22** (SSH 接続に使用される標準ポート) 経由で接続し、セッションの種類に **[XFCE]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="ef869-115">Connect via port **22** (the standard port used for SSH connections), and specify **XFCE** as the session type.</span></span> <span data-ttu-id="ef869-116">**[OK]** ボタンをクリックして、ユーザー設定を確定します。</span><span class="sxs-lookup"><span data-stu-id="ef869-116">Click the **OK** button to confirm your preferences.</span></span>

    ![X2Go との接続](../images/new-session-1.png)

    <span data-ttu-id="ef869-118">"X2Go との接続"__</span><span class="sxs-lookup"><span data-stu-id="ef869-118">_Connecting with X2Go_</span></span>

1. <span data-ttu-id="ef869-119">右側の "新しいセッション" パネルで、リモート デスクトップに使用する解像度を選択します。</span><span class="sxs-lookup"><span data-stu-id="ef869-119">In the "New session" panel on the right, select the resolution that you wish to use for the remote desktop.</span></span> <span data-ttu-id="ef869-120">次に、パネルの上部で **[新しいセッション]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef869-120">Then click **New session** at the top of the panel.</span></span>

    ![新しいセッションの開始](../images/new-session-2.png)

    <span data-ttu-id="ef869-122">"新しいセッションの開始"__</span><span class="sxs-lookup"><span data-stu-id="ef869-122">_Starting a new session_</span></span>

1. <span data-ttu-id="ef869-123">[演習 1](#Exercise1)で指定したパスワードを入力して、**[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ef869-123">Enter the password you specified in [Exercise 1](#Exercise1), and then click the **OK** button.</span></span> <span data-ttu-id="ef869-124">ホスト キーを信頼するかどうかの確認が表示された場合は、**[はい]** を選択してください。</span><span class="sxs-lookup"><span data-stu-id="ef869-124">If asked if you trust the host key, answer **Yes**.</span></span> <span data-ttu-id="ef869-125">また、SSH デーモンが起動しないことを示すエラー メッセージは無視します。</span><span class="sxs-lookup"><span data-stu-id="ef869-125">Also ignore any error messages stating that the SSH daemon could not be started.</span></span>

    ![VM へのログイン](../images/new-session-3.png)

    <span data-ttu-id="ef869-127">"VM へのログイン"__</span><span class="sxs-lookup"><span data-stu-id="ef869-127">_Logging into the VM_</span></span>

1. <span data-ttu-id="ef869-128">リモート デスクトップが表示されるのを待って、以下のように表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ef869-128">Wait for the remote desktop to appear and confirm that it resembles the one below.</span></span>

    > <span data-ttu-id="ef869-129">デスクトップ上のテキストとアイコンが大きすぎる場合は、セッションを終了します。</span><span class="sxs-lookup"><span data-stu-id="ef869-129">If the text and icons on the desktop are too large, terminate the session.</span></span> <span data-ttu-id="ef869-130">"新しいセッション" パネルの右下隅にあるアイコンをクリックして、メニューから **[Session preferences...]\(セッションのユーザー設定\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ef869-130">Click the icon in the lower-right corner of the "New Session" panel and select **Session preferences...** from the menu.</span></span> <span data-ttu-id="ef869-131">"新しいセッション" ダイアログ内の "入力/出力" タブに移動して、ディスプレイ DPI を調整し、新しいセッションを開始します。</span><span class="sxs-lookup"><span data-stu-id="ef869-131">Go to the "Input/Output" tab in the "New session" dialog and adjust the display DPI, and then start a new session.</span></span> <span data-ttu-id="ef869-132">96 DPI から始めて、必要に応じて調整します。</span><span class="sxs-lookup"><span data-stu-id="ef869-132">Start with 96 DPI and adjust as needed.</span></span>

    ![接続完了](../images/ubuntu-desktop.png)

    <span data-ttu-id="ef869-134">_接続完了_</span><span class="sxs-lookup"><span data-stu-id="ef869-134">_Connected!_</span></span>

<span data-ttu-id="ef869-135">これで接続されました。少し時間を取ってデスクトップ上のショートカットの詳細を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="ef869-135">Now that you are connected, take a moment to explore the shortcuts on the desktop.</span></span> <span data-ttu-id="ef869-136">これらは、VM に事前にインストールされている多くのデータ サイエンス ツールへのショートカットです。これには、[Jupyter](http://jupyter.org/)、[R Studio](https://www.rstudio.com/)、[Microsoft Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) などが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef869-136">These are shortcuts to the numerous data-science tools preinstalled in the VM, which include [Jupyter](http://jupyter.org/), [R Studio](https://www.rstudio.com/), and the [Microsoft Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/), among others.</span></span>