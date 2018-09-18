<span data-ttu-id="351c6-101">次に、Azure Portal を使用し、基になっている JSON ファイルを直接編集することによって、ダッシュボードを作成および変更する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="351c6-101">Next, let's look at how to create and modify dashboards using the Azure Portal, and by editing the underlying JSON file directly.</span></span>

## <a name="what-is-a-dashboard"></a><span data-ttu-id="351c6-102">ダッシュボードとは</span><span class="sxs-lookup"><span data-stu-id="351c6-102">What is a dashboard?</span></span>

<span data-ttu-id="351c6-103">"_ダッシュボード_" とは、Azure portal に表示される UI タイルのカスタマイズ可能なコレクションです。</span><span class="sxs-lookup"><span data-stu-id="351c6-103">A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal.</span></span> <span data-ttu-id="351c6-104">タイルを追加、削除、配置して必要なビューを正確に作成してから、そのビューをダッシュボードとして保存します。</span><span class="sxs-lookup"><span data-stu-id="351c6-104">You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard.</span></span> <span data-ttu-id="351c6-105">複数のダッシュボードがサポートされており、必要に応じてそれらを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-105">Multiple dashboards are supported, and you can switch between them as needed.</span></span> <span data-ttu-id="351c6-106">自分のダッシュボードを他のチーム メンバーと共有することもできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-106">You can even share your dashboards with other team members.</span></span>

<span data-ttu-id="351c6-107">ダッシュボードを使用すると、Azure を非常に柔軟に管理できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-107">Dashboards give you considerable flexibility regarding how you manage Azure.</span></span> <span data-ttu-id="351c6-108">たとえば、組織内の特定のロール用のダッシュボードを作成した後、ロールベースのアクセス制御 (RBAC) を使用して、そのダッシュボードにアクセスできるユーザーを制御できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-108">For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard.</span></span> <span data-ttu-id="351c6-109">したがって、データベース管理者は SQL Database サービスのビューを含むダッシュボードを使用し、Azure Active Directory 管理者は Azure AD 内のユーザーとグループのビューを使用します。</span><span class="sxs-lookup"><span data-stu-id="351c6-109">Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD.</span></span> <span data-ttu-id="351c6-110">ポータル内の運用環境と開発環境の間でポータルをカスタマイズし、管理している環境ごとに特定のダッシュボードを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-110">You can even customize the portal between your production and development environments within the portal - creating a specific dashboard for each environment you are managing.</span></span>

<span data-ttu-id="351c6-111">ダッシュボードは、JavaScript Object Notation (JSON) ファイルとして保存されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-111">Dashboards are stored as JavaScript Object Notation (JSON) files.</span></span> <span data-ttu-id="351c6-112">つまり、他のコンピューターとの間でアップロードまたはダウンロードしたり、Azure ディレクトリのメンバーと共有したりできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-112">This means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory.</span></span> <span data-ttu-id="351c6-113">Azure では、ポータルで管理できる仮想マシンやストレージ アカウントと同様に、ダッシュボードはリソース グループ内に格納されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-113">Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.</span></span>

> [!TIP]
> <span data-ttu-id="351c6-114">ダッシュボードは JSON ファイルであるため、[プログラムでカスタマイズする](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically)こともできるので、魅力的な管理ツールとなっています。</span><span class="sxs-lookup"><span data-stu-id="351c6-114">Because dashboards are JSON files, you can also [customize them programmatically](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), making them compelling administrative tools.</span></span> <span data-ttu-id="351c6-115">さらに、一部のタイルの種類はクエリ ベースにすることができるので、ソース データが変更されたときに自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-115">Also, some tile types can be query-based, so they update automatically when the source data changes.</span></span>

## <a name="explore-the-default-dashboard"></a><span data-ttu-id="351c6-116">既定のダッシュボードを探索する</span><span class="sxs-lookup"><span data-stu-id="351c6-116">Explore the default dashboard</span></span>

<span data-ttu-id="351c6-117">既定のダッシュボードには、"ダッシュボード" という名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="351c6-117">The default dashboard is named "Dashboard".</span></span> <span data-ttu-id="351c6-118">ポータルにログインすると、5 つの Web パーツを含むこのダッシュボードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-118">When you log into the portal, you are presented with this dashboard containing five web parts.</span></span>

![既定の Web パーツ](../media-draft/8-dashboard-default-webparts.png)

<span data-ttu-id="351c6-120">既定の Web パーツは次のとおりです</span><span class="sxs-lookup"><span data-stu-id="351c6-120">These default web parts are</span></span>

1. <span data-ttu-id="351c6-121">すべてのリソース</span><span class="sxs-lookup"><span data-stu-id="351c6-121">All resources</span></span>

1. <span data-ttu-id="351c6-122">Azure の概要</span><span class="sxs-lookup"><span data-stu-id="351c6-122">Azure Getting Started</span></span>

1. <span data-ttu-id="351c6-123">クイック スタートとチュートリアル</span><span class="sxs-lookup"><span data-stu-id="351c6-123">Quickstarts + tutorials</span></span>

1. <span data-ttu-id="351c6-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="351c6-124">Marketplace</span></span>

1. <span data-ttu-id="351c6-125">Service Health</span><span class="sxs-lookup"><span data-stu-id="351c6-125">Service Health</span></span>

## <a name="creating-and-managing-dashboards"></a><span data-ttu-id="351c6-126">ダッシュボードの作成と管理</span><span class="sxs-lookup"><span data-stu-id="351c6-126">Creating and managing dashboards</span></span>

<span data-ttu-id="351c6-127">ダッシュボードの上部にあるコントロールを使用すると、ダッシュボードの作成、アップロード、ダウンロード、編集、共有を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-127">Along the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard.</span></span> <span data-ttu-id="351c6-128">また、ダッシュボードの全画面表示への切り替え、複製、または削除もできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-128">You can also switch a dashboard to full screen, clone it, or delete it.</span></span>

![ダッシュボードのコントロールをカスタマイズする](../media-draft/8-customise-dashboard-controls.png)

- [<span data-ttu-id="351c6-130">ダッシュボードを選択する</span><span class="sxs-lookup"><span data-stu-id="351c6-130">Select dashboard</span></span>](#select-dashboard)
- [<span data-ttu-id="351c6-131">新しいダッシュボードを作成する</span><span class="sxs-lookup"><span data-stu-id="351c6-131">Create a new dashboard</span></span>](#create-new)
- [<span data-ttu-id="351c6-132">アップロードとダウンロード</span><span class="sxs-lookup"><span data-stu-id="351c6-132">Upload and Download</span></span>](#upload-download)
- [<span data-ttu-id="351c6-133">編集</span><span class="sxs-lookup"><span data-stu-id="351c6-133">Edit</span></span>](#edit-dashboard)
- [<span data-ttu-id="351c6-134">共有</span><span class="sxs-lookup"><span data-stu-id="351c6-134">Share</span></span>](#share-dashboard)
- [<span data-ttu-id="351c6-135">全画面表示</span><span class="sxs-lookup"><span data-stu-id="351c6-135">Full screen</span></span>](#full-screen)
- [<span data-ttu-id="351c6-136">複製</span><span class="sxs-lookup"><span data-stu-id="351c6-136">Clone</span></span>](#clone-dashboard)
- [<span data-ttu-id="351c6-137">削除</span><span class="sxs-lookup"><span data-stu-id="351c6-137">Delete</span></span>](#delete-dashboard)

<a name="select-dashboard"></a>

## <a name="select-dashboard"></a><span data-ttu-id="351c6-138">ダッシュボードを選択する</span><span class="sxs-lookup"><span data-stu-id="351c6-138">Select dashboard</span></span>

<span data-ttu-id="351c6-139">ツール バーの左端に、**[ダッシュボードの選択]** ドロップダウン コントロールがあります。</span><span class="sxs-lookup"><span data-stu-id="351c6-139">To the far left of the toolbar is the **Select Dashboard** drop-down control.</span></span> <span data-ttu-id="351c6-140">このコントロールをクリックすると、お使いのアカウントで既に定義されているダッシュボードの中から選択できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-140">Clicking this control enables you to select from dashboards that you have already defined for your account.</span></span> <span data-ttu-id="351c6-141">このコントロールにより、目的の異なる複数のダッシュボードを簡単に定義し、その時に行おうとしていることに応じてダッシュボードを簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-141">This control makes it simple for you to define multiple dashboards for different purposes and then switch from one to another and back again, depending on what you are trying to do at the time.</span></span>

<span data-ttu-id="351c6-142">作成したダッシュボードはすべて最初は非公開であることに注意してください。つまり、作成者だけが表示できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-142">Note that any dashboards that you create will initially be private; that is, only you can see them.</span></span> <span data-ttu-id="351c6-143">ダッシュボードを企業全体で使用できるようにするには、ダッシュボードを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="351c6-143">To make a dashboard available across your enterprise, you need to share it.</span></span> <span data-ttu-id="351c6-144">そのオプションについては後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="351c6-144">We'll look at that option shortly.</span></span>

<a name="create-new"></a>

## <a name="create-a-new-dashboard"></a><span data-ttu-id="351c6-145">新しいダッシュボードを作成する</span><span class="sxs-lookup"><span data-stu-id="351c6-145">Create a new dashboard</span></span>

<span data-ttu-id="351c6-146">新しいダッシュボードを作成するには、**[新しいダッシュボード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-146">To create a new dashboard, click **New dashboard**.</span></span> <span data-ttu-id="351c6-147">タイルが含まれないダッシュボード ワークスペースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-147">The dashboard workspace appears, with no tiles present.</span></span> <span data-ttu-id="351c6-148">タイルを好きなように追加、削除、調整することができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-148">You can then add, remove and adjust tiles however you like.</span></span> <span data-ttu-id="351c6-149">ダッシュボードのカスタマイズが完了したら、**[カスタマイズ完了]** をクリックして保存し、そのダッシュボードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="351c6-149">When you are finished customizing the dashboard, click **Done customizing** to save and switch to that dashboard.</span></span>

<a name="upload-download"></a>

## <a name="upload-and-download"></a><span data-ttu-id="351c6-150">アップロードとダウンロード</span><span class="sxs-lookup"><span data-stu-id="351c6-150">Upload and Download</span></span>

<span data-ttu-id="351c6-151">**[アップロード]** および **[ダウンロード]** ボタンを使うと、現在のダッシュボードを JSON ファイルとしてダウンロードし、カスタマイズしてから配布およびアップロードするか、他のユーザーにそのファイルをアップロードさせて Azure portal に戻し、現在のダッシュボードを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-151">The **Upload** and **Download** buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.</span></span>

<span data-ttu-id="351c6-152">**[ダウンロード]** をクリックすると、現在のダッシュボードが既定のダウンロード フォルダーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="351c6-152">If you click **Download**, the current dashboard downloads into your default Downloads folder.</span></span> <span data-ttu-id="351c6-153">ダウンロードしたファイルを開いて、JSON コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="351c6-153">Opening the downloaded file then shows the JSON code.</span></span>

![ダッシュボードの JSON コード](../media-draft/8-dashboard-json-code.png)

<span data-ttu-id="351c6-155">そのコードを手動で編集し (たとえば、タイルのサイズを変更)、**[アップロード]** ボタンをクリックして Azure に再びアップロードします。</span><span class="sxs-lookup"><span data-stu-id="351c6-155">You can then edit that code manually (for example, by changing tile sizes) and then upload it back to Azure by clicking the **Upload** button.</span></span>

<a name="edit-dashboard"></a>

### <a name="edit-a-dashboard"></a><span data-ttu-id="351c6-156">ダッシュボードを編集する</span><span class="sxs-lookup"><span data-stu-id="351c6-156">Edit a dashboard</span></span>

<span data-ttu-id="351c6-157">JSON ファイルをダウンロードし、ファイル内の値を変更して、ファイルを Azure に アップロードすることによってダッシュボードを編集できますが、ユーザー インターフェイスをデザインする場合、この方法は直感的ではありません。</span><span class="sxs-lookup"><span data-stu-id="351c6-157">Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, that approach isn't intuitive for designing a user interface.</span></span> <span data-ttu-id="351c6-158">GUI を使用して現在のダッシュボードを構成するには、いくつかの方法で編集モードにします。</span><span class="sxs-lookup"><span data-stu-id="351c6-158">To use the GUI to configure your current dashboard you can enter edit mode in several ways:</span></span>

1. <span data-ttu-id="351c6-159">**[編集]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-159">Click the **Edit** button</span></span>
1. <span data-ttu-id="351c6-160">ダッシュボードを右クリックし、**[編集]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-160">Right-click on the dashboard and click **Edit**.</span></span> 
1. <span data-ttu-id="351c6-161">ダッシュボードのタイルをポイントします。編集オプションと共に右上隅に `...` メニューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-161">Hover over a tile on the dashboard - a `...` menu will appear on the top/right corner with edit options.</span></span>

<span data-ttu-id="351c6-162">ダッシュボードが編集モードに切り替わります。</span><span class="sxs-lookup"><span data-stu-id="351c6-162">The dashboard switches to edit mode.</span></span>

![ダッシュボードを編集する](../media-draft/8-edit-dashboard.png)

<span data-ttu-id="351c6-164">左側のタイル ギャラリーにいくつかのタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-164">On the left-hand side appears the Tile Gallery, with several possible tiles.</span></span> <span data-ttu-id="351c6-165">カテゴリとリソース別に、タイル ギャラリーをフィルター処理することができます。</span><span class="sxs-lookup"><span data-stu-id="351c6-165">You can filter the Tile Gallery by category and resource type:</span></span>

![タイル ギャラリー](../media-draft/8-tile-gallery.png)

<span data-ttu-id="351c6-167">左側の一覧でタイルを選択し、作業領域にドラッグするだけで、タイルを簡単に追加できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-167">Adding tiles is as easy as selecting the tile from the list on the left and then dragging it to the work area.</span></span> <span data-ttu-id="351c6-168">その後は、各タイルを移動したり、サイズを変更したり、タイルに表示されるデータを変更したりできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-168">You can then move each tile about, resize it, or change the data that it displays.</span></span>

> [!TIP]
> <span data-ttu-id="351c6-169">多くのユーザーが気づいていない便利な機能の 1 つとして、子ブレード上の要素を取得し、ダッシュボードに配置できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-169">One cool feature a lot of people are unaware of is that you can take elements on child blades and put them on your dashboard.</span></span> <span data-ttu-id="351c6-170">項目をポイントし、`...` タイル編集メニューを探します。このメニューに含まれている [ダッシュボードにピン留めする] を使用すると、サービスからタイルをすばやく取得し、ダッシュボードに配置できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-170">Just hover over the item and look for the `...` tile edit menu - this will have a "Pin to Dashboard" option which lets you quickly grab a tile from a service and put it onto the dashboard.</span></span>

<span data-ttu-id="351c6-171">編集モードの作業領域は、正方形に分割されています。</span><span class="sxs-lookup"><span data-stu-id="351c6-171">The work area in edit mode is divided into squares.</span></span> <span data-ttu-id="351c6-172">各タイルは少なくとも 1 つの正方形を占める必要があり、タイルは最も近くて最大のタイル区分線セットにスナップされます。</span><span class="sxs-lookup"><span data-stu-id="351c6-172">Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers.</span></span> <span data-ttu-id="351c6-173">重なったタイルは邪魔にならないように移動されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-173">Any overlapping tiles are moved out of the way.</span></span> <span data-ttu-id="351c6-174">タイルを小さくすると、周辺のタイルは再び近くに移動されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-174">When you make a tile smaller, the surrounding tiles will move back up against it.</span></span>

#### <a name="change-tile-sizes"></a><span data-ttu-id="351c6-175">タイルのサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="351c6-175">Change tile sizes</span></span>

<span data-ttu-id="351c6-176">一部のタイルには設定されたサイズがあり、プログラムによってのみサイズを編集できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-176">Some tiles have a set size, and you can edit their size only programmatically.</span></span> <span data-ttu-id="351c6-177">ただし、右下隅に灰色のボタンがあるタイルは、そのコーナー インジケーターをドラッグすることによって編集できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-177">However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.</span></span>

![サイズ変更可能なタイル](../media-draft/8-resizable-tile.png)

<span data-ttu-id="351c6-179">または、コンテキスト メニューを右クリックし、希望のサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="351c6-179">Alternatively, right-click the context menu and specify the size you want.</span></span>

![タイルのサイズ](../media-draft/8-tile-size.png)

<span data-ttu-id="351c6-181">ダッシュボードを作成するには、タイル ギャラリーからワークスペースにタイルを取得し、それらを再配置します。</span><span class="sxs-lookup"><span data-stu-id="351c6-181">To create your dashboard, pull tiles from the Tile Gallery onto the workspace and then rearrange them.</span></span>

#### <a name="change-tile-settings"></a><span data-ttu-id="351c6-182">タイルの設定を変更する</span><span class="sxs-lookup"><span data-stu-id="351c6-182">Change tile settings</span></span>

<span data-ttu-id="351c6-183">一部のタイルには編集可能な設定があります。</span><span class="sxs-lookup"><span data-stu-id="351c6-183">Some tiles have editable settings.</span></span> <span data-ttu-id="351c6-184">たとえば、時計タイルの場合、ワークスペースにドラッグすると、**[時計の編集]** タイルが開きます。</span><span class="sxs-lookup"><span data-stu-id="351c6-184">For example, with the clock tile, when you drag it onto the workspace, it opens the **Edit clock** tile.</span></span> <span data-ttu-id="351c6-185">表示するタイム ゾーンと、12 時間または 24 時間のどちらの形式で表示するかを設定できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-185">You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.</span></span>

![時計を編集する](../media-draft/8-edit-clock.png)

<span data-ttu-id="351c6-187">多国籍企業や大陸横断企業の場合、異なるタイム ゾーンごとにさらに時計を追加できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-187">For multi-national or transcontinental companies, you can add clocks, each in a different time zone.</span></span>

#### <a name="accepting-your-edits"></a><span data-ttu-id="351c6-188">編集内容の受け入れ</span><span class="sxs-lookup"><span data-stu-id="351c6-188">Accepting your edits</span></span>

<span data-ttu-id="351c6-189">必要に応じてタイルを調整した後は、**[カスタマイズ完了]** をクリックするか、または右クリックして **[カスタマイズ完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-189">When you have arranged the tiles as you want them, either click **Done customizing**, or right-click and then click **Done customizing**.</span></span>

## <a name="edit-a-dashboard-by-changing-the-json-file"></a><span data-ttu-id="351c6-190">JSON ファイルを変更することでダッシュボードを編集する</span><span class="sxs-lookup"><span data-stu-id="351c6-190">Edit a dashboard by changing the JSON file</span></span>

<span data-ttu-id="351c6-191">JSON ファイルを変更することで、ダッシュボードを編集することもできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-191">You can also edit a dashboard by changing the JSON file.</span></span> <span data-ttu-id="351c6-192">この方法の方がより多くの設定変更オプションがありますが、Azure にファイルをアップロードして戻すまで、変更内容を見ることができません。</span><span class="sxs-lookup"><span data-stu-id="351c6-192">This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure.</span></span>

![JSON の設定](../media-draft/8-json-code.png)

<span data-ttu-id="351c6-194">上の例では、タイルのサイズを変更するには、**colSpan** 変数と **rowSpan** 変数を編集した後、ファイルを保存し、それを Azure にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="351c6-194">In the example above, to change the size of the tile, edit the **colSpan** and **rowSpan** variables, then save the file and upload it back to Azure.</span></span> <span data-ttu-id="351c6-195">ファイルを他のユーザーに配布することもできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-195">You can also distribute the file to other users.</span></span>

## <a name="reset-a-dashboard"></a><span data-ttu-id="351c6-196">ダッシュボードをリセットする</span><span class="sxs-lookup"><span data-stu-id="351c6-196">Reset a dashboard</span></span>

<span data-ttu-id="351c6-197">ダッシュボードを既定のスタイルにリセットできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-197">You can reset any dashboard to the default style.</span></span> <span data-ttu-id="351c6-198">編集モードで右クリックして、**[既定の状態にリセット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="351c6-198">In edit mode, right-click and select **Reset to default state**.</span></span> <span data-ttu-id="351c6-199">ダイアログ ボックスで、そのダッシュボードのリセットの確認を求められます。</span><span class="sxs-lookup"><span data-stu-id="351c6-199">A dialog box will ask you to confirm that you want to reset that dashboard.</span></span>

<a name="share-dashboard"></a>

## <a name="share-or-unshare-a-dashboard"></a><span data-ttu-id="351c6-200">ダッシュボードを共有または共有解除する</span><span class="sxs-lookup"><span data-stu-id="351c6-200">Share or unshare a dashboard</span></span>

<span data-ttu-id="351c6-201">新しく定義したダッシュボードはプライベートであり、自分のアカウントでのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-201">When you define a new dashboard, it is private and visible only to your account.</span></span> <span data-ttu-id="351c6-202">他のユーザーも表示できるようにするには、ダッシュボードを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="351c6-202">To make it visible to others, you need to share a dashboard.</span></span> <span data-ttu-id="351c6-203">ただし、他の Azure リソースと同様に、共有ダッシュボードを格納するリソース グループを指定する必要があります (または既存のリソース グループを使用します)。</span><span class="sxs-lookup"><span data-stu-id="351c6-203">However, as with any other Azure resource, you need to specify a resource group (or use an existing resource group) to store shared dashboards in.</span></span> <span data-ttu-id="351c6-204">既存のリソース グループがない場合は、ユーザーが指定した場所に Azure によって *dashboards* リソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-204">If you do not have an existing resource group, Azure will create a *dashboards* resource group in whichever location you specify.</span></span> <span data-ttu-id="351c6-205">既存のリソース グループがある場合は、そのリソース グループを指定してダッシュボードを格納できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-205">If you have existing resource groups, you can specify that resource group to store the dashboards.</span></span>

![共有 + アクセス制御 1](../media-draft/8-share-dashboards-default.png)

<span data-ttu-id="351c6-207">テンプレートを共有すると、2 つ目の **[共有 + アクセス制御]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-207">When you have shared the template, you will see a second **Sharing + access control** blade.</span></span>

![共有 + アクセス制御 2](../media-draft/8-share-dashboards-access-control.png)

<span data-ttu-id="351c6-209">**[ユーザーの管理]** をクリックして、そのダッシュボードにアクセスできるユーザーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="351c6-209">You can then click **Manage users** to specify the users who have access to that dashboard.</span></span>

### <a name="switching-to-a-shared-dashboard"></a><span data-ttu-id="351c6-210">共有ダッシュボードへの切り替え</span><span class="sxs-lookup"><span data-stu-id="351c6-210">Switching to a shared dashboard</span></span>

<span data-ttu-id="351c6-211">共有ダッシュボードに切り替えるには、ダッシュボードの一覧をクリックして、**[すべてのダッシュボードを参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-211">To switch to a shared dashboard, you click on the list of dashboards, and then click **Browse all dashboards**.</span></span>

![すべてのダッシュボードを参照](../media-draft/8-browse-dashboards.png)

<span data-ttu-id="351c6-213">**[すべてのダッシュボード]** ブレードに共有ダッシュボードの名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-213">You will now see the **All dashboards** blade, with the names of any shared dashboards displayed.</span></span> <span data-ttu-id="351c6-214">ダッシュボードをクリックするだけで、Azure portal に適用されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-214">Just click on a dashboard to apply it to the Azure portal.</span></span>

![共有ダッシュボード](../media-draft/8-select-shared-dashboard.png)

<a name="full-screen"></a>

## <a name="display-a-dashboard-as-a-full-screen"></a><span data-ttu-id="351c6-216">ダッシュボードを全画面で表示する</span><span class="sxs-lookup"><span data-stu-id="351c6-216">Display a dashboard as a full screen</span></span>

<span data-ttu-id="351c6-217">ダッシュボードを最大サイズで表示する場合は、**[全画面表示]** ボタンをクリックすると、ブラウザーのメニューなしで現在のダッシュボードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-217">If you want the largest dashboard real estate, click the **Full screen** button to display your current dashboard without any browser menus.</span></span> <span data-ttu-id="351c6-218">画面の境界の外側に表示されているタイルがある場合は、画面の右側と下部にスライダー バーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-218">If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.</span></span>

<span data-ttu-id="351c6-219">全画面表示モードでの作業が終わったら、Esc キーを押すか、画面上部のダッシュボード名の横にある **[全画面表示を終了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="351c6-219">When you have finished working in full-screen mode, press the ESC key or click **Exit Full Screen** next to the Dashboard name at the top of the screen.</span></span>

<a name="clone-dashboard"></a>

## <a name="clone-a-dashboard"></a><span data-ttu-id="351c6-220">ダッシュボードを複製する</span><span class="sxs-lookup"><span data-stu-id="351c6-220">Clone a dashboard</span></span>

<span data-ttu-id="351c6-221">ダッシュボードを複製すると、"\<ダッシュボード名> の複製" という名前のインスタント コピーが作成され、現在のダッシュボードとしてそのコピーに切り替わります。</span><span class="sxs-lookup"><span data-stu-id="351c6-221">Cloning a dashboard creates an instant copy called "Clone of \<dashboard name>" and switches to that copy as the current dashboard.</span></span> <span data-ttu-id="351c6-222">複製により、共有する前にダッシュボードを簡単に作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="351c6-222">Cloning is also an easy way to create dashboards before sharing them.</span></span> <span data-ttu-id="351c6-223">たとえば、ほぼ望みどおりのダッシュボードがある場合は、それを複製し、必要な変更を行ってから共有します。</span><span class="sxs-lookup"><span data-stu-id="351c6-223">For example, if you have a dashboard that is almost as you want it, clone it, make the changes that you need, and then share it.</span></span>

<a name="delete-dashboard"></a>

## <a name="delete-a-dashboard"></a><span data-ttu-id="351c6-224">ダッシュボードを削除する</span><span class="sxs-lookup"><span data-stu-id="351c6-224">Delete a dashboard</span></span>

<span data-ttu-id="351c6-225">ダッシュボードを削除すると、使用可能なダッシュボードの一覧から削除されます。</span><span class="sxs-lookup"><span data-stu-id="351c6-225">Deleting a dashboard removes it from your list of available dashboards.</span></span> <span data-ttu-id="351c6-226">ダッシュボードの削除の確認を求められますが、削除されたダッシュボードを回復する機能はありません。</span><span class="sxs-lookup"><span data-stu-id="351c6-226">You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.</span></span>

<span data-ttu-id="351c6-227">新しいダッシュボードを作成して、これらのオプションを使ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="351c6-227">Let's try out some of these options by creating a new dashboard.</span></span>
