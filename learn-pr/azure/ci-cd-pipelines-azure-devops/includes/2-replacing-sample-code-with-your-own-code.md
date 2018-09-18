<span data-ttu-id="dd73e-101">Azure DevOps プロジェクトを作成すると、誰もが最初に尋ねることは､どのようにしてサンプル アプリを自分のアプリに置き換えるのかということです｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-101">After creating an Azure DevOps project, the first thing everyone ask is how do you replace the sample app with your own app?</span></span> <span data-ttu-id="dd73e-102">これ非常に簡単であり､このユニットでは、そのための 2 つの方法を学びます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-102">It's quite simple and in this unit, you will learn two ways to do it.</span></span>

1. <span data-ttu-id="dd73e-103">VSTS git リポジトリ内のコードの､実際のコードへの置き換え</span><span class="sxs-lookup"><span data-stu-id="dd73e-103">Replacing the code in the VSTS git repository with your real code</span></span>

2. <span data-ttu-id="dd73e-104">実際のコードを保持している外部 git リポジトリへビルド パイプラインをポイントする</span><span class="sxs-lookup"><span data-stu-id="dd73e-104">Pointing your build pipeline to an external git repo holding your real code</span></span>

## <a name="replacing-code-in-vsts-git-repository"></a><span data-ttu-id="dd73e-105">VSTS git リポジトリ内のコードの置き換え</span><span class="sxs-lookup"><span data-stu-id="dd73e-105">Replacing code in VSTS git repository</span></span>

<span data-ttu-id="dd73e-106">1 つの簡単な方法は、VSTS 内の git レポジトリのクローンを自分のハードドライブに作成して､あらゆるものを独自のコードに置き換え、VSTS および voila に戻すことです｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-106">One simple way is by cloning the git repo in VSTS onto your hard drive, replacing everything with your own code, uploading back to VSTS and voila.</span></span> <span data-ttu-id="dd73e-107">これでコードがビルドされ、CI/CD パイプライン経由でデプロイされます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-107">Your code will now be built and deployed through the CI/CD pipeline.</span></span>

<span data-ttu-id="dd73e-108">このユニットでは、最初にソースコードをハード ドライブにあるnode.js アプリにダウンロードして保存します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-108">For this unit, you will start by downloading and storing the source code to the node.js app onto your hard drive.</span></span>

1. <span data-ttu-id="dd73e-109"><https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip> からソース コードをダウンロードします｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-109">Download the source code from <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip></span></span>

2. <span data-ttu-id="dd73e-110">MicrosoftLearnDevOps.zip の内容をハード ドライブの適当な場所に抽出します。</span><span class="sxs-lookup"><span data-stu-id="dd73e-110">Extract the contents of MicrosoftLearnDevOps.zip somewhere on your hard drive.</span></span> <span data-ttu-id="dd73e-111">この例では､`C:\users\abel\Downloads\MicrosoftLearnDevOps` を使用しています｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-111">For this example `C:\users\abel\Downloads\MicrosoftLearnDevOps` was used</span></span>  
<span data-ttu-id="dd73e-112">![ディレクトリの解凍](/media-draft/2-unzippedfolder.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-112">![Unzipped Directory](/media-draft/2-unzippedfolder.png)</span></span>

<span data-ttu-id="dd73e-113">次に、ハード ドライブにリポジトリのクローンを作成し､サンプル アプリを実際の node.js アプリに置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd73e-113">Next, you need to clone the repo onto your hard drive and replace the sample app with the real node.js app.</span></span> <span data-ttu-id="dd73e-114">このユニットでは、コンピューターにすでに git がインストールされているものと仮定します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-114">This unit assumes you already have git installed on your computer.</span></span>

1. <span data-ttu-id="dd73e-115">Azure portal から、Azure DevOps プロジェクトを参照し、コード リポジトリのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-115">From Azure portal browse to your Azure DevOps Project and click on the code repository link.</span></span>  
![](/media-draft/2-browsetorepolink.png)

2. <span data-ttu-id="dd73e-116">[Clone] をクリックして、右上にある git リポジトリの URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-116">Click on Clone and copy the url for the git repo in the upper right-hand side.</span></span>  
![クローン用 URL のコピー](/media-draft/2-copycloneurl.png)  
<span data-ttu-id="dd73e-118">これで、リポジトリの URL がクリップボードにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="dd73e-118">This will copy the repo url to your clipboard</span></span>

3. <span data-ttu-id="dd73e-119">ハード ドライブにリポジトリのクローンを作成します。</span><span class="sxs-lookup"><span data-stu-id="dd73e-119">Clone the repo to your hard drive</span></span>  
![Git Clone の作成](/media-draft/2-gitclone.png)  
<span data-ttu-id="dd73e-121">この例では、C:\Users\abel\Source\TripleCrown\DevOps にレポジトリのクローンを作成しています｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-121">In this example the repo was cloned to C:\Users\abel\Source\TripleCrown\DevOps</span></span>

4. <span data-ttu-id="dd73e-122">ローカルのレポジトリから､`.git` ディレクトリ以外のすべてを削除します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-122">Delete everything from your local repo except for the `.git` directory</span></span>  
<span data-ttu-id="dd73e-123">![レポジトリから削除](/media-draft/2-deleterepoofeverything.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-123">![Delete Repo of Everything](/media-draft/2-deleterepoofeverything.png)</span></span>

5. <span data-ttu-id="dd73e-124">ダウンロードした node.js アプリのソース コードをリポジトリ フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-124">Copy the source code for the downloaded node.js app into the repo folder</span></span>  
![コードの置き換え](/media-draft/2-replacedeverything.png)

6. <span data-ttu-id="dd73e-126">コマンド ラインから `git add *` を入力して、ローカル リポジトリにすべての変更を追加します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-126">Add all the changes to your local repo by typing `git add *` from the command line</span></span>  
<span data-ttu-id="dd73e-127">![Git すべて追加](/media-draft/2-gitaddall.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-127">![Git Add All](/media-draft/2-gitaddall.png)</span></span>

7. <span data-ttu-id="dd73e-128">`git commit -m "replace sample app with real code"` と入力して、ローカル リポジトリに変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-128">Commit changes to your local repo by typing `git commit -m "replace sample app with real code"`</span></span>  
<span data-ttu-id="dd73e-129">![Git コミット](/media-draft/2-gitcommit.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-129">![Git Commit](/media-draft/2-gitcommit.png)</span></span>

8. <span data-ttu-id="dd73e-130">`git push`を使用して、変更内容を VSTS の git リポジトリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-130">Push changes back to the git repo in VSTS with `git push`</span></span>  
<span data-ttu-id="dd73e-131">![Git プッシュ](/media-draft/2-gitpush.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-131">![Git Push](/media-draft/2-gitpush.png)</span></span>

9. <span data-ttu-id="dd73e-132">変更を VSTS に戻すと、ビルドから実際のアプリ コードが送信されます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-132">After pushing changes back to VSTS, this should send the real app code through the build</span></span>  
<span data-ttu-id="dd73e-133">![ビルドの開始](/media-draft/2-buildkickedoff.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-133">![Build Kicked Off](/media-draft/2-buildkickedoff.png)</span></span>  
<span data-ttu-id="dd73e-134">Azure までの ![Build in Action (ビルドの進捗状況)](/media-draft/2-buildinaction.png) とリリースパイプライン</span><span class="sxs-lookup"><span data-stu-id="dd73e-134">![Build in Action](/media-draft/2-buildinaction.png) and release pipeline all the way to azure</span></span>  
 <span data-ttu-id="dd73e-135">![リリース中](/media-draft/2-releaserunning.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-135">![Release Running](/media-draft/2-releaserunning.png)</span></span>

 <span data-ttu-id="dd73e-136">デプロイが完了すると、Azure portal に戻ることで実際のアプリが展開されたことを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="dd73e-136">After the deployment finishes, you can verify the real app was deployed by going back to the Azure portal</span></span>

 1. <span data-ttu-id="dd73e-137">Azure portal に移動して､ Azure DevOps プロジェクトを参照し、右側にあるデプロイされたアプリをクリックします</span><span class="sxs-lookup"><span data-stu-id="dd73e-137">Go to the Azure portal, browse to your Azure DevOps project, and click on your deployed app on the right-hand side</span></span>  
 ![サンプルのアプリ リンクを起動する](/media-draft/2-launchapp.png)

 2. <span data-ttu-id="dd73e-139">これにより、ブラウザーでアプリが起動します。</span><span class="sxs-lookup"><span data-stu-id="dd73e-139">This launches the running app in your browser</span></span>  
 ![App Running](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a><span data-ttu-id="dd73e-141">外部 git リポジトリの利用</span><span class="sxs-lookup"><span data-stu-id="dd73e-141">Using external git repo</span></span>

<span data-ttu-id="dd73e-142">サンプルコードを実際のアプリ コードに置き換えるもう 1 つの方法は､アプリのコードを保持している外部 git リポジトリへビルド パイプラインをポイントすることです｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-142">Another way to swap out the sample app with your real app code is by pointing the build pipeline to an external git repository that holds your app code.</span></span> <span data-ttu-id="dd73e-143">この例では、実際のアプリのコードを github リポジトリにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-143">For this example, upload the real app code to a github repository.</span></span>

<span data-ttu-id="dd73e-144">実際のコードを github にアップロードしたら､次の操作を行って､その github リポジトリへビルド パイプラインをポイントします｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-144">After uploading the real code to github, do the following to point the build pipeline to this github repository</span></span>

1. <span data-ttu-id="dd73e-145">Azure portal から Azure DevOps プロジェクトを参照し、ビルド リンクをクリックします</span><span class="sxs-lookup"><span data-stu-id="dd73e-145">From the Azure portal, browse to your Azure DevOps project and click on the build link</span></span>  
![Build Link](/media-draft/2-buildlink.png)

2. <span data-ttu-id="dd73e-147">ビルド パイプラインに移動したら､ビルド パイプラインをクリックし、 `Edit` をクリックします｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-147">This takes you to the build pipelines, click your build pipeline and then `Edit`</span></span>  
<span data-ttu-id="dd73e-148">![[Edit Build] をクリックします。](/media-draft/2-clickeditbuildlink.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-148">![Click Edit Build](/media-draft/2-clickeditbuildlink.png)</span></span>

3. <span data-ttu-id="dd73e-149">ビルド エディターに移動したら､`Get sources` をクリックします｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-149">This takes you to the build editor, click on `Get sources`</span></span>  
<span data-ttu-id="dd73e-150">![[Get Source] をクリックします。](/media-draft/2-clickgetsource.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-150">![Click Get Source](/media-draft/2-clickgetsource.png)</span></span>

4. <span data-ttu-id="dd73e-151">Select a source page に移動します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-151">This takes you to the Select a source page.</span></span> <span data-ttu-id="dd73e-152">ビルド パイプラインでは､VSTS の Git ばかりでなく､ GitHub や GitHub Enterprise、Subversion、Bitbucket Cloud､さらには外部 git に基づくあらゆるレポジトリを利用できることに注意してください｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-152">Notice how you can not only use VSTS Git, but also GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud, and any external Git based repo for the build pipeline.</span></span> <span data-ttu-id="dd73e-153">この課題では、GitHub を選択します。</span><span class="sxs-lookup"><span data-stu-id="dd73e-153">For this exercise, select GitHub</span></span>  
![[GitHub] を選択します。](/media-draft/2-selectgithub.png)

5. <span data-ttu-id="dd73e-155">GitHub の接続ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dd73e-155">This takes you to the GitHub connection page.</span></span> <span data-ttu-id="dd73e-156">OAuth または Personal Access Token のいずれかを使用して、自分の GitHub アカウントに接続します｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-156">Either use OAuth or a Personal Access Token to connect with your GitHub account</span></span>

6. <span data-ttu-id="dd73e-157">実際のアプリのコードを保持している github リポジトリを選択して、`Save & queue` をクリックします｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-157">Select the github repo holding the real app code and click `Save & queue`</span></span>  
<span data-ttu-id="dd73e-158">![[保存] と [Queue (キュー)]](/media-draft/2-saveandqueue.png)</span><span class="sxs-lookup"><span data-stu-id="dd73e-158">![Save and Queue](/media-draft/2-saveandqueue.png)</span></span>

7. <span data-ttu-id="dd73e-159">`Save & queue` ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dd73e-159">Click the `Save & queue` button</span></span>  
![](/media-draft/2-saveandqueuedialog.png)

8. <span data-ttu-id="dd73e-160">この操作によりビルドが保存されて､開始されます｡GitHub にホストされていた実際のアプリ コードがのビルド パイプライン､リリース パイプラインを経由して Azure に送信されます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-160">This action saves and kicks off the build, sending the real app code hosted in GitHub through our build and release pipelines all the way to Azure</span></span>  
![ビルド中](/media-draft/2-buildrunning.png)

## <a name="summary"></a><span data-ttu-id="dd73e-162">まとめ</span><span class="sxs-lookup"><span data-stu-id="dd73e-162">Summary</span></span>

<span data-ttu-id="dd73e-163">このユニットでは、DevOps プロジェクトにあるサンプル コードを実際のアプリ コードに置き換える 2 つの方法を学びました｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-163">In this unit, you learned two different ways to replace the sample code in the DevOps project with real app code.</span></span> <span data-ttu-id="dd73e-164">この置き換えは、VSTS git リポジトリにあるコードを置き換えることでも､あるいはアプリのコードを保持している別の外部レポジトリにビルド パイプラインをリンクさせることによっても行うことができます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-164">This can be done either by replacing the code in the VSTS git repo, or by linking the build pipeline with another external repo which holds your app code.</span></span>

<span data-ttu-id="dd73e-165">この後は、ビルドおよびリリース パイプラインのカスタマイズ方法を学びます｡</span><span class="sxs-lookup"><span data-stu-id="dd73e-165">Next learn how to customize the build and release pipelines.</span></span>