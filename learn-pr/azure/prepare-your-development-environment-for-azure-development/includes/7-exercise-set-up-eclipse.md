<span data-ttu-id="2c6c2-101">この演習では、ローカル マシンに Eclipse をインストールし、次に Azure Toolkit をインストールし、Azure と統合された Java アプリケーションを開発する準備を整えます。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-101">In this exercise, you will install Eclipse on your local machine and then install the Azure Toolkit, preparing you for developing Java applications with Azure integration.</span></span> <span data-ttu-id="2c6c2-102">インストールはすばやくシンプルに実行できます。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-102">The installation is quick and simple.</span></span> <span data-ttu-id="2c6c2-103">この演習を終了すると、Azure の機能とサービスを利用して、最初の Java アプリケーションを起動するために必要なすべての設定が完了します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-103">At the end of the exercise, you will have everything set up that you need to start your first Java application, taking advantage of the features and services of Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="2c6c2-104">Eclipse IDE をインストールする</span><span class="sxs-lookup"><span data-stu-id="2c6c2-104">Install Eclipse IDE</span></span>

1. <span data-ttu-id="2c6c2-105">http://www.eclipse.org/downloads/packages/installer からお使いのオペレーティング システムに適した Eclipse バージョンをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-105">Download the Eclipse version that suits your operating system from http://www.eclipse.org/downloads/packages/installer.</span></span>

1. <span data-ttu-id="2c6c2-106">ダウンロードした Eclipse インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-106">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="2c6c2-107">Windows の場合は、ダウンロードしたファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-107">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="2c6c2-108">macOS と Linux の場合は、ダウンロードしたファイルからインストーラーを展開します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-108">On macOS and Linux, unzip the installer from the downloaded file.</span></span> <span data-ttu-id="2c6c2-109">展開が完了したら、インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-109">Then start the installer once unzipped.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2c6c2-110">Java Development Kit がインストールされていない場合、インストールするように求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-110">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="2c6c2-111">インストールするパッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-111">Select the packages to install.</span></span> <span data-ttu-id="2c6c2-112">Java 開発者の場合は、Java または Java EE Eclipse IDE オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-112">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="2c6c2-113">マシン上のインストール先を選択します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-113">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="2c6c2-114">Eclipse を起動して、正しくインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-114">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="2c6c2-115">Azure Toolkit for Eclipse をインストールする</span><span class="sxs-lookup"><span data-stu-id="2c6c2-115">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2c6c2-116">Azure Toolkit のインストールは、Windows、macOS、Linux で同じです。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-116">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="2c6c2-117">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-117">Start Eclipse.</span></span>

1. <span data-ttu-id="2c6c2-118">**[Help]\(ヘルプ\)** > **[Install New Software]\(新しいソフトウェアのインストール\)** に移動します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-118">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="2c6c2-119">次のスクリーンショットは、**[Install New Software]\(新しいソフトウェアのインストール\)** 項目のメニューの場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-119">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Eclipse の [Help]\(ヘルプ\) メニュー内で強調表示されている [Install New Software]\(新しいソフトウェアのインストール\) オプションのスクリーンショット](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="2c6c2-121">**[Available Software]\(使用できるソフトウェア\)** ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-121">The **Available Software** dialog will open.</span></span> <span data-ttu-id="2c6c2-122">**[Work with:]\(操作対象:\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-122">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="2c6c2-123">結果の **[Azure Toolkit for Java]** オプションをオンにします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-123">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="2c6c2-124">**[Contact all update sites during install to find required software]\(インストール時にすべての更新プログラム サイトに接続して必要なソフトウェアを検索する\)** オプションがまだオフではない場合はオフにします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-124">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="2c6c2-125">次のスクリーンショットは、前述の **[Available Software]\(使用できるソフトウェア\)** のインストール構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-125">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Eclipse の [Available Software]\(使用できるソフトウェア\) ウィンドウのスクリーンショット。Azure Toolkit for Java を検索してインストールするために必要な構成が強調表示されています。](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="2c6c2-127">**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-127">Click **Next**.</span></span>

1. <span data-ttu-id="2c6c2-128">プロンプトが表示されたらライセンス契約を確認して同意し、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-128">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="2c6c2-129">Azure Toolkit が Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-129">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="2c6c2-130">必要に応じて Eclipse を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-130">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="2c6c2-131">Eclipse に **[Tools]\(ツール\)** > **[Azure]** メニュー オプションが表示されることを確認して Azure Toolkit のインストールを検証します。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-131">Validate installation of Azure Toolkit by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>

## <a name="summary"></a><span data-ttu-id="2c6c2-132">まとめ</span><span class="sxs-lookup"><span data-stu-id="2c6c2-132">Summary</span></span>

<span data-ttu-id="2c6c2-133">このユニットでは、Eclipse for Java をインストールし、Azure のサービスと製品との統合を利用する準備をしました。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-133">In this unit, you installed Eclipse for Java, and prepared it to take advantage of the integration with Azure services and products.</span></span> <span data-ttu-id="2c6c2-134">インストールはすばやく簡単に実行できるので、Eclipse は、クラウド サービスと統合された Java 開発のタスクに最適です。</span><span class="sxs-lookup"><span data-stu-id="2c6c2-134">The installation is quick and straightforward, making Eclipse ideal for the task of Java development with cloud services integration.</span></span>
