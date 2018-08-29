<span data-ttu-id="cbaa1-101">Azure 製品には深い階層があります。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-101">Azure products have a deep hierarchy.</span></span> <span data-ttu-id="cbaa1-102">たとえば、Linux 仮想マシンを作成する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-102">For example, suppose you needed to create a Linux Virtual Machine.</span></span> <span data-ttu-id="cbaa1-103">**ホーム** **>** **仮想マシン** **>** **コンピューティング** **>** **Ubuntu Server** というレベル間を移動することがあります。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-103">You might navigate through these levels: **Home** **>** **Virtual machines** **>** **Compute** **>** **Ubuntu Server**.</span></span> <span data-ttu-id="cbaa1-104">Azure portal では、このような移動順序が直観的になるようにその UI が最適化されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-104">The Azure portal optimizes its UI to make this type of navigation sequence intuitive.</span></span> <span data-ttu-id="cbaa1-105">ここでは、これを可能にする主要な UI 要素について説明します。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-105">Here, you will survey the key UI elements that make this possible.</span></span> <span data-ttu-id="cbaa1-106">メニューとサブメニューを経由して移動し、ブレードを使用してサービスを見つけて構成します。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-106">You will navigate through menus and sub-menus and use blades to find and configure services.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="cbaa1-107">Azure portal レイアウト</span><span class="sxs-lookup"><span data-stu-id="cbaa1-107">Azure Portal Layout</span></span>

<span data-ttu-id="cbaa1-108">Azure portal は、Microsoft Azure を制御するための主要な GUI (グラフィカル ユーザー インターフェイス) です。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-108">The Azure portal is the main graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="cbaa1-109">管理操作の大多数はポータルで実行できます。ポータルは一般的に、シングル タスクを実行するために最適なインターフェイスであり、構成オプションが詳しく表示される場所です。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-109">You can carry out the large majority of management actions on the portal and the portal is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

<span data-ttu-id="cbaa1-110">ポータルの左側ウィンドウにはリソース ウィンドウがあります。このウィンドウには、主要なリソース タイプが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-110">On the left-hand pane of the portal is the resource pane, which lists the main resource types.</span></span> <span data-ttu-id="cbaa1-111">Azure には、ここに表示されているよりもはるかに多いリソース タイプがあることにご留意ください。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-111">Note that Azure has far many more resource types than just those shown.</span></span>

## <a name="using-blades-in-azure-portal"></a><span data-ttu-id="cbaa1-112">Azure portal でブレードを使用する</span><span class="sxs-lookup"><span data-stu-id="cbaa1-112">Using Blades in Azure Portal</span></span>

<span data-ttu-id="cbaa1-113">Azure portal では、ナビゲーションにブレード モデルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-113">The Azure Portal uses a blades model for navigation.</span></span> <span data-ttu-id="cbaa1-114">_ブレード_とはスライド式のパネルであり、ナビゲーション シーケンスにおけるシングル レベルの UI が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-114">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="cbaa1-115">たとえば、**仮想マシン** **>** **コンピューティング** **>** **Ubuntu Server** というシーケンスのそれぞれの要素が 1 つのブレードで表されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-115">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** **>** **Compute** **>** **Ubuntu Server**.</span></span>

<span data-ttu-id="cbaa1-116">UI 内の各ブレードには一般的に構成可能なオプションがたくさん含まれます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-116">Each blade within the UI typically contains a number of configurable options.</span></span> <span data-ttu-id="cbaa1-117">このようなオプションのいくつかにより別のブレードが生成され、そのブレードは既存ブレードの右に表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-117">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="cbaa1-118">新しいブレードでは、さらに構成可能なオプションによって別のブレードが生成され、そのブレードからも別のブレードが生成されるというように続きます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-118">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="cbaa1-119">すぐに、いくつかのブレードを同時に開くようになります。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-119">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="cbaa1-120">ブレードを最大化して画面全体を埋めることもできます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-120">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="cbaa1-121">行った構成変更を保存せずにブレードを閉じようとすると、プロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-121">If you try to close a blade without saving any configuration changes that you have made, then you will receive a prompt.</span></span>

## <a name="configuring-settings-in-azure-portal"></a><span data-ttu-id="cbaa1-122">Azure portal で設定を構成する</span><span class="sxs-lookup"><span data-stu-id="cbaa1-122">Configuring Settings in Azure Portal</span></span>

<span data-ttu-id="cbaa1-123">Azure portal には、構成オプションがいくつか表示されます。ほとんどの場合、画面右上のステータス バーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-123">The Azure portal displays several configuration options, mostly in the status bar to the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="cbaa1-124">通知</span><span class="sxs-lookup"><span data-stu-id="cbaa1-124">Notifications</span></span>

<span data-ttu-id="cbaa1-125">ベルのアイコンをクリックすると、[通知] ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-125">Clicking the bell icon displays the Notifications pane.</span></span> <span data-ttu-id="cbaa1-126">このウィンドウには、実行された過去のアクションとそのステータスが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-126">This pane lists the last actions that have been carried out, along with their status.</span></span>

![[通知] ブレード](../images/2-notifications-blade.PNG)

### <a name="cloud-shell"></a><span data-ttu-id="cbaa1-128">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="cbaa1-128">Cloud Shell</span></span>

<span data-ttu-id="cbaa1-129">Cloud Shell アイコン (>_) をクリックすると、Cloud Shell セッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-129">If you click the Cloud Shell icon (>_), you will create a new cloud shell session.</span></span> <span data-ttu-id="cbaa1-130">そのセッションで Linux Bash または PowerShell on Linux を使用するように求められます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-130">You are prompted to use either Linux Bash or PowerShell on Linux in that session.</span></span>

![Cloud Shell](../images/2-choose-shell.PNG)

### <a name="settings"></a><span data-ttu-id="cbaa1-132">設定</span><span class="sxs-lookup"><span data-stu-id="cbaa1-132">Settings</span></span>

<span data-ttu-id="cbaa1-133">Azure portal 設定を変更するには、"歯車" アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-133">Clicking on the "gear" icon to change Azure Portal settings.</span></span> <span data-ttu-id="cbaa1-134">これらの設定には、以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-134">These settings include:</span></span>

* <span data-ttu-id="cbaa1-135">ログアウト時</span><span class="sxs-lookup"><span data-stu-id="cbaa1-135">Logout time</span></span>
* <span data-ttu-id="cbaa1-136">配色</span><span class="sxs-lookup"><span data-stu-id="cbaa1-136">Color scheme</span></span>
* <span data-ttu-id="cbaa1-137">ハイ コントラスト テーマ</span><span class="sxs-lookup"><span data-stu-id="cbaa1-137">High contrast themes</span></span>
* <span data-ttu-id="cbaa1-138">(モバイル デバイスに) トースト通知</span><span class="sxs-lookup"><span data-stu-id="cbaa1-138">Toast notifications (to a mobile device)</span></span>
* <span data-ttu-id="cbaa1-139">ダブルクリックしてテーマを変更する</span><span class="sxs-lookup"><span data-stu-id="cbaa1-139">Double-click to change theme</span></span>
* <span data-ttu-id="cbaa1-140">Language</span><span class="sxs-lookup"><span data-stu-id="cbaa1-140">Language</span></span>
* <span data-ttu-id="cbaa1-141">地域の形式</span><span class="sxs-lookup"><span data-stu-id="cbaa1-141">Regional format</span></span>

![ポータル設定](../images/2-settings-blade.PNG)

<span data-ttu-id="cbaa1-143">設定を変更したら、**[適用]** をクリックして変更を確定します。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-143">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="cbaa1-144">[フィードバック] ブレード</span><span class="sxs-lookup"><span data-stu-id="cbaa1-144">Feedback Blade</span></span>

<span data-ttu-id="cbaa1-145">笑顔アイコンをクリックすると、**[フィードバックの送信]** ブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-145">The smiley face icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="cbaa1-146">ここで Azure に関するフィードバックを Microsoft に送信できます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-146">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="cbaa1-147">Microsoft がメールでフィードバックに回答できるかどうかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-147">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

![フィードバック](../images/2-feedback-blade.PNG)

### <a name="help-blade"></a><span data-ttu-id="cbaa1-149">[ヘルプ] ブレード</span><span class="sxs-lookup"><span data-stu-id="cbaa1-149">Help Blade</span></span>

<span data-ttu-id="cbaa1-150">疑問符をクリックすると、**[ヘルプ]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-150">Click the question mark to show the **Help** blade.</span></span> <span data-ttu-id="cbaa1-151">ここでは、次のようなさまざまなトピックから選択します。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-151">Here you choose from a number of topics, including:</span></span>

* <span data-ttu-id="cbaa1-152">新機能</span><span class="sxs-lookup"><span data-stu-id="cbaa1-152">What's new</span></span>
* <span data-ttu-id="cbaa1-153">Azure のロードマップ</span><span class="sxs-lookup"><span data-stu-id="cbaa1-153">Azure roadmap</span></span>
* <span data-ttu-id="cbaa1-154">ガイド付きツアーの起動</span><span class="sxs-lookup"><span data-stu-id="cbaa1-154">Launch guided tour</span></span>
* <span data-ttu-id="cbaa1-155">キーボード ショートカット</span><span class="sxs-lookup"><span data-stu-id="cbaa1-155">Keyboard shortcuts</span></span>
* <span data-ttu-id="cbaa1-156">診断の表示</span><span class="sxs-lookup"><span data-stu-id="cbaa1-156">Show diagnostics</span></span>
* <span data-ttu-id="cbaa1-157">プライバシーと使用条件</span><span class="sxs-lookup"><span data-stu-id="cbaa1-157">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="cbaa1-158">ディレクトリとサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="cbaa1-158">Directory and Subscription</span></span>

<span data-ttu-id="cbaa1-159">本とフィルターのアイコンをクリックすると、**[ディレクトリとサブスクリプション]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-159">Click the Book and Filter icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="cbaa1-160">Azure では、1 つのディレクトリに複数のサブスクリプションを関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-160">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="cbaa1-161">[ディレクトリとサブスクリプション] ブレードで、サブスクリプションを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-161">In the Directory and Subscription blade, you can change between subscriptions.</span></span> <span data-ttu-id="cbaa1-162">ここでは、サブスクリプションを変更するか、別のディレクトリに変更できます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-162">Here, you can change your subscription, or change to another directory.</span></span>

![Directory](../images/2-directory-blade-1.PNG)

### <a name="profile-settings"></a><span data-ttu-id="cbaa1-164">プロファイルの設定</span><span class="sxs-lookup"><span data-stu-id="cbaa1-164">Profile Settings</span></span>

<span data-ttu-id="cbaa1-165">右上隅にある自分の名前をクリックすると、自分のプロファイル設定を変更できます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-165">If you click on your name in the top right-hand corner, you can then change your profile settings.</span></span>
<span data-ttu-id="cbaa1-166">次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-166">Options include:</span></span>

* <span data-ttu-id="cbaa1-167">Azure からサインアウトする</span><span class="sxs-lookup"><span data-stu-id="cbaa1-167">Sign out of Azure</span></span>
* <span data-ttu-id="cbaa1-168">パスワードの変更</span><span class="sxs-lookup"><span data-stu-id="cbaa1-168">Change password</span></span>
* <span data-ttu-id="cbaa1-169">連絡先情報の変更</span><span class="sxs-lookup"><span data-stu-id="cbaa1-169">Change contact information</span></span>
* <span data-ttu-id="cbaa1-170">アクセス許可の表示</span><span class="sxs-lookup"><span data-stu-id="cbaa1-170">View permissions</span></span>
* <span data-ttu-id="cbaa1-171">Azure チームにアイデアを送信する</span><span class="sxs-lookup"><span data-stu-id="cbaa1-171">Submit an idea to the Azure team</span></span>
* <span data-ttu-id="cbaa1-172">課金状況の表示</span><span class="sxs-lookup"><span data-stu-id="cbaa1-172">View your bill</span></span>
* <span data-ttu-id="cbaa1-173">ディレクトリの切り替え (前のセクションと同じように、[ディレクトリとサブスクリプション] ブレードが表示されます)</span><span class="sxs-lookup"><span data-stu-id="cbaa1-173">Switch directory (shows the Directory + Subscription blade as in the previous section)</span></span>

![プロファイルの設定](../images/2-portal-menu.png)

<span data-ttu-id="cbaa1-175">**[明細の表示]** をクリックすると、**[コスト管理 + 課金 - 請求書]** ページが表示されます。このページは、Azure におけるコストの発生場所の分析に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-175">If you now click **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

![課金ページ](../images/2-portal-billing.PNG)

## <a name="summary"></a><span data-ttu-id="cbaa1-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="cbaa1-177">Summary</span></span>

<span data-ttu-id="cbaa1-178">Azure は大型の製品であり、ポータル UI もそれを反映して大規模になります。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-178">Azure is a large product and the portal UI reflects this.</span></span> <span data-ttu-id="cbaa1-179">このような複雑な環境での操作を支援するポータルの主な手段が、階層を示すブレードです。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-179">The primary way that the portal helps you navigate this complexity is with blades to indicate hierarchy.</span></span> <span data-ttu-id="cbaa1-180">ブレードを使用することで、特定のタスクに集中できるのと同時に、そのポイントまでのパスがわかりやすく示されます。</span><span class="sxs-lookup"><span data-stu-id="cbaa1-180">Blades let you focus on a specific task while clearly indicating the path you took to reach that point.</span></span>