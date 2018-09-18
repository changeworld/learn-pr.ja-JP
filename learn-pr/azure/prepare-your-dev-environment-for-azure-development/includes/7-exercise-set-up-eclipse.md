<span data-ttu-id="58dcf-101">このユニットでは、ローカル コンピューターに Eclipse と Azure Toolkit をインストールします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-101">In this unit, you will install Eclipse and the Azure Toolkit on your local machine.</span></span> <span data-ttu-id="58dcf-102">インストールは短時間で済み、簡単です。</span><span class="sxs-lookup"><span data-stu-id="58dcf-102">The installation is quick and simple.</span></span> <span data-ttu-id="58dcf-103">演習の最後には、Azure で初めての Java アプリケーションを作成するのに必要なものがすべて揃います。</span><span class="sxs-lookup"><span data-stu-id="58dcf-103">At the end of the exercise, you will have everything you need to create your first Java application on Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="58dcf-104">Eclipse IDE をインストールする</span><span class="sxs-lookup"><span data-stu-id="58dcf-104">Install Eclipse IDE</span></span>

1. <span data-ttu-id="58dcf-105">http://www.eclipse.org/downloads/packages/installer からお使いのオペレーティング システムに適した Eclipse バージョンをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-105">Download the Eclipse version that suits your operating system from http://www.eclipse.org/downloads/packages/installer.</span></span>

1. <span data-ttu-id="58dcf-106">ダウンロードした Eclipse インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-106">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="58dcf-107">Windows の場合は、ダウンロードしたファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-107">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="58dcf-108">macOS と Linux の場合は、ダウンロードしたファイルからインストーラーを展開して実行します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-108">On macOS and Linux, unzip the installer from the downloaded file and run it.</span></span>

        > [!NOTE]
        > <span data-ttu-id="58dcf-109">Java Development Kit がインストールされていない場合、インストールするように求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="58dcf-109">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="58dcf-110">インストールするパッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-110">Select the packages to install.</span></span> <span data-ttu-id="58dcf-111">Java 開発者の場合は、Java または Java EE Eclipse IDE オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-111">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="58dcf-112">マシン上のインストール先を選択します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-112">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="58dcf-113">Eclipse を起動して、正しくインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-113">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="58dcf-114">Azure Toolkit for Eclipse をインストールする</span><span class="sxs-lookup"><span data-stu-id="58dcf-114">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="58dcf-115">Azure Toolkit のインストールは、Windows、macOS、 Linux で同じです。</span><span class="sxs-lookup"><span data-stu-id="58dcf-115">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="58dcf-116">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-116">Start Eclipse.</span></span>

1. <span data-ttu-id="58dcf-117">**[Help]\(ヘルプ\)** > **[Install New Software]\(新しいソフトウェアのインストール\)** に移動します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-117">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="58dcf-118">次のスクリーンショットは、**[Install New Software]\(新しいソフトウェアのインストール\)** 項目のメニューの場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="58dcf-118">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Eclipse の [Help]\(ヘルプ\) メニュー内で強調表示されている [Install New Software]\(新しいソフトウェアのインストール\) オプションのスクリーンショット](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="58dcf-120">**[Available Software]\(使用できるソフトウェア\)** ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="58dcf-120">The **Available Software** dialog will open.</span></span> <span data-ttu-id="58dcf-121">**[Work with:]\(操作対象:\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-121">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="58dcf-122">結果の **[Azure Toolkit for Java]** オプションをオンにします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-122">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="58dcf-123">**[Contact all update sites during install to find required software]\(インストール時にすべての更新プログラム サイトに接続して必要なソフトウェアを検索する\)** オプションがまだオフではない場合はオフにします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-123">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="58dcf-124">次のスクリーンショットは、前述の **[Available Software]\(使用できるソフトウェア\)** のインストール構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="58dcf-124">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Eclipse の [Available Software]\(使用できるソフトウェア\) ウィンドウのスクリーンショット。Azure Toolkit for Java を検索してインストールするために必要な構成が強調表示されています。](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="58dcf-126">**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-126">Click **Next**.</span></span>

1. <span data-ttu-id="58dcf-127">プロンプトが表示されたらライセンス契約を確認して同意し、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="58dcf-127">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="58dcf-128">Azure Toolkit が Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="58dcf-128">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="58dcf-129">必要に応じて Eclipse を再起動します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-129">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="58dcf-130">Eclipse に **[ツール]** > **[Azure]** メニュー オプションが表示されることを確認して、Azure Toolkit のインストールを検証します。</span><span class="sxs-lookup"><span data-stu-id="58dcf-130">Validate the Azure Toolkit installation by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>

## <a name="summary"></a><span data-ttu-id="58dcf-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="58dcf-131">Summary</span></span>

<span data-ttu-id="58dcf-132">このユニットでは、Eclipse をインストールし、Azure のサービスと製品との統合を利用する準備をしました。</span><span class="sxs-lookup"><span data-stu-id="58dcf-132">In this unit, you installed Eclipse and prepared it to take advantage of the integration with Azure services and products.</span></span>