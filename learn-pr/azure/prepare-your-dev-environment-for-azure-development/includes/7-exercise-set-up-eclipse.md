<span data-ttu-id="2232b-101">ここでは、ご利用のコンピューター上に Eclipse と Azure Toolkit をインストールします。</span><span class="sxs-lookup"><span data-stu-id="2232b-101">Here you'll install Eclipse and the Azure Toolkit on your development machine.</span></span> <span data-ttu-id="2232b-102">演習の最後には、Azure に接続する Java アプリケーションを作成するのに必要なものがすべて揃います。</span><span class="sxs-lookup"><span data-stu-id="2232b-102">By the end of the exercise, you'll have everything you need to create a Java application connected to Azure.</span></span>

## <a name="install-eclipse-ide"></a><span data-ttu-id="2232b-103">Eclipse IDE をインストールする</span><span class="sxs-lookup"><span data-stu-id="2232b-103">Install Eclipse IDE</span></span>

1. <span data-ttu-id="2232b-104">[お使いのオペレーティング システムに適した Eclipse IDE](https://www.eclipse.org/downloads/packages/installer) をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="2232b-104">Download the appropriate [Eclipse IDE for your operating system](https://www.eclipse.org/downloads/packages/installer).</span></span>

1. <span data-ttu-id="2232b-105">ダウンロードしたら、Eclipse インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="2232b-105">Start the Eclipse installer once downloaded.</span></span>

    1. <span data-ttu-id="2232b-106">Windows の場合は、ダウンロードしたファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="2232b-106">On Windows, double-click the downloaded file.</span></span>

    1. <span data-ttu-id="2232b-107">macOS と Linux の場合は、ダウンロードしたファイルからインストーラーを展開して実行します。</span><span class="sxs-lookup"><span data-stu-id="2232b-107">On macOS and Linux, unzip the installer from the downloaded file and run it.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2232b-108">Java Development Kit がインストールされていない場合、インストールするように求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="2232b-108">The installer may prompt you to install the Java Development Kit, if it is missing.</span></span>

1. <span data-ttu-id="2232b-109">インストールするパッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="2232b-109">Select the packages to install.</span></span> <span data-ttu-id="2232b-110">Java 開発者の場合は、Java または Java EE Eclipse IDE オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2232b-110">For Java developers, choose either the Java or Java EE Eclipse IDE option.</span></span>

1. <span data-ttu-id="2232b-111">マシン上のインストール先を選択します。</span><span class="sxs-lookup"><span data-stu-id="2232b-111">Select the installation destination on your machine.</span></span>

1. <span data-ttu-id="2232b-112">Eclipse を起動して、正しくインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2232b-112">Launch Eclipse to validate that it installed correctly.</span></span>

## <a name="install-azure-toolkit-for-eclipse"></a><span data-ttu-id="2232b-113">Azure Toolkit for Eclipse をインストールする</span><span class="sxs-lookup"><span data-stu-id="2232b-113">Install Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="2232b-114">Azure Toolkit のインストールは、Windows、macOS、 Linux で同じです。</span><span class="sxs-lookup"><span data-stu-id="2232b-114">Installing the Azure Toolkit is the same across Windows, macOS, and Linux.</span></span>

1. <span data-ttu-id="2232b-115">Eclipse を起動します。</span><span class="sxs-lookup"><span data-stu-id="2232b-115">Start Eclipse.</span></span>

1. <span data-ttu-id="2232b-116">**[Help]\(ヘルプ\)** > **[Install New Software]\(新しいソフトウェアのインストール\)** に移動します。</span><span class="sxs-lookup"><span data-stu-id="2232b-116">Go to **Help** > **Install New Software...**.</span></span>

    <span data-ttu-id="2232b-117">次のスクリーンショットは、**[Install New Software]\(新しいソフトウェアのインストール\)** 項目のメニューの場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="2232b-117">The following screenshot shows the menu location of the **Install New Software...** item.</span></span>

    ![Eclipse の [Help]\(ヘルプ\) メニュー内で強調表示されている [Install New Software]\(新しいソフトウェアのインストール\) オプションのスクリーンショット](../media/7-eclipse-install-new-software.png)

1. <span data-ttu-id="2232b-119">**[Available Software]\(使用できるソフトウェア\)** ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="2232b-119">The **Available Software** dialog will open.</span></span> <span data-ttu-id="2232b-120">**[Work with:]\(操作対象:\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2232b-120">In the **Work with:** text box, type `http://dl.microsoft.com/eclipse/` and press Enter.</span></span>

1. <span data-ttu-id="2232b-121">結果の **[Azure Toolkit for Java]** オプションをオンにします。</span><span class="sxs-lookup"><span data-stu-id="2232b-121">In the results, check the **Azure Toolkit for Java** option.</span></span> <span data-ttu-id="2232b-122">**[Contact all update sites during install to find required software]\(インストール時にすべての更新プログラム サイトに接続して必要なソフトウェアを検索する\)** オプションがまだオフではない場合はオフにします。</span><span class="sxs-lookup"><span data-stu-id="2232b-122">Make sure you uncheck the **Contact all update sites during install to find required software** option, if it isn't already.</span></span>

    <span data-ttu-id="2232b-123">次のスクリーンショットは、前述の **[Available Software]\(使用できるソフトウェア\)** のインストール構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="2232b-123">The following screenshot shows the **Available Software** install configuration as described above.</span></span>

    ![Eclipse の [Available Software]\(使用できるソフトウェア\) ウィンドウのスクリーンショット。Azure Toolkit for Java を検索してインストールするために必要な構成が強調表示されています。](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. <span data-ttu-id="2232b-125">**[Next]\(次へ\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2232b-125">Click **Next**.</span></span>

1. <span data-ttu-id="2232b-126">プロンプトが表示されたらライセンス契約を確認して同意し、**[Finish]\(完了\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2232b-126">Review and accept the license agreements when prompted, and click **Finish**.</span></span>

1. <span data-ttu-id="2232b-127">Azure Toolkit が Eclipse によってダウンロードされてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="2232b-127">Eclipse will download and install the Azure Toolkit.</span></span>

1. <span data-ttu-id="2232b-128">必要に応じて Eclipse を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2232b-128">Restart Eclipse if required.</span></span>

1. <span data-ttu-id="2232b-129">Eclipse に **[ツール]** > **[Azure]** メニュー オプションが表示されることを確認して、Azure Toolkit のインストールを検証します。</span><span class="sxs-lookup"><span data-stu-id="2232b-129">Validate the Azure Toolkit installation by verifying that you can find a **Tools** > **Azure** menu option in Eclipse.</span></span>
