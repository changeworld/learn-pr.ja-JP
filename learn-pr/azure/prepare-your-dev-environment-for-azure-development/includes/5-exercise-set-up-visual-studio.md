<span data-ttu-id="e217d-101">ここでは、Visual Studio をご利用の Windows または macOS 開発マシンにインストールします。</span><span class="sxs-lookup"><span data-stu-id="e217d-101">Here, you'll install Visual Studio on either your Windows or your macOS development machine.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="e217d-102">演習の手順</span><span class="sxs-lookup"><span data-stu-id="e217d-102">Exercise steps</span></span>

<span data-ttu-id="e217d-103">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="e217d-103">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="e217d-104">Windows</span><span class="sxs-lookup"><span data-stu-id="e217d-104">Windows</span></span>

1. <span data-ttu-id="e217d-105">Visual Studio のインストーラーを、 https://visualstudio.microsoft.com/downloads/ からダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e217d-105">Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/.</span></span>

1. <span data-ttu-id="e217d-106">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="e217d-106">Run the installer.</span></span>

1. <span data-ttu-id="e217d-107">**[ワークロード]** タブで **Azure 開発**ワークロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e217d-107">On the **Workloads** tab, select the **Azure development** workload.</span></span>

    <span data-ttu-id="e217d-108">次のスクリーンショットは、Visual Studio で Azure を開発するために Visual Studio インストーラーのワークロードが選択されているところを示しています。</span><span class="sxs-lookup"><span data-stu-id="e217d-108">The following screenshot shows the Visual Studio Installer workload selected to allow Azure development within Visual Studio.</span></span>

    ![Azure 開発ワークロードが強調表示された Visual Studio インストーラーのスクリーンショット。](../media/5-select-azure-workload.png)

1. <span data-ttu-id="e217d-110">(省略可能) Azure で使用する Web アプリケーションを作成するために、ASP.NET および Web 開発ワークロードをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e217d-110">(Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.</span></span>

1. <span data-ttu-id="e217d-111">**[インストール]** をクリックし、Visual Studio がインストールされるのを待機します。</span><span class="sxs-lookup"><span data-stu-id="e217d-111">Click **Install**, and wait for Visual Studio to install.</span></span> <span data-ttu-id="e217d-112">Visual Studio が既にインストールされているシステムの場合、このボタンの表示が **[変更]** となることがあります。</span><span class="sxs-lookup"><span data-stu-id="e217d-112">For systems with Visual Studio already installed, this button may say **Modify**.</span></span>

1. <span data-ttu-id="e217d-113">インストールが完了したら、Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="e217d-113">When the installation is complete, open Visual Studio.</span></span>

1. <span data-ttu-id="e217d-114">Visual Studio の [表示] メニューに移動し、**[Cloud Explorer]** オプションがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e217d-114">Go to the View menu in Visual Studio and make sure you have the **Cloud Explorer** option.</span></span>

    <span data-ttu-id="e217d-115">次のスクリーン ショットでは、Azure 開発ワークロードがインストールされている場合に存在する Cloud Explorer のメニュー オプションを示します。</span><span class="sxs-lookup"><span data-stu-id="e217d-115">The following screenshot shows the Cloud Explorer menu option that will be present if you have the Azure development workload installed.</span></span>

    ![Cloud Explorer のメニュー オプションが強調表示された Visual Studio の [表示] メニューのスクリーンショット。](../media/5-verify-cloud-explorer.png)

<span data-ttu-id="e217d-117">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e217d-117">::: zone-end</span></span>

<span data-ttu-id="e217d-118">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="e217d-118">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="e217d-119">macOS</span><span class="sxs-lookup"><span data-stu-id="e217d-119">macOS</span></span>

1. <span data-ttu-id="e217d-120">https://visualstudio.microsoft.com/ に移動し、Visual Studio for Mac インストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e217d-120">Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.</span></span>

1. <span data-ttu-id="e217d-121">VisualStudioInstaller.dmg ファイルをクリックしてインストーラーをマウントし、ロゴをダブルクリックして実行します。</span><span class="sxs-lookup"><span data-stu-id="e217d-121">Click the VisualStudioInstaller.dmg file to mount the installer, then run it by double-clicking the logo.</span></span>

1. <span data-ttu-id="e217d-122">表示されたプライバシーとライセンス条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="e217d-122">Acknowledge the Privacy and License terms when presented.</span></span>

1. <span data-ttu-id="e217d-123">インストーラーにより、どのコンポーネントをインスト―ルするか尋ねられます。</span><span class="sxs-lookup"><span data-stu-id="e217d-123">The installer will ask which components you wish to install.</span></span> <span data-ttu-id="e217d-124">Azure のコンポーネントは既に Visual Studio for Mac の一部ですが、Azure 用の Web エクスペリエンスを開発するには、**.NET Core** プラットフォームをインストールすることが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="e217d-124">Azure components are already part of Visual Studio for Mac, but it is recommended to install the **.NET Core** platform to develop web experiences for Azure.</span></span>

    <span data-ttu-id="e217d-125">次のスクリーン ショットは、Visual Studio for Mac で Azure を開発する機能を追加するために必要な .NET Core プラットフォームを示しています。</span><span class="sxs-lookup"><span data-stu-id="e217d-125">The following screenshot shows the .NET Core platform required to add Azure development capabilities to Visual Studio for Mac.</span></span>

    ![選択されている .NET Core プラットフォーム オプションが強調表示された Visual Studio for Mac インストーラーのスクリーン ショット。](../media/5-vsmac-install-net-core.png)

1. <span data-ttu-id="e217d-127">選択に問題がなければ、**[Install and Update]** \(インストールして更新\) をクリックし、インストーラーが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="e217d-127">Click **Install and Update** once you are happy with the selections, and wait for the installer to complete.</span></span>

1. <span data-ttu-id="e217d-128">必要なアクセス許可を昇格するように求められた場合、それの実行に管理者の資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="e217d-128">If you are prompted to elevate the permissions needed, use your administrator credentials to do so.</span></span>

1. <span data-ttu-id="e217d-129">インストーラーが完了したら、Visual Studio for Mac を開始します。</span><span class="sxs-lookup"><span data-stu-id="e217d-129">Once the installer is complete, start Visual Studio for Mac.</span></span>

<span data-ttu-id="e217d-130">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="e217d-130">::: zone-end</span></span>