<span data-ttu-id="5279b-101">Azure DevOps プロジェクトにおけるすぐに使用できるエクスペリエンスでは、選択されたテクノロジに適したビルドとリリース パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="5279b-101">The out of the box experience with an Azure DevOps Project creates build and release pipelines that make sense for the technologies picked.</span></span> <span data-ttu-id="5279b-102">このモジュールでは、Kubernetes クラスターにホストされているコンテナー内で実行される Node.js アプリに適したビルドとリリース パイプラインを作成しました。</span><span class="sxs-lookup"><span data-stu-id="5279b-102">For this module, you created a build and release pipeline that makes sense for a node.js app running in a container hosted in a Kubernetes cluster.</span></span> 

<span data-ttu-id="5279b-103">自分のプロジェクト向けに特定の処理が行われるように、ビルドとリリース パイプラインをカスタマイズしなければならないことはよくあります。</span><span class="sxs-lookup"><span data-stu-id="5279b-103">Often times we need to customize the build and release pipelines to do specific things for our project.</span></span> <span data-ttu-id="5279b-104">VSTS でのビルドとリリース パイプラインは、100% カスタマイズ可能です。</span><span class="sxs-lookup"><span data-stu-id="5279b-104">The build and release pipelines in VSTS are 100% customizable.</span></span> <span data-ttu-id="5279b-105">パイプラインには、必要な任意の処理を行わせることができます。</span><span class="sxs-lookup"><span data-stu-id="5279b-105">You can make the pipelines do whatever you need it to do.</span></span>

<span data-ttu-id="5279b-106">このユニットでは、ビルドとリリース パイプラインをカスタマイズする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5279b-106">In this unit, learn how to customize your build and release pipelines.</span></span>

## <a name="customize-the-build-pipeline"></a><span data-ttu-id="5279b-107">ビルド パイプラインをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="5279b-107">Customize the build pipeline</span></span>

<span data-ttu-id="5279b-108">VSTS のビルド エンジンは単なるタスク ランナーであり、タスクを 1 つずつ順に実行します。</span><span class="sxs-lookup"><span data-stu-id="5279b-108">The build engine in VSTS is just a task runner, doing one task, after another.</span></span> <span data-ttu-id="5279b-109">ビルドをカスタマイズするには、タスクを追加または削除し、タスクに適切なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="5279b-109">To customize the build, you just add or remove tasks and fill out the correct parameters for the task.</span></span>

<span data-ttu-id="5279b-110">VSTS には、すぐに使用できる約 100 個のタスクが付属しています。</span><span class="sxs-lookup"><span data-stu-id="5279b-110">Out of the box, VSTS comes with about 100 tasks that you can use.</span></span> <span data-ttu-id="5279b-111">付属タスクにないことを行う必要がある場合は、マーケットプレースで探してみてください。ダウンロードして使用できるビルドおよびリリース タスクが 700 個以上あります。</span><span class="sxs-lookup"><span data-stu-id="5279b-111">If you need to do something that doesn't exist out of the box, check the marketplace where there are over 700 build and release tasks ready to be downloaded and used.</span></span> <span data-ttu-id="5279b-112">また、独自のカスタム タスクを記述する機能もあります。</span><span class="sxs-lookup"><span data-stu-id="5279b-112">You also have the ability to write your own custom tasks.</span></span>

<span data-ttu-id="5279b-113">このユニットでは、コードのセキュリティ スキャンを行うために、マーケットプレース タスクの WhiteSource Bolt をインストールして、ビルド パイプラインをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="5279b-113">For this unit, you will customize the build pipeline by installing the marketplace tasks WhiteSource Bolt to do security scanning of our code.</span></span>

1. <span data-ttu-id="5279b-114">Azure portal で Azure DevOps プロジェクトを参照し、ビルド定義のリンクをクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-114">Browse to the Azure DevOps Project in the Azure portal and click on the build definition link</span></span>  
![ビルド リンク](/media-draft/3-buildlink.png)

2. <span data-ttu-id="5279b-116">これにより、ビルド パイプラインのページに移動します。</span><span class="sxs-lookup"><span data-stu-id="5279b-116">This takes you to the build pipelines page.</span></span> <span data-ttu-id="5279b-117">ビルドをクリックし、[`Edit`] を選択します</span><span class="sxs-lookup"><span data-stu-id="5279b-117">Click on the build and select `Edit`</span></span>  
<span data-ttu-id="5279b-118">![ビルドを編集する](/media-draft/3-editbuild.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-118">![Edit Build](/media-draft/3-editbuild.png)</span></span>

3. <span data-ttu-id="5279b-119">これにより、ビルド定義に移動します。</span><span class="sxs-lookup"><span data-stu-id="5279b-119">This takes you to the build definition.</span></span> <span data-ttu-id="5279b-120">[`+`] をクリックして、Agent Job 1 にタスクを追加します</span><span class="sxs-lookup"><span data-stu-id="5279b-120">Click on the `+` to add a task to Agent Job 1</span></span>  
<span data-ttu-id="5279b-121">![タスクを追加する](/media-draft/3-addtask.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-121">![Add Task](/media-draft/3-addtask.png)</span></span>

4. <span data-ttu-id="5279b-122">テキスト フィールドに「`bolt`」と入力し、[`Get it free`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-122">In the text field, type `bolt` and click `Get it free`</span></span>  
<span data-ttu-id="5279b-123">![無料で入手する](/media-draft/3-getitfree.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-123">![Get it free](/media-draft/3-getitfree.png)</span></span>

5. <span data-ttu-id="5279b-124">これにより、WhiteSource Bolt のマーケットプレース ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="5279b-124">This takes you to the WhiteSource Bolt Marketplace page.</span></span> <span data-ttu-id="5279b-125">[`Get it free`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-125">Click `Get it free`</span></span>  
<span data-ttu-id="5279b-126">![White Source Bolt を無料で入手する](/media-draft/3-getwhitesourceboltfree.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-126">![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png)</span></span>

6. <span data-ttu-id="5279b-127">VSTS の組織を選択し、[`Install`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-127">Choose your VSTS organization and then click `Install`</span></span>  
<span data-ttu-id="5279b-128">![インストールする](/media-draft/3-install.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-128">![Install](/media-draft/3-install.png)</span></span>

7. <span data-ttu-id="5279b-129"><https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate> で説明されている手順に従って、WhiteSource Bolt のライセンス認証を行います</span><span class="sxs-lookup"><span data-stu-id="5279b-129">Activate WhiteSource Bolt by following the directions here <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate></span></span>

8. <span data-ttu-id="5279b-130">DevOps プロジェクトが読み込まれている Azure portal に戻り、ビルド パイプラインのリンクをクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-130">Go back to your Azure portal with the DevOps project loaded and click the build pipeline link</span></span>  
![ビルド パイプライン リンク](/media-draft/3-buildpipelinelink.png)

9. <span data-ttu-id="5279b-132">ビルドを選択し、`Edit` をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-132">Select your build and click `Edit`</span></span>  
<span data-ttu-id="5279b-133">![ビルドを編集する](/media-draft/3-editbuild.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-133">![Edit Build](/media-draft/3-editbuild.png)</span></span>

10. <span data-ttu-id="5279b-134">[`+`] をクリックして Agent job 1 にタスクを追加し、検索フィールドに「`bolt`」と入力して、[`Add`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-134">Click the `+` Add a task to Agent job 1, type in `bolt` in the search field and click `Add`</span></span>  
<span data-ttu-id="5279b-135">![Bolt を追加する](/media-draft/3-addbolt.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-135">![Add Bolt](/media-draft/3-addbolt.png)</span></span>

11. <span data-ttu-id="5279b-136">これにより、WhiteSource Bolt タスクがタスクの一覧の一番下に追加されます。これを一番上にドラッグします</span><span class="sxs-lookup"><span data-stu-id="5279b-136">This adds the WhiteSource Bolt task to the bottom of the task list, drag it to the top</span></span>  
![Bolt を一番上に](/media-draft/3-boltattop.png)

12. <span data-ttu-id="5279b-138">[`Save & queue`] をクリックし、[`Save & queue`] を選択します</span><span class="sxs-lookup"><span data-stu-id="5279b-138">Click `Save & queue` and select `Save & queue`</span></span>  
<span data-ttu-id="5279b-139">![保存してキューに登録する](/media-draft/3-saveandqueue.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-139">![Save and Queue](/media-draft/3-saveandqueue.png)</span></span>

13. <span data-ttu-id="5279b-140">[`Save & queue`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-140">Click `Save & queue`</span></span>  
<span data-ttu-id="5279b-141">![[保存してキューに登録] ダイアログ](/media-draft/3-saveandqueuedialog.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-141">![Save and Queue Dialog](/media-draft/3-saveandqueuedialog.png)</span></span>

<span data-ttu-id="5279b-142">これにより、変更したビルド パイプラインが保存され、ビルドがキューに登録されます。</span><span class="sxs-lookup"><span data-stu-id="5279b-142">This saves the modified build pipeline and queues the build.</span></span> <span data-ttu-id="5279b-143">ビルドが完了した後、ビルド `WhiteSource Bolt Build Report` を見ると、ソース コードが WhiteSource Bolt によってスキャンされ、セキュリティの脆弱性が探されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="5279b-143">After the build finishes, looking at the build `WhiteSource Bolt Build Report`, you can see the source code was scanned by WhiteSource Bolt looking for security vulnerabilities.</span></span>

![ビルド レポート](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a><span data-ttu-id="5279b-145">リリース パイプラインをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="5279b-145">Customize the release pipeline</span></span>

<span data-ttu-id="5279b-146">ビルドと同様に、リリース パイプラインもタスク ランナーであり、同じ方法でカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="5279b-146">Like the build, the release pipeline is task runner and can be customized the same way.</span></span> <span data-ttu-id="5279b-147">このユニットでは、リリースの最後に、Web パフォーマンス テストを追加します。</span><span class="sxs-lookup"><span data-stu-id="5279b-147">For this unit, you will add a web performance test at the end of the release.</span></span> <span data-ttu-id="5279b-148">これにより、アプリが Kubernetes クラスターにデプロイされて正常に実行されていることが確認されます。</span><span class="sxs-lookup"><span data-stu-id="5279b-148">This will verifying that your app is deployed and running successfully in the Kubernetes cluster.</span></span>

1. <span data-ttu-id="5279b-149">Azure portal で DevOps プロジェクトを参照し、リリース パイプラインのリンクをクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-149">Browse to the DevOps project in the Azure Portal and click on the link for the release pipeline</span></span>  
![リリース リンク](/media-draft/3-releaselink.png)

2. <span data-ttu-id="5279b-151">これにより、リリース パイプラインのページに移動します。</span><span class="sxs-lookup"><span data-stu-id="5279b-151">This takes you to the release pipeline page.</span></span> <span data-ttu-id="5279b-152">リリース パイプラインをクリックし、[`Edit`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-152">Click your release pipeline and click on `Edit`</span></span>  
<span data-ttu-id="5279b-153">![リリース パイプラインを編集する](/media-draft/3-editreleasepipeline.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-153">![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png)</span></span>

3. <span data-ttu-id="5279b-154">リリース `Dev` ステージのタスクをクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-154">Click on the tasks in your release `Dev` stage</span></span>  
<span data-ttu-id="5279b-155">![リリース ステージ](/media-draft/3-releasestage.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-155">![Release Stage](/media-draft/3-releasestage.png)</span></span>

4. <span data-ttu-id="5279b-156">`+` をクリックして Phase1 にタスクを追加し、検索フィールドに「`web test`」と入力して、Cloud-based Web Performance Test タスクの [`Add`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-156">Click on the `+` Add a task to Phase1, type `web test` in the search field and click `Add` for the Cloud-based Web Performance Test task</span></span>  
<span data-ttu-id="5279b-157">![Web テストを追加する](/media-draft/3-addwebtest.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-157">![Add Web Test](/media-draft/3-addwebtest.png)</span></span>

5. <span data-ttu-id="5279b-158">Quick Web Performance Test タスクをクリックし、[Web サイトの URL] にアプリへの URL を追加することで、タスクを編集します (URL を調べるには、Azure portal の DevOps プロジェクトのページに移動し、右側でサンプル アプリの外部エンドポイントを右クリックしてリンクをコピーします)。次に、TestName として、「`Ping Test`」と入力します</span><span class="sxs-lookup"><span data-stu-id="5279b-158">Edit the Quick Web Performance Test task by clicking on it and adding the url to your app in the Website URL (to find the url, go to the Azure portal DevOps project page and on the right-hand side, right-click your sample app external endpoint and copy link) and then for TestName, enter in `Ping Test`</span></span>  
<span data-ttu-id="5279b-159">![URL をコピーする](/media-draft/3-copyurl.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-159">![Copy URL](/media-draft/3-copyurl.png)</span></span>  
<span data-ttu-id="5279b-160">![Web テスト タスクを編集する](/media-draft/3-editwebtesttask.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-160">![Edit Web Test Task](/media-draft/3-editwebtesttask.png)</span></span>

6. <span data-ttu-id="5279b-161">[`Save`] をクリックします</span><span class="sxs-lookup"><span data-stu-id="5279b-161">Click `Save`</span></span>  
<span data-ttu-id="5279b-162">![リリースを保存する](/media-draft/3-saverelease.png)</span><span class="sxs-lookup"><span data-stu-id="5279b-162">![Save Release](/media-draft/3-saverelease.png)</span></span>

<span data-ttu-id="5279b-163">これで、リリースを実行すると、新しい helm パッケージのデプロイ後に Web テストが実行され、アプリの URL が正常に参照されます。</span><span class="sxs-lookup"><span data-stu-id="5279b-163">Now, when you run a release, after deploying the new helm package, a web test is run hitting the app url successfully.</span></span>

![Web テスト](/media-draft/3-webtest.png)


## <a name="summary"></a><span data-ttu-id="5279b-165">まとめ</span><span class="sxs-lookup"><span data-stu-id="5279b-165">Summary</span></span>

<span data-ttu-id="5279b-166">このユニットでは、ビルドとリリース パイプラインをカスタマイズする方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="5279b-166">In this unit, you learned how to customize your build and release pipelines.</span></span> <span data-ttu-id="5279b-167">また、マーケットプレースのタスクをインストールして使用する方法についても説明しました。</span><span class="sxs-lookup"><span data-stu-id="5279b-167">You also learned how to install and use tasks from the Marketplace.</span></span>