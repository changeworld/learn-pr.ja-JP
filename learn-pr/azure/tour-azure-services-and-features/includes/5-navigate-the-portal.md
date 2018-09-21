<span data-ttu-id="ab27a-101">アカウントを作成したら、**Azure portal** にサインインできます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-101">Now that we have an account, we can sign into the **Azure portal**.</span></span> <span data-ttu-id="ab27a-102">ポータルは、作成したすべてのサブスクリプションとリソースを操作できる Web ベースの管理サイトです。</span><span class="sxs-lookup"><span data-stu-id="ab27a-102">The portal is a web-based administration site that lets you interact with all of your subscriptions and resources you have created.</span></span> <span data-ttu-id="ab27a-103">Azure で行うほぼすべてのことを、この Web インターフェイスから実行できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-103">Almost everything you do with Azure can be done through this web interface.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="ab27a-104">Azure portal のレイアウト</span><span class="sxs-lookup"><span data-stu-id="ab27a-104">Azure portal layout</span></span>

<span data-ttu-id="ab27a-105">Azure portal は、Microsoft Azure を制御するための主要な GUI (グラフィカル ユーザー インターフェイス) です。</span><span class="sxs-lookup"><span data-stu-id="ab27a-105">The Azure portal is the primary graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="ab27a-106">管理操作の大半はポータルで実行できます。一般に、ポータルはシングル タスクを実行する場合や、構成オプションの詳細を確認する場合に最適なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ab27a-106">You can carry out the majority of management actions in the portal, and it is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

![Azure portal](../media/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

<span data-ttu-id="ab27a-108">ポータル ビューの残りの部分は、作業中の特定の要素に対応しています。</span><span class="sxs-lookup"><span data-stu-id="ab27a-108">The remainder of the portal view is for the specific elements you are working with.</span></span> <span data-ttu-id="ab27a-109">既定の (メイン) ページは "_ダッシュボード_" です。</span><span class="sxs-lookup"><span data-stu-id="ab27a-109">The default (main) page is the _dashboard_.</span></span> <span data-ttu-id="ab27a-110">これについては後ほど説明しますが、これはリソースのカスタマイズ可能な鳥瞰図を表しています。</span><span class="sxs-lookup"><span data-stu-id="ab27a-110">We'll cover this a bit later, but this represents a customizable birds-eye-view of your resources.</span></span> <span data-ttu-id="ab27a-111">ダッシュボードを使用して、管理する特定のリソースにジャンプしたり、リソース パネルの **[すべてのリソース]** エントリでリソースを検索したりできます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-111">You can use it to jump into specific resources you want to manage, or search for resources with the **All resources** entry in the resource panel.</span></span> <span data-ttu-id="ab27a-112">仮想マシンや Web アプリなどのリソースを管理するときは、そのリソースに関する具体的な情報が表示される "_ブレード_" を使用します。</span><span class="sxs-lookup"><span data-stu-id="ab27a-112">When you are managing a resource, such as a virtual machine or a web app, you will work with a _blade_ that presents specific information about the resource.</span></span>

## <a name="what-is-a-blade"></a><span data-ttu-id="ab27a-113">ブレードとは</span><span class="sxs-lookup"><span data-stu-id="ab27a-113">What is a blade?</span></span>

<span data-ttu-id="ab27a-114">Azure portal では、ナビゲーションにブレード モデルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-114">The Azure portal uses a blades model for navigation.</span></span> <span data-ttu-id="ab27a-115">_ブレード_とはスライド式のパネルであり、ナビゲーション シーケンスにおけるシングル レベルの UI が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-115">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="ab27a-116">たとえば、**仮想マシン**  >  **コンピューティング**  >  **Ubuntu Server** というシーケンスのそれぞれの要素が 1 つのブレードで表されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-116">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** > **Compute** > **Ubuntu Server**.</span></span>

<span data-ttu-id="ab27a-117">各ブレードには、情報と構成可能なオプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ab27a-117">Each blade contains some information and configurable options.</span></span> <span data-ttu-id="ab27a-118">これらのオプションの中には、別のブレードが生成されるものもあります。そのブレードは既存のブレードの右側に表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-118">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="ab27a-119">新しいブレードでは、さらに構成可能なオプションによって別のブレードが生成され、そのブレードからも別のブレードが生成されるというように続きます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-119">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="ab27a-120">すぐに、いくつかのブレードを同時に開くようになります。</span><span class="sxs-lookup"><span data-stu-id="ab27a-120">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="ab27a-121">ブレードを最大化して画面全体に表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-121">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="ab27a-122">新しいブレードは常に所有者の右側に追加されるので、ウィンドウの下部にあるスクロールバーを使って戻ることで、構成内の現在の場所にどのように到達したかを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-122">Since new blades are always added to the right of the owner, you can use the scrollbar at the bottom of the window to go backwards to see how you got to this spot in the configuration.</span></span> <span data-ttu-id="ab27a-123">また、ブレードの上隅にある `X` ボタンをクリックして、ブレードを個別に閉じることもできます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-123">Alternatively, you can close blades individually by clicking the `X` button in the top corner of the blade.</span></span> <span data-ttu-id="ab27a-124">保存していない変更がある場合は、続行すると変更内容が失われることを通知するメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-124">If you have unsaved changes, Azure will prompt you to let you know that the changes will be lost if you continue.</span></span>

## <a name="configuring-settings-in-the-azure-portal"></a><span data-ttu-id="ab27a-125">Azure portal での設定の構成</span><span class="sxs-lookup"><span data-stu-id="ab27a-125">Configuring settings in the Azure portal</span></span>

<span data-ttu-id="ab27a-126">Azure portal には、構成オプションがいくつか表示されます。ほとんどの場合、画面右上のステータス バーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-126">The Azure portal displays several configuration options, mostly in the status bar at the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="ab27a-127">通知</span><span class="sxs-lookup"><span data-stu-id="ab27a-127">Notifications</span></span>

<span data-ttu-id="ab27a-128">ベルのアイコンをクリックすると、**[通知]** ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-128">Clicking the bell icon displays the **Notifications** pane.</span></span> <span data-ttu-id="ab27a-129">このウィンドウには、実行された過去のアクションとそのステータスが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-129">This pane lists the last actions that have been carried out, along with their status.</span></span>

### <a name="cloud-shell"></a><span data-ttu-id="ab27a-130">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="ab27a-130">Cloud Shell</span></span>

<span data-ttu-id="ab27a-131">**Cloud Shell** アイコン (>_) をクリックすると、新しい Azure Cloud Shell セッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-131">If you click the **Cloud Shell** icon (>_), you will create a new Azure Cloud Shell session.</span></span> <span data-ttu-id="ab27a-132">Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーからアクセスできる対話型シェルです。</span><span class="sxs-lookup"><span data-stu-id="ab27a-132">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span> <span data-ttu-id="ab27a-133">Azure Cloud Shell は、作業に最適なシェル エクスペリエンスを選択できる柔軟性を備えています。</span><span class="sxs-lookup"><span data-stu-id="ab27a-133">It provides the flexibility of choosing the shell experience that best suits the way you work.</span></span> <span data-ttu-id="ab27a-134">Linux ユーザーは Bash エクスペリエンスを選ぶことができ、Windows ユーザーは PowerShell を選ぶことができます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-134">Linux users can opt for a Bash experience, while Windows users can opt for PowerShell.</span></span> <span data-ttu-id="ab27a-135">このブラウザーベースのターミナルでは、ポータルに組み込まれているコマンド ライン インターフェイスを使用して、現在のサブスクリプション内のすべての Azure リソースを制御および管理できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-135">This browser-based terminal lets you control and administer all of your Azure resources in the current subscription through a command-line interface built right into the portal.</span></span>

### <a name="settings"></a><span data-ttu-id="ab27a-136">設定</span><span class="sxs-lookup"><span data-stu-id="ab27a-136">Settings</span></span>

<span data-ttu-id="ab27a-137">Azure portal 設定を変更するには、**歯車**アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab27a-137">Click the **gear** icon to change the Azure portal settings.</span></span> <span data-ttu-id="ab27a-138">これらの設定には、以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-138">These settings include:</span></span>

- <span data-ttu-id="ab27a-139">ログアウト時</span><span class="sxs-lookup"><span data-stu-id="ab27a-139">Logout time</span></span>
- <span data-ttu-id="ab27a-140">色とコントラスト テーマ</span><span class="sxs-lookup"><span data-stu-id="ab27a-140">Color and contrast themes</span></span>
- <span data-ttu-id="ab27a-141">(モバイル デバイスへの) トースト通知</span><span class="sxs-lookup"><span data-stu-id="ab27a-141">Toast notifications (to a mobile device)</span></span>
- <span data-ttu-id="ab27a-142">言語と地域の形式</span><span class="sxs-lookup"><span data-stu-id="ab27a-142">Language and regional format</span></span>

![ポータル設定](../media/5-settings-blade.png)

<span data-ttu-id="ab27a-144">設定を変更したら、**[適用]** をクリックして変更を確定します。</span><span class="sxs-lookup"><span data-stu-id="ab27a-144">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="ab27a-145">[フィードバック] ブレード</span><span class="sxs-lookup"><span data-stu-id="ab27a-145">Feedback blade</span></span>

<span data-ttu-id="ab27a-146">**笑顔アイコン**をクリックすると、**[フィードバックの送信]** ブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-146">The **smiley face** icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="ab27a-147">ここで Azure に関するフィードバックを Microsoft に送信できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-147">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="ab27a-148">Microsoft からメールでフィードバックに返答できるようにするかどうかを指定できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ab27a-148">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

### <a name="help-blade"></a><span data-ttu-id="ab27a-149">[ヘルプ] ブレード</span><span class="sxs-lookup"><span data-stu-id="ab27a-149">Help blade</span></span>

<span data-ttu-id="ab27a-150">**疑問符**のアイコンをクリックすると、**[ヘルプ]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-150">Click the **question mark** icon to show the **Help** blade.</span></span> <span data-ttu-id="ab27a-151">ここでは、次のようなオプションから選択します。</span><span class="sxs-lookup"><span data-stu-id="ab27a-151">Here you choose from several options, including:</span></span>

- <span data-ttu-id="ab27a-152">新機能</span><span class="sxs-lookup"><span data-stu-id="ab27a-152">What's new</span></span>
- <span data-ttu-id="ab27a-153">Azure のロードマップ</span><span class="sxs-lookup"><span data-stu-id="ab27a-153">Azure roadmap</span></span>
- <span data-ttu-id="ab27a-154">ガイド付きツアーの起動</span><span class="sxs-lookup"><span data-stu-id="ab27a-154">Launch guided tour</span></span>
- <span data-ttu-id="ab27a-155">キーボード ショートカット</span><span class="sxs-lookup"><span data-stu-id="ab27a-155">Keyboard shortcuts</span></span>
- <span data-ttu-id="ab27a-156">診断の表示</span><span class="sxs-lookup"><span data-stu-id="ab27a-156">Show diagnostics</span></span>
- <span data-ttu-id="ab27a-157">プライバシーと使用条件</span><span class="sxs-lookup"><span data-stu-id="ab27a-157">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="ab27a-158">ディレクトリとサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="ab27a-158">Directory and subscription</span></span>

<span data-ttu-id="ab27a-159">**本とフィルター**のアイコンをクリックすると、**[ディレクトリ + サブスクリプション]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-159">Click the **Book and Filter** icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="ab27a-160">Azure では、1 つのディレクトリに複数のサブスクリプションを関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-160">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="ab27a-161">**[ディレクトリ + サブスクリプション]** ブレードで、サブスクリプションを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-161">On the **Directory + subscription** blade, you can change between subscriptions.</span></span> <span data-ttu-id="ab27a-162">ここでは、サブスクリプションを変更するか、別のディレクトリに変更できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-162">Here, you can change your subscription or change to another directory.</span></span>

![ディレクトリ](../media/5-directory-blade.png)

### <a name="profile-settings"></a><span data-ttu-id="ab27a-164">プロファイルの設定</span><span class="sxs-lookup"><span data-stu-id="ab27a-164">Profile settings</span></span>

<span data-ttu-id="ab27a-165">右上隅にある自分の名前をクリックすると、いくつかのオプションを持つメニューが開きます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-165">If you click on your name in the top right-hand corner, a menu opens with a few options:</span></span>

- <span data-ttu-id="ab27a-166">別のアカウントでサインインするか、完全にサインアウトします。</span><span class="sxs-lookup"><span data-stu-id="ab27a-166">Sign in with another account, or sign out entirely</span></span>
- <span data-ttu-id="ab27a-167">アカウント プロファイルを表示すれば、パスワードを変更できます</span><span class="sxs-lookup"><span data-stu-id="ab27a-167">View your account profile, where you can change your password</span></span>
- <span data-ttu-id="ab27a-168">権限を確認します</span><span class="sxs-lookup"><span data-stu-id="ab27a-168">Check your permissions</span></span>
- <span data-ttu-id="ab27a-169">課金状況を表示します (右側にある [...] ボタンをクリックします)</span><span class="sxs-lookup"><span data-stu-id="ab27a-169">View your bill (click the "..." button on the right-hand side)</span></span>
- <span data-ttu-id="ab27a-170">連絡先情報を更新します (右側にある [...] ボタンをクリックします)。</span><span class="sxs-lookup"><span data-stu-id="ab27a-170">Update your contact information (click the "..." button on the right-hand side)</span></span>

<span data-ttu-id="ab27a-171">**[明細の表示]** をクリックすると、**[コスト管理 + 課金 - 請求書]** ページが表示されます。このページは、Azure におけるコストの発生場所の分析に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-171">If you click "..." and then **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

<span data-ttu-id="ab27a-172">Azure は大規模な製品であり、Azure portal UI (ユーザー インターフェイス) もこれを反映しています。</span><span class="sxs-lookup"><span data-stu-id="ab27a-172">Azure is a large product, and the Azure portal user interface (UI) reflects this.</span></span> <span data-ttu-id="ab27a-173">スライド式ブレードのアプローチにより、さまざまな管理タスク間を簡単に移動できます。</span><span class="sxs-lookup"><span data-stu-id="ab27a-173">The sliding blade approach allows you to navigate back and forth through the various administration tasks with ease.</span></span> <span data-ttu-id="ab27a-174">練習のために、この UI を少し使ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="ab27a-174">Let's experiment a bit with this UI so you get some practice.</span></span>