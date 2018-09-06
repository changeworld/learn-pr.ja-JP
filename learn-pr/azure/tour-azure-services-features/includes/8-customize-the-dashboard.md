<span data-ttu-id="23767-101">次に、Azure Portal を使用し、基になっている JSON ファイルを直接編集することによって、ダッシュボードを作成および変更する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="23767-101">Next, let's look at how to create and modify dashboards using the Azure Portal, and by editing the underlying JSON file directly.</span></span>

## <a name="what-is-a-dashboard"></a><span data-ttu-id="23767-102">ダッシュボードとは</span><span class="sxs-lookup"><span data-stu-id="23767-102">What is a dashboard?</span></span>

<span data-ttu-id="23767-103">"_ダッシュボード_" とは、Azure portal に表示される UI タイルのカスタマイズ可能なコレクションです。</span><span class="sxs-lookup"><span data-stu-id="23767-103">A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal.</span></span> <span data-ttu-id="23767-104">タイルを追加、削除、配置して必要なビューを正確に作成してから、そのビューをダッシュボードとして保存します。</span><span class="sxs-lookup"><span data-stu-id="23767-104">You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard.</span></span> <span data-ttu-id="23767-105">複数のダッシュボードがサポートされており、必要に応じてそれらを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="23767-105">Multiple dashboards are supported, and you can switch between them as needed.</span></span> <span data-ttu-id="23767-106">自分のダッシュボードを他のチーム メンバーと共有することさえできます。</span><span class="sxs-lookup"><span data-stu-id="23767-106">You can even share your dashboards with other team members.</span></span>

<span data-ttu-id="23767-107">ダッシュボードを使うと、Azure を非常に柔軟に管理できます。</span><span class="sxs-lookup"><span data-stu-id="23767-107">Dashboards give you considerable flexibility in terms of how you manage Azure.</span></span> <span data-ttu-id="23767-108">たとえば、組織内の特定のロール用のダッシュボードを作成した後、ロールベースのアクセス制御 (RBAC) を使用して、そのダッシュボードにアクセスできるユーザーを制御できます。</span><span class="sxs-lookup"><span data-stu-id="23767-108">For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard.</span></span> <span data-ttu-id="23767-109">したがって、データベース管理者は SQL Database サービスのビューを含むダッシュボードを使用し、Azure Active Directory 管理者は Azure AD 内のユーザーとグループのビューを使用します。</span><span class="sxs-lookup"><span data-stu-id="23767-109">Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD.</span></span>

<span data-ttu-id="23767-110">ダッシュボードは、JavaScript Object Notation (JSON) ファイルとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="23767-110">Dashboards are stored as JavaScript Object Notation (JSON) files.</span></span> <span data-ttu-id="23767-111">つまり、他のコンピューターとの間でアップロードまたはダウンロードしたり、Azure ディレクトリのメンバーと共有したりできます。</span><span class="sxs-lookup"><span data-stu-id="23767-111">This means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory.</span></span> <span data-ttu-id="23767-112">Azure ではダッシュボードをリソース グループ内に格納します。これはポータルで管理できる仮想マシンやストレージ アカウントと同じです。</span><span class="sxs-lookup"><span data-stu-id="23767-112">Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.</span></span>

<span data-ttu-id="23767-113">ダッシュボードは JSON ファイルなので、プログラムでカスタマイズすることもでき、非常に強力な管理ツールになります。</span><span class="sxs-lookup"><span data-stu-id="23767-113">Because dashboards are JSON files, you can also customize them programmatically, making them very powerful administrative tools.</span></span> <span data-ttu-id="23767-114">さらに、一部のタイルの種類はクエリに基づくので、ソース データが変更されると自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="23767-114">In addition, some tile types can be query-based so they update automatically when the source data changes.</span></span>


## <a name="default-dashboard"></a><span data-ttu-id="23767-115">既定のダッシュボード</span><span class="sxs-lookup"><span data-stu-id="23767-115">Default dashboard</span></span>

<span data-ttu-id="23767-116">既定のダッシュボードには単に "ダッシュボード" という名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="23767-116">The default dashboard is simply named "Dashboard".</span></span> <span data-ttu-id="23767-117">ポータルにログインすると、5 つの Web パーツを含むこのダッシュボードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-117">When you log into the portal, you are presented with this dashboard containing five web parts.</span></span>

![既定の Web パーツ](../media-draft/4-dashboard-default-webparts.png)

<span data-ttu-id="23767-119">既定の Web パーツは次のとおりです</span><span class="sxs-lookup"><span data-stu-id="23767-119">These default web parts are</span></span>

1. <span data-ttu-id="23767-120">すべてのリソース</span><span class="sxs-lookup"><span data-stu-id="23767-120">All resources</span></span>
2. <span data-ttu-id="23767-121">クイック スタートとチュートリアル</span><span class="sxs-lookup"><span data-stu-id="23767-121">Quickstarts + tutorials</span></span>
3. <span data-ttu-id="23767-122">サービス正常性</span><span class="sxs-lookup"><span data-stu-id="23767-122">Service Health</span></span>
4. <span data-ttu-id="23767-123">マーケットプレース</span><span class="sxs-lookup"><span data-stu-id="23767-123">Marketplace</span></span>
5. <span data-ttu-id="23767-124">Azure の概要</span><span class="sxs-lookup"><span data-stu-id="23767-124">Azure Getting Started</span></span>

## <a name="creating-and-managing-dashboards"></a><span data-ttu-id="23767-125">ダッシュボードの作成と管理</span><span class="sxs-lookup"><span data-stu-id="23767-125">Creating and managing dashboards</span></span>

<span data-ttu-id="23767-126">ダッシュボードの上部にあるコントロールを使用すると、ダッシュボードの作成、アップロード、ダウンロード、編集、共有を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="23767-126">Along the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard.</span></span> <span data-ttu-id="23767-127">また、ダッシュボードの全画面表示への切り替え、複製、または削除もできます。</span><span class="sxs-lookup"><span data-stu-id="23767-127">You can also switch a dashboard to full screen, clone it, or delete it.</span></span>

![ダッシュボードのコントロールをカスタマイズする](../media-draft/7-customise-dashboard-controls.PNG)

### <a name="select-dashboard"></a><span data-ttu-id="23767-129">ダッシュボードを選択する</span><span class="sxs-lookup"><span data-stu-id="23767-129">Select Dashboard</span></span>

<span data-ttu-id="23767-130">左側に、**[ダッシュボードの選択]** ドロップダウン コントロールがあります。</span><span class="sxs-lookup"><span data-stu-id="23767-130">To the left is the **Select Dashboard** drop-down control.</span></span> <span data-ttu-id="23767-131">このコントロールをクリックすると、お使いのアカウントで既に定義されているダッシュボードの中から選択できます。</span><span class="sxs-lookup"><span data-stu-id="23767-131">Clicking this control enables you to select from dashboards that you have already defined for your account.</span></span> <span data-ttu-id="23767-132">このコントロールにより、目的の異なる複数のダッシュボードをシンプルに定義し、そのとき行いたいことに応じてダッシュボードを簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="23767-132">This control makes it simple for you to define multiple dashboards for different purposes and then simply switch from one to another and back again, depending on what you are trying to do at the time.</span></span>

<span data-ttu-id="23767-133">作成したダッシュボードはすべて最初はプライベートであることに注意してください。つまり、作成者だけが表示できます。</span><span class="sxs-lookup"><span data-stu-id="23767-133">Note that any dashboards that you create will initially be private; that is, only you can see them.</span></span> <span data-ttu-id="23767-134">ダッシュボードを企業全体で使用できるようにするには、ダッシュボードを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23767-134">To make a dashboard available across your enterprise, you need to share it.</span></span> <span data-ttu-id="23767-135">ダッシュボードの共有/共有解除について詳しくは、後のセクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="23767-135">See the later section on sharing/unsharing dashboards for more information.</span></span>

### <a name="create-a-new-dashboard"></a><span data-ttu-id="23767-136">新しいダッシュボードを作成する</span><span class="sxs-lookup"><span data-stu-id="23767-136">Create a new dashboard</span></span>

<span data-ttu-id="23767-137">新しいダッシュボードを作成するには、**[新しいダッシュボード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="23767-137">To create a new dashboard, click **New dashboard**.</span></span> <span data-ttu-id="23767-138">タイルが含まれないダッシュボード ワークスペースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-138">The dashboard workspace appears, with no tiles present.</span></span> <span data-ttu-id="23767-139">タイルを好きなように追加、削除、調整することができます。これについて、もう少し詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="23767-139">You can then add, remove and adjust tiles however you like - we'll look more at this in a bit.</span></span> <span data-ttu-id="23767-140">ダッシュボードのカスタマイズが完了したら、**[カスタマイズ完了]** をクリックして保存し、そのダッシュボードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="23767-140">When you are finished customizing the dashboard, click **Done customizing** to save and switch to that dashboard.</span></span>

### <a name="upload-and-download"></a><span data-ttu-id="23767-141">アップロードとダウンロード</span><span class="sxs-lookup"><span data-stu-id="23767-141">Upload and Download</span></span>

<span data-ttu-id="23767-142">**[アップロード]** および **[ダウンロード]** ボタンを使うと、現在のダッシュボードを JSON ファイルとしてダウンロードし、カスタマイズしてから配布およびアップロードするか、他のユーザーにそのファイルをアップロードさせて Azure portal に戻し、現在のダッシュボードを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="23767-142">The **Upload** and **Download** buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.</span></span>

<span data-ttu-id="23767-143">**[ダウンロード]** をクリックすると、現在のダッシュボードが既定のダウンロード フォルダーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="23767-143">If you click **Download**, the current dashboard downloads into your default Downloads folder.</span></span> <span data-ttu-id="23767-144">ダウンロードしたファイルを開いて、JSON コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="23767-144">Opening the downloaded file then shows the JSON code.</span></span>

![ダッシュボードの JSON コード](../media-draft/7-dashboard-json-code.PNG)

<span data-ttu-id="23767-146">そのコードを手動で編集し (たとえば、タイルのサイズを変更)、**[アップロード]** ボタンをクリックして Azure に再びアップロードします。</span><span class="sxs-lookup"><span data-stu-id="23767-146">You can then edit that code manually (for example, by changing tile sizes) and then upload it back to Azure by clicking the **Upload** button.</span></span>

### <a name="edit-a-dashboard"></a><span data-ttu-id="23767-147">ダッシュボードを編集する</span><span class="sxs-lookup"><span data-stu-id="23767-147">Edit a dashboard</span></span>

<span data-ttu-id="23767-148">このトピックの詳細については、"ダッシュボード" に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="23767-148">See the "dashboard" topic for more information on this topic.</span></span>

### <a name="share-or-unshare-a-dashboard"></a><span data-ttu-id="23767-149">ダッシュボードを共有または共有解除する</span><span class="sxs-lookup"><span data-stu-id="23767-149">Share or unshare a dashboard</span></span>

<span data-ttu-id="23767-150">新しく定義したダッシュボードはプライベートであり、自分のアカウントでのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-150">When you define a new dashboard, it is private and visible only to your account.</span></span> <span data-ttu-id="23767-151">他のユーザーも表示できるようにするには、ダッシュボードを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23767-151">To make it visible to others, you need to share a dashboard.</span></span> <span data-ttu-id="23767-152">ただし、他の Azure リソースと同様に、共有ダッシュボードを格納するリソース グループを指定する必要があります (または既存のリソース グループを使用します)。</span><span class="sxs-lookup"><span data-stu-id="23767-152">However, as with any other Azure resource, you need to specify a resource group (or use an existing resource group) to store shared dashboards in.</span></span> <span data-ttu-id="23767-153">既存のリソース グループがない場合は、ユーザーが指定した場所に Azure によって *dashboards* リソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="23767-153">If you do not have an existing resource group, Azure will create a *dashboards* resource group in whichever location you specify.</span></span> <span data-ttu-id="23767-154">既存のリソース グループがある場合は、そのリソース グループを指定してダッシュボードを格納できます。</span><span class="sxs-lookup"><span data-stu-id="23767-154">If you have existing resource groups, you can specify that resource group to store the dashboards.</span></span>

![共有 + アクセス制御 1](../media-draft/7-share-dashboards-default.PNG)

<span data-ttu-id="23767-156">テンプレートを共有すると、2 つ目の **[共有 + アクセス制御]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-156">When you have shared the template, you will see a second **Sharing + access control** blade.</span></span>

![共有 + アクセス制御 2](../media-draft/7-share-dashboards-access-control.PNG)

<span data-ttu-id="23767-158">**[ユーザーの管理]** をクリックして、そのダッシュボードにアクセスできるユーザーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="23767-158">You can then click **Manage users** to specify the users who have access to that dashboard.</span></span>

### <a name="switching-to-a-shared-dashboard"></a><span data-ttu-id="23767-159">共有ダッシュボードへの切り替え</span><span class="sxs-lookup"><span data-stu-id="23767-159">Switching to a shared dashboard</span></span>

<span data-ttu-id="23767-160">共有ダッシュボードに切り替えるには、ダッシュボードの一覧をクリックして、**[すべてのダッシュボードを参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="23767-160">To switch to a shared dashboard, you click on the list of dashboards, and then click **Browse all dashboards**.</span></span>

![すべてのダッシュボードを参照](../media-draft/7-browse-dashboards.PNG)

<span data-ttu-id="23767-162">**[すべてのダッシュボード]** ブレードに共有ダッシュボードの名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-162">You will now see the **All dashboards** blade, with the names of any shared dashboards displayed.</span></span> <span data-ttu-id="23767-163">ダッシュボードをクリックするだけで、Azure portal にそれを適用できます。</span><span class="sxs-lookup"><span data-stu-id="23767-163">Simply click on a dashboard to apply it to the Azure portal.</span></span>

![共有ダッシュボード](../media-draft/7-select-shared-dashboard.png)

### <a name="display-a-dashboard-as-a-full-screen"></a><span data-ttu-id="23767-165">ダッシュボードを全画面で表示する</span><span class="sxs-lookup"><span data-stu-id="23767-165">Display a dashboard as a full screen</span></span>

<span data-ttu-id="23767-166">ダッシュボードを最大の大きさで表示したい場合は、**[全画面表示]** ボタンをクリックすると、ブラウザーのメニューなしで現在のダッシュボードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-166">If you really want the largest dashboard real estate, click the **Full screen** button to display your current dashboard without any browser menus.</span></span> <span data-ttu-id="23767-167">画面の境界の外側に表示されているタイルがある場合は、画面の右側と下部にスライダー バーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-167">If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.</span></span>

<span data-ttu-id="23767-168">全画面表示モードでの作業が終わったら、Esc キーを押すか、画面上部のダッシュボード名の横にある **[全画面表示を終了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="23767-168">When you have finished working in full-screen mode, press the ESC key or click **Exit Full Screen** next to the Dashboard name at the top of the screen.</span></span>

### <a name="clone-a-dashboard"></a><span data-ttu-id="23767-169">ダッシュボードを複製する</span><span class="sxs-lookup"><span data-stu-id="23767-169">Clone a dashboard</span></span>

<span data-ttu-id="23767-170">ダッシュボードを複製すると、"\<ダッシュボード名> の複製" という名前のインスタント コピーが作成されて、そのコピーが現在のダッシュボードになります。</span><span class="sxs-lookup"><span data-stu-id="23767-170">Cloning a dashboard simply creates an instant copy called "Clone of \<dashboard name>" and switches to that copy as the current dashboard.</span></span> <span data-ttu-id="23767-171">複製により、共有する前にダッシュボードを簡単に作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="23767-171">Cloning is also an easy way to create dashboards prior to sharing them.</span></span> <span data-ttu-id="23767-172">たとえば、ほぼ望みどおりのダッシュボードがある場合は、単にそれを複製し、必要な変更を行ってから共有します。</span><span class="sxs-lookup"><span data-stu-id="23767-172">For example, if you have a dashboard that is almost as you want it, simply clone it, make the changes that you need, and then share it.</span></span>

### <a name="delete-a-dashboard"></a><span data-ttu-id="23767-173">ダッシュボードを削除する</span><span class="sxs-lookup"><span data-stu-id="23767-173">Delete a dashboard</span></span>

<span data-ttu-id="23767-174">ダッシュボードを削除すると、使用可能なダッシュボードの一覧から削除されます。</span><span class="sxs-lookup"><span data-stu-id="23767-174">Deleting a dashboard removes it from your list of available dashboards.</span></span> <span data-ttu-id="23767-175">ダッシュボードの削除の確認を求められますが、削除されたダッシュボードを回復する機能はありません。</span><span class="sxs-lookup"><span data-stu-id="23767-175">You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.</span></span>

## <a name="dashboard"></a><span data-ttu-id="23767-176">ダッシュボード</span><span class="sxs-lookup"><span data-stu-id="23767-176">Dashboard</span></span>

<span data-ttu-id="23767-177">JSON ファイルをダウンロードし、ファイル内の値を変更して、Azure にファイルをアップロードして戻すことによってダッシュボードを編集できますが、ユーザー インターフェイスをデザインするのにそれほど直感的な方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="23767-177">Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, that approach isn't terribly intuitive for designing a user interface.</span></span> <span data-ttu-id="23767-178">GUI を使用して現在のダッシュボードを構成するには、**[編集]** ボタンをクリックするか、またはダッシュボードを右クリックして **[編集]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="23767-178">To use the GUI to configure your current dashboard, click the **Edit** button or right-click on the dashboard and click **Edit**.</span></span> <span data-ttu-id="23767-179">ダッシュボードが編集モードに切り替わります。</span><span class="sxs-lookup"><span data-stu-id="23767-179">The dashboard switches to edit mode.</span></span>

![ダッシュボードを編集する](../media-draft/7-edit-dashboard.PNG)

<span data-ttu-id="23767-181">左側のタイル ギャラリーにいくつかのタイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23767-181">On the left-hand side appears the Tile Gallery, with several possible tiles.</span></span> <span data-ttu-id="23767-182">次の条件でタイル ギャラリーをフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="23767-182">You can filter the Tile Gallery by the following criteria:</span></span>

* <span data-ttu-id="23767-183">全般</span><span class="sxs-lookup"><span data-stu-id="23767-183">General</span></span>
* <span data-ttu-id="23767-184">type</span><span class="sxs-lookup"><span data-stu-id="23767-184">Type</span></span>
* <span data-ttu-id="23767-185">Search</span><span class="sxs-lookup"><span data-stu-id="23767-185">Search</span></span>
* <span data-ttu-id="23767-186">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="23767-186">Resource Group</span></span>
* <span data-ttu-id="23767-187">タグ</span><span class="sxs-lookup"><span data-stu-id="23767-187">Tag</span></span>

![タイル ギャラリー](../media-draft/7-tile-gallery.png)

<span data-ttu-id="23767-189">Azure Active Directory、モノのインターネット (IoT)、Microsoft Intune などのカテゴリによって、これらの各オプションをさらに調整できます。</span><span class="sxs-lookup"><span data-stu-id="23767-189">You can also further refine each of these options by category (for example, Azure Active Directory, Internet of Things [IoT], Microsoft Intune, and so on).</span></span>

<span data-ttu-id="23767-190">左側の一覧でタイルを選択し、作業領域にドラッグするだけで、タイルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="23767-190">Adding tiles is simply a question of selecting the tile from the list on the left and then dragging it to the work area.</span></span> <span data-ttu-id="23767-191">その後は、各タイルを移動したり、サイズを変更したり、表示されるデータを変更したりできます。</span><span class="sxs-lookup"><span data-stu-id="23767-191">You can then move each tile about, resize it, or change the data that it displays.</span></span>

<span data-ttu-id="23767-192">編集モードの作業領域は、正方形に分割されています。</span><span class="sxs-lookup"><span data-stu-id="23767-192">The work area in edit mode is divided into squares.</span></span> <span data-ttu-id="23767-193">各タイルは少なくとも 1 つの正方形を占める必要があり、タイルは最も近くて最大のタイル区分線セットにスナップされます。</span><span class="sxs-lookup"><span data-stu-id="23767-193">Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers.</span></span> <span data-ttu-id="23767-194">重なったタイルは邪魔にならないように移動されます。</span><span class="sxs-lookup"><span data-stu-id="23767-194">Any overlapping tiles are moved out of the way.</span></span> <span data-ttu-id="23767-195">タイルを小さくすると、周辺のタイルは再び近くに移動されます。</span><span class="sxs-lookup"><span data-stu-id="23767-195">When you make a tile smaller, the surrounding tiles will move back up against it.</span></span>

### <a name="change-tile-sizes"></a><span data-ttu-id="23767-196">タイルのサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="23767-196">Change tile sizes</span></span>

<span data-ttu-id="23767-197">一部のタイルには設定されたサイズがあり、プログラムによってのみサイズを編集できます。</span><span class="sxs-lookup"><span data-stu-id="23767-197">Some tiles have a set size and you can edit their size only programmatically.</span></span> <span data-ttu-id="23767-198">ただし、右下隅にグレーのボタンがあるタイルは、そのコーナー インジケーターをドラッグすることによって編集できます。</span><span class="sxs-lookup"><span data-stu-id="23767-198">However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.</span></span>

![サイズ変更可能なタイル](../media-draft/7-resizable-tile.png)

<span data-ttu-id="23767-200">または、コンテキスト メニューを右クリックし、希望のサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="23767-200">Alternatively, right-click the context menu and specify the size you want.</span></span>

![タイルのサイズ](../media-draft/7-tile-size.png)

<span data-ttu-id="23767-202">ダッシュボードを作成するには、タイル ギャラリーからワークスペースにタイルを取得し、それらを再調整します。</span><span class="sxs-lookup"><span data-stu-id="23767-202">To create your dashboard, simply pull tiles from the Tile Gallery onto the workspace and then rearrange them.</span></span>

### <a name="change-tile-settings"></a><span data-ttu-id="23767-203">タイルの設定を変更する</span><span class="sxs-lookup"><span data-stu-id="23767-203">Change tile settings</span></span>

<span data-ttu-id="23767-204">一部のタイルには編集可能な設定があります。</span><span class="sxs-lookup"><span data-stu-id="23767-204">Some tiles have editable settings.</span></span> <span data-ttu-id="23767-205">たとえば、時計タイルの場合、ワークスペースにドラッグすると、**[時計の編集]** タイルが開きます。</span><span class="sxs-lookup"><span data-stu-id="23767-205">For example, with the clock tile, when you drag it onto the workspace, it opens the **Edit clock** tile.</span></span> <span data-ttu-id="23767-206">表示するタイム ゾーンと、12 時間または 24 時間のどちらの形式で表示するかを設定できます。</span><span class="sxs-lookup"><span data-stu-id="23767-206">You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.</span></span>

![時計を編集する](../media-draft/7-edit-clock.png)

<span data-ttu-id="23767-208">多国籍企業や大陸横断企業の場合、異なるタイム ゾーンごとにさらに時計を追加できます。</span><span class="sxs-lookup"><span data-stu-id="23767-208">For multi-national or transcontinental companies, you can add clocks, each in a different time zone.</span></span>

### <a name="accepting-your-edits"></a><span data-ttu-id="23767-209">編集内容の受け入れ</span><span class="sxs-lookup"><span data-stu-id="23767-209">Accepting your edits</span></span>

<span data-ttu-id="23767-210">必要に応じてタイルを調整した後は、**[カスタマイズ完了]** をクリックするか、または右クリックして **[カスタマイズ完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="23767-210">When you have arranged the tiles as you want them, either click **Done customizing**, or right-click and then click **Done customizing**.</span></span>

## <a name="edit-a-dashboard-by-changing-the-json-file"></a><span data-ttu-id="23767-211">JSON ファイルを変更することでダッシュボードを編集する</span><span class="sxs-lookup"><span data-stu-id="23767-211">Edit a dashboard by changing the JSON file</span></span>

<span data-ttu-id="23767-212">JSON ファイルを変更することで、ダッシュボードを編集することもできます。</span><span class="sxs-lookup"><span data-stu-id="23767-212">You can also edit a dashboard by changing the JSON file.</span></span> <span data-ttu-id="23767-213">この方法の方がより多くの設定変更オプションがありますが、Azure にファイルをアップロードして戻すまで、変更内容を見ることができません。</span><span class="sxs-lookup"><span data-stu-id="23767-213">This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure.</span></span>

![JSON の設定](../media-draft/7-json-code.png)

<span data-ttu-id="23767-215">上の例では、タイルのサイズを変更するには、**colSpan** 変数と **rowSpan** 変数を編集した後、ファイルを保存し、それを Azure にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="23767-215">In the example above, to change the size of the tile, edit the **colSpan** and **rowSpan** variables, then save the file and upload it back to Azure.</span></span> <span data-ttu-id="23767-216">ファイルを他のユーザーに配布することもできます。</span><span class="sxs-lookup"><span data-stu-id="23767-216">You can also distribute the file to other users.</span></span>

## <a name="reset-a-dashboard"></a><span data-ttu-id="23767-217">ダッシュボードをリセットする</span><span class="sxs-lookup"><span data-stu-id="23767-217">Reset a dashboard</span></span>

<span data-ttu-id="23767-218">ダッシュボードを既定のスタイルにリセットできます。</span><span class="sxs-lookup"><span data-stu-id="23767-218">You can reset any dashboard to the default style.</span></span> <span data-ttu-id="23767-219">編集モードで右クリックして、**[既定の状態にリセット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="23767-219">In edit mode, right-click and select **Reset to default state**.</span></span> <span data-ttu-id="23767-220">ダイアログ ボックスで、そのダッシュボードをリセットすることの確認を求められます。</span><span class="sxs-lookup"><span data-stu-id="23767-220">A dialog box will ask you to confirm that you want to reset that dashboard.</span></span>

## <a name="summary"></a><span data-ttu-id="23767-221">まとめ</span><span class="sxs-lookup"><span data-stu-id="23767-221">Summary</span></span>

<span data-ttu-id="23767-222">ダッシュボードでは、ポータルを通じて Azure サービスのさまざまな側面を管理するための柔軟なツールが提供されます。</span><span class="sxs-lookup"><span data-stu-id="23767-222">Dashboards provide a flexible tool for managing different aspects of Azure services through the Portal.</span></span> <span data-ttu-id="23767-223">サービスの状態を簡単に監視できるようになります。</span><span class="sxs-lookup"><span data-stu-id="23767-223">They make it convenient to monitor the state of your services.</span></span> <span data-ttu-id="23767-224">ダッシュボードは共有できるため、チームの全員が同じデータを見て、重要なコンポーネントの状態を常に認識しているのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="23767-224">Because they are sharable, they help ensure that everyone on your team sees the same data and stays aware of the state of your critical components.</span></span>

