<span data-ttu-id="95b7b-101">これまで、CI/CD パイプラインの設定を迅速に行うことは常に簡単ではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="95b7b-101">Setting up a CI/CD pipeline has always been challenging to do quickly.</span></span> <span data-ttu-id="95b7b-102">今では、完全なエンド ツー エンドの Azure DevOps プロジェクトをまったく何もない状態から驚くほど簡単に設定できます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-102">Now, it is incredibly easy to go from nothing at all to a full end to end Azure DevOps project.</span></span> <span data-ttu-id="95b7b-103">Azure DevOps プロジェクトでは次のものを作成できます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-103">And in the Azure DevOps project, you get the following:</span></span>

1. <span data-ttu-id="95b7b-104">Azure でプロビジョニングされたインフラストラクチャ。</span><span class="sxs-lookup"><span data-stu-id="95b7b-104">Infrastructure provisioned for you in Azure.</span></span>

2. <span data-ttu-id="95b7b-105">VSTS インスタンス内のチーム プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="95b7b-105">A Team Project in a VSTS instance.</span></span>

3. <span data-ttu-id="95b7b-106">VSTS インスタンスのリポジトリで選択した言語でのサンプル アプリのソース コード。</span><span class="sxs-lookup"><span data-stu-id="95b7b-106">Source code for a sample app in the language that you picked in the repo of your VSTS instance.</span></span>

4. <span data-ttu-id="95b7b-107">選択したテクノロジで意味のあるビルドとリリースのパイプライン。</span><span class="sxs-lookup"><span data-stu-id="95b7b-107">A build and release pipeline that makes sense for the technologies picked.</span></span>

<span data-ttu-id="95b7b-108">また、完了すると、Azure DevOps プロジェクトはサンプル アプリを取得し、Azure でユーザー用にプロビジョニングされたインフラストラクチャに、パイプラインを介してビルドし、リリースします。</span><span class="sxs-lookup"><span data-stu-id="95b7b-108">And when its's done, the Azure DevOps project takes the sample app, builds and releases it through the pipelines into the infrastructure it provisions for you up in Azure.</span></span> <span data-ttu-id="95b7b-109">これらすべてをほんの数回クリックで実行できます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-109">And you get all of this with just a couple of clicks.</span></span>

## <a name="create-an-azure-devops-project"></a><span data-ttu-id="95b7b-110">Azure DevOps プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="95b7b-110">Create an Azure DevOps project</span></span>

<span data-ttu-id="95b7b-111">Azure portal から Azure DevOps プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-111">You create an Azure DevOps project from the Azure portal.</span></span>

1. <span data-ttu-id="95b7b-112">[Azure portal](https://portal.azure.com) に移動し、[`Create a resource`] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95b7b-112">Head to the [Azure Portal](https://portal.azure.com) and click `Create a resource`</span></span>  
![](/media-draft/1-azureportal.png)

2. <span data-ttu-id="95b7b-113">[`DevOps Project`] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95b7b-113">Click `DevOps Project`</span></span>  
<span data-ttu-id="95b7b-114">![[DevOps プロジェクト] を選択する](/media-draft/1-pickdevopsproject.png)</span><span class="sxs-lookup"><span data-stu-id="95b7b-114">![Pick DevOps Project](/media-draft/1-pickdevopsproject.png)</span></span>

3. <span data-ttu-id="95b7b-115">次の画面では、使用する言語を選択します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-115">The next screen is where you get to pick what language you want to use.</span></span> <span data-ttu-id="95b7b-116">.NET、Java、Node、PHP、Python、Ruby、Go を選択できます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-116">Notice how you can choose .NET (of course), java, node, php, python, ruby, and go.</span></span> <span data-ttu-id="95b7b-117">Git リポジトリから独自のコードを取り込むこともできます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-117">You can even bring your own code in from a git repo.</span></span> <span data-ttu-id="95b7b-118">このユニットでは、Node.js アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-118">For this unit, let’s go ahead and create a Node.js app.</span></span> <span data-ttu-id="95b7b-119">[Node.js] をクリックし、[次へ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95b7b-119">Click Node.js and click Next</span></span>  
![言語として Node.js を選択する](/media-draft/1-picknodejsforlang.png)

4. <span data-ttu-id="95b7b-121">次に、使用する Node.js フレームワークの選択を求められます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-121">Next it's going to ask you what node.js framework you want to use.</span></span> <span data-ttu-id="95b7b-122">このユニットでは [シンプルな Node.js アプリ] を選択し、[次へ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95b7b-122">For this unit, pick Simple Node.js app and click Next</span></span>  
![シンプルな Node を選択する](/media-draft/1-picksimplenode.png)

5. <span data-ttu-id="95b7b-124">次に、プロビジョニングしてアプリを実行するインフラストラクチャの選択を求められます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-124">Next, it's going to ask you what infrastructure do you want to provision and run your app in?</span></span> <span data-ttu-id="95b7b-125">このユニットでは、Azure Kubernetes Service を使用して Kubernetes クラスターをプロビジョニングしてそこに展開します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-125">For this unit, let’s provision and deploy into a Kubernetes cluster using Azure Kubernetes Service.</span></span> <span data-ttu-id="95b7b-126">[Kubernetes サービス] を選択して [次へ] をクリックします</span><span class="sxs-lookup"><span data-stu-id="95b7b-126">Select Kubernetes Service and click Next</span></span>  
![Kubernetes を選択する](/media-draft/1-pickkubernetes.png)

6. <span data-ttu-id="95b7b-128">次に、VSTS の新しいインスタンスを作成するか、既存のものを選択できます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-128">And now, you can either create a brand new instance of VSTS or choose an existing one.</span></span> <span data-ttu-id="95b7b-129">また、Kubernetes クラスターを実行する場所と方法を設定します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-129">You also get to set up where and how you want your kubernetes cluster to run.</span></span> <span data-ttu-id="95b7b-130">このユニットでは、[Select new]\(新しい選択\) を選択して新しい VSTS インスタンスを作成し、VSTS インスタンスに一意の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-130">For this unit, go ahead and create a new VSTS instance by choosing Select new and give your VSTS instance a unique name.</span></span> <span data-ttu-id="95b7b-131">プロジェクト名に「Learn」と入力し、Azure サブスクリプションを選択し、クラスター名を LearnCluster にし、場所として米国東部を選択し、[完了] をクリックします</span><span class="sxs-lookup"><span data-stu-id="95b7b-131">Enter Learn for the Project name, pick your azure subscription, name the Cluster Name LearnCluster, select East US for the location, and click Done</span></span>  
![最終確認画面](/media-draft/1-finalconfirmation.png)

<span data-ttu-id="95b7b-133">文字どおりの意味です。</span><span class="sxs-lookup"><span data-stu-id="95b7b-133">And that is literally it!</span></span> <span data-ttu-id="95b7b-134">Azure の処理が終わるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-134">This takes a while so kick back, relax and just let Azure do its thing.</span></span> <span data-ttu-id="95b7b-135">ほとんどの時間は、Azure インフラストラクチャのプロビジョニングと構成に費やされます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-135">Most of the time will be spent provisioning and configuring your Azure infrastructure.</span></span> <span data-ttu-id="95b7b-136">このモジュールでは、これは Azure Kubernetes Service と Azure Container Registry です。</span><span class="sxs-lookup"><span data-stu-id="95b7b-136">For this module, this will be your Azure Kubernetes Service and Azure Container Registry.</span></span>

## <a name="a-lap-around-the-finished-azure-devops-project"></a><span data-ttu-id="95b7b-137">完成した Azure DevOps プロジェクト</span><span class="sxs-lookup"><span data-stu-id="95b7b-137">A lap around the finished Azure DevOps Project</span></span>

<span data-ttu-id="95b7b-138">Azure の処理が終わると、アラートで通知されます</span><span class="sxs-lookup"><span data-stu-id="95b7b-138">When Azure is done, you will be notified in your Alerts</span></span>

1. <span data-ttu-id="95b7b-139">アラートをクリックし、[リソースに移動] をクリックします</span><span class="sxs-lookup"><span data-stu-id="95b7b-139">Click on the alert and then Go to resource</span></span>  
![アラートからリソースに移動する](/media-draft/1-gotoresourcefromalert.png)

2. <span data-ttu-id="95b7b-141">プロビジョニングされたすべてのものが表示されているポータル ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-141">This takes you to a portal blade that displays everything provisioned.</span></span> <span data-ttu-id="95b7b-142">左側は CI/CD パイプラインです。</span><span class="sxs-lookup"><span data-stu-id="95b7b-142">On the left-hand side is your CI/CD pipeline.</span></span> <span data-ttu-id="95b7b-143">コード リポジトリ、ビルド定義、リリース定義があります。</span><span class="sxs-lookup"><span data-stu-id="95b7b-143">You have your code repository, your build definition, and also your release definition.</span></span> <span data-ttu-id="95b7b-144">すべてのリンクは、VSTS のリソースに直接移動するディープ リンクです。</span><span class="sxs-lookup"><span data-stu-id="95b7b-144">All the links are deep links that take you directly into the resource in VSTS.</span></span> <span data-ttu-id="95b7b-145">右側には、Azure に展開されたすべてのインフラストラクチャが表示されます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-145">And on the right-hand side, you see all the infrastructure deployed for you in Azure.</span></span> <span data-ttu-id="95b7b-146">サンプル アプリが既に展開された Kubernetes クラスターと Application Insight もあります。</span><span class="sxs-lookup"><span data-stu-id="95b7b-146">You have your Kubernetes cluster with your sample app already deployed and also application insight.</span></span> <span data-ttu-id="95b7b-147">これらすべてのリンクは、Azure 内のリソースへのディープ リンクです。</span><span class="sxs-lookup"><span data-stu-id="95b7b-147">Once again, all these links are deep links to the resources in Azure.</span></span>  
![ポータル DevOps プロジェクト](/media-draft/1-pickdevopsproject.png)

3. <span data-ttu-id="95b7b-149">ソース コードのリンクをクリックします</span><span class="sxs-lookup"><span data-stu-id="95b7b-149">Click on the link for your source code</span></span>  
![ソースへのリンク](/media-draft/1-linktosource.png)

4. <span data-ttu-id="95b7b-151">VSTS プロジェクトの Git リポジトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-151">This takes you to the git repo in your VSTS project.</span></span> <span data-ttu-id="95b7b-152">これは、helm グラフを含むサンプル Node.js アプリを保持する通常の Git リポジトリであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="95b7b-152">Notice this is just a normal git repo holding the sample node.js app with helm charts.</span></span>  
![VSTS リポジトリ](/media-draft/1-vstsrepo.png)

5. <span data-ttu-id="95b7b-154">ビルドに移動します</span><span class="sxs-lookup"><span data-stu-id="95b7b-154">Go into the builds</span></span>  
![ビルドへのリンク](/media-draft/1-navtobuild.png)

6. <span data-ttu-id="95b7b-156">ビルドをクリックしてから [編集] をクリックして作成されたビルドを編集します</span><span class="sxs-lookup"><span data-stu-id="95b7b-156">Edit the created build by clicking on the build and then click Edit</span></span>  
![](/media-draft/1-editbuildlink.png)

7. <span data-ttu-id="95b7b-157">選択したテクノロジで意味のあるビルド パイプラインが表示されます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-157">You will see a build pipeline that makes sense for the technologies picked.</span></span> <span data-ttu-id="95b7b-158">Kubernetes クラスターに Node.js アプリを作成したので、Docker タスクを使用して Node.js アプリを作成し、イメージ コンテナー イメージを作成し、Helm タスクを使用して Helm パッケージを作成するパイプラインが作成されます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-158">Since we picked a node.js app into a Kubernetes cluster, we get a build pipeline that uses Docker tasks to build the Node.js app, create the image container image and then Helm tasks to create a Helm package.</span></span>  
![ビルド パイプライン](/media-draft/1-buildpipeline.png)

8. <span data-ttu-id="95b7b-160">[`Releases`] をクリックしてリリース パイプラインに移動します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-160">Go to the Release pipeline by clicking `Releases`</span></span>  
<span data-ttu-id="95b7b-161">![リリースに移動する](/media-draft/1-gotoreleases.png)</span><span class="sxs-lookup"><span data-stu-id="95b7b-161">![Go to Releases](/media-draft/1-gotoreleases.png)</span></span>

9. <span data-ttu-id="95b7b-162">作成されたリリース パイプラインが表示されます。</span><span class="sxs-lookup"><span data-stu-id="95b7b-162">You'll see the release pipeline created.</span></span> <span data-ttu-id="95b7b-163">リリースをクリックして [`Edit pipeline`] を選択してパイプラインを編集します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-163">Edit it by clicking on the release and selecting `Edit pipeline`</span></span>  
<span data-ttu-id="95b7b-164">![リリースを編集する](/media-draft/1-editrelease.png)</span><span class="sxs-lookup"><span data-stu-id="95b7b-164">![Edit Release](/media-draft/1-editrelease.png)</span></span>

10. <span data-ttu-id="95b7b-165">Azure では選択したテクノロジで意味のあるリリース パイプラインが作成されています。</span><span class="sxs-lookup"><span data-stu-id="95b7b-165">Azure has created a release pipeline that makes sense for the technologies picked.</span></span> <span data-ttu-id="95b7b-166">Kubernetes クラスターでホストされているコンテナーで実行されている Node アプリ。</span><span class="sxs-lookup"><span data-stu-id="95b7b-166">A node app running in a container hosted in a Kubernetes cluster.</span></span>
<span data-ttu-id="95b7b-167">![リリース パイプライン](/media-draft/1-releasepipeline.png)</span><span class="sxs-lookup"><span data-stu-id="95b7b-167">![Release Pipeline](/media-draft/1-releasepipeline.png)</span></span>

11. <span data-ttu-id="95b7b-168">Azure portal に戻り、Kubernetes サービスの外部エンドポイントをクリックします</span><span class="sxs-lookup"><span data-stu-id="95b7b-168">Go back to the Azure portal and click on the external endpoint for the kubernetes service</span></span>  
![](/media-draft/1-clickonendpoint.png)

12. <span data-ttu-id="95b7b-169">ビルドされて AKS クラスターに展開されたサンプル アプリが表示されます</span><span class="sxs-lookup"><span data-stu-id="95b7b-169">You should see the sample app build and deployed into your AKS cluster</span></span>  
![実行中のアプリ](/media-draft/1-apprunning.png)

## <a name="summary"></a><span data-ttu-id="95b7b-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="95b7b-171">Summary</span></span>

<span data-ttu-id="95b7b-172">このユニットでは、次のもので構成される Azure DevOps プロジェクトを作成しました。</span><span class="sxs-lookup"><span data-stu-id="95b7b-172">In this unit, you created an Azure DevOps project that consists of:</span></span>

1. <span data-ttu-id="95b7b-173">アプリのインフラストラクチャ - Azure Kubernetes Service と Application Insight</span><span class="sxs-lookup"><span data-stu-id="95b7b-173">Infrastructure for your app - Azure Kubernetes Service and Application Insight</span></span>

2. <span data-ttu-id="95b7b-174">VSTS インスタンス内のチーム プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="95b7b-174">A Team Project in a VSTS instance.</span></span>

3. <span data-ttu-id="95b7b-175">VSTS インスタンスのリポジトリ内のコンテナーで実行される Node.js サンプル アプリのソース コード。</span><span class="sxs-lookup"><span data-stu-id="95b7b-175">Source code for a Node.js sample app running in a container in the repo of your VSTS instance.</span></span>

4. <span data-ttu-id="95b7b-176">Azure Kubernetes Service インスタンスで実行される Node.js コンテナー アプリ用のビルドとリリース パイプライン。</span><span class="sxs-lookup"><span data-stu-id="95b7b-176">A build and release pipeline for a Node.js container app running in your Azure Kubernetes Service instance.</span></span>

<span data-ttu-id="95b7b-177">次に、サンプル アプリを実際のアプリに置き換える方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="95b7b-177">Next, learn how to replace the sample app with your real app.</span></span>