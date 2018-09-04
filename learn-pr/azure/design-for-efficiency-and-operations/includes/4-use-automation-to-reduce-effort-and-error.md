<span data-ttu-id="17aa6-101">どのような種類のワークロードであっても、そのインフラストラクチャの管理には構成タスクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-101">Managing the infrastructure of any type of workload involves configuration tasks.</span></span> <span data-ttu-id="17aa6-102">この構成は手動でもできますが、手動手順は手間がかかり、エラーが発生しやすく、非効率的です。</span><span class="sxs-lookup"><span data-stu-id="17aa6-102">This configuration can be done manually, but manual steps can be labor-intensive, error prone, and inefficient.</span></span> <span data-ttu-id="17aa6-103">何百ものシステムを Azure に展開する必要のあるプロジェクトのリーダーを任されたらどうしますか。</span><span class="sxs-lookup"><span data-stu-id="17aa6-103">What if you are assigned to lead a project that required the deployment of hundreds of systems on Azure?</span></span> <span data-ttu-id="17aa6-104">このようなリソースをどのようにして構築して構成しますか。</span><span class="sxs-lookup"><span data-stu-id="17aa6-104">How would you build and configure these resources?</span></span> <span data-ttu-id="17aa6-105">それにはどのくらいの時間がかかるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="17aa6-105">How long would this take?</span></span> <span data-ttu-id="17aa6-106">各システムがばらつきなく適切に構成されたことを保証できますか。</span><span class="sxs-lookup"><span data-stu-id="17aa6-106">Could you ensure that each system was configured properly, with no variance between them?</span></span> <span data-ttu-id="17aa6-107">アーキテクチャの設計でオートメーションを使用すると、これらの課題に対応できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-107">By using automation in your architecture design, you can work past these challenges.</span></span> <span data-ttu-id="17aa6-108">Azure での自動化に利用できるいくつかの方法を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="17aa6-108">Let's take a look at some of the ways you can automate on Azure.</span></span>

## <a name="infrastructure-as-code"></a><span data-ttu-id="17aa6-109">コードとしてのインフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="17aa6-109">Infrastructure as code</span></span>

<span data-ttu-id="17aa6-110">サービスとインフラストラクチャの展開を自動化するには、命令型と宣言型の 2 種類の方法を利用できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-110">When automating the deployment of services and infrastructure, there are two different approaches you can take: imperative and declarative.</span></span> <span data-ttu-id="17aa6-111">命令型の方法では、想定する結果を生成するために実行されるコマンドを明示的に示します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-111">In an imperative approach, you explicitly state the commands that are executed to produce the outcome you are looking for.</span></span> <span data-ttu-id="17aa6-112">宣言型の方法では、実行方法を指定する代わりに、望ましい結果を指定します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-112">With a declarative approach, you specify what you want the outcome to be instead of specifying how you want it done.</span></span> <span data-ttu-id="17aa6-113">どちらの方法にも価値があるので、どちらを選んでも誤ることはありません。</span><span class="sxs-lookup"><span data-stu-id="17aa6-113">Both approaches are valuable, so there's no wrong choice.</span></span> <span data-ttu-id="17aa6-114">これらの異なる方法は Azure ではどのようになっていて、どのように使用するのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="17aa6-114">What do these different approaches look like on Azure, and how do you use them?</span></span>

### <a name="imperative-automation"></a><span data-ttu-id="17aa6-115">命令型のオートメーション</span><span class="sxs-lookup"><span data-stu-id="17aa6-115">Imperative automation</span></span>

<span data-ttu-id="17aa6-116">命令型のオートメーションから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="17aa6-116">Let's start with imperative automation.</span></span> <span data-ttu-id="17aa6-117">命令型のオートメーションでは、何かを行う "_方法_" を指定します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-117">With imperative automation, we're specifying _how_ things are to be done.</span></span> <span data-ttu-id="17aa6-118">通常これは、スクリプト言語または SDK を使用してプログラム的に行われます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-118">This is typically done programmatically through a scripting language or SDK.</span></span> <span data-ttu-id="17aa6-119">Azure リソースの場合は、Azure CLI または Azure PowerShell を使用できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-119">For Azure resources, we could use the Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="17aa6-120">Azure CLI を使用してストレージ アカウントを作成する例を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="17aa6-120">Let's take a look at an example that uses the Azure CLI to create a storage account.</span></span>

```azure-cli
az group create --name storage-resource-group --location westus
az storage account create --resource-group storage-resource-group --name mystorageaccount --kind BlobStorage --access-tier hot
```

<span data-ttu-id="17aa6-121">この例では、これらのリソースを作成する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-121">In this example, we're specifying how to create these resources.</span></span> <span data-ttu-id="17aa6-122">コマンドを実行してリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-122">Execute a command to create a resource group.</span></span> <span data-ttu-id="17aa6-123">別のコマンドを実行してストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-123">Execute another command to create a storage account.</span></span> <span data-ttu-id="17aa6-124">必要な結果を生成するために実行するコマンドを、Azure に明示的に指示します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-124">We're explicitly telling Azure what commands to run to produce the output we need.</span></span>

<span data-ttu-id="17aa6-125">この方法では、インフラストラクチャを完全に自動化できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-125">With this approach, we're able to fully automate our infrastructure.</span></span> <span data-ttu-id="17aa6-126">入力と出力のための領域を提供でき、毎回同じコマンドが実行されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-126">We can provide areas for input and output, and can ensure that the same commands are executed every time.</span></span> <span data-ttu-id="17aa6-127">リソースを自動化することにより、プロセスから手動の手順を取り除き、リソース管理の運用をいっそう効率的にします。</span><span class="sxs-lookup"><span data-stu-id="17aa6-127">By automating our resources, we've taken the manual steps out of the process, making resource administration operationally more efficient.</span></span> <span data-ttu-id="17aa6-128">ただし、この方法にはいくつかの欠点があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-128">There are some downsides to this approach though.</span></span> <span data-ttu-id="17aa6-129">アーキテクチャが複雑になると、リソースを作成するスクリプトもたちまち複雑になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-129">Scripts to create resources can quickly become complex as the architecture becomes more complex.</span></span> <span data-ttu-id="17aa6-130">完全な実行のためには、エラー処理と入力の検証を追加することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-130">Error handling and input validation may need to be added to ensure full execution.</span></span> <span data-ttu-id="17aa6-131">コマンドが変更される可能性があり、スクリプトを継続的にメンテナンスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-131">Commands may change, requiring ongoing maintenance of the scripts.</span></span>

### <a name="declarative-automation"></a><span data-ttu-id="17aa6-132">宣言型のオートメーション</span><span class="sxs-lookup"><span data-stu-id="17aa6-132">Declarative automation</span></span>

<span data-ttu-id="17aa6-133">宣言型のオートメーションでは、望ましい結果が "_何_" かを指定し、それを行う方法の詳細は使用しているシステムに委ねます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-133">With declarative automation, we're specifying _what_ we want our result to be, leaving the details of how it's done to the system we're using.</span></span> <span data-ttu-id="17aa6-134">Azure では、宣言型のオートメーションは Azure Resource Manager テンプレートを使用して行われます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-134">On Azure, declarative automation is done through the use of Azure Resource Manager templates.</span></span>

<span data-ttu-id="17aa6-135">Resource Manager テンプレートは、作成したいものを指定する JSON 構造のファイルです。</span><span class="sxs-lookup"><span data-stu-id="17aa6-135">Resource Manager templates are JSON-structured files that specify what we want created.</span></span> <span data-ttu-id="17aa6-136">次の例では、指定した名前とプロパティを使用してストレージ アカウントを作成するように Azure に指示しています。</span><span class="sxs-lookup"><span data-stu-id="17aa6-136">In the example below, we're telling Azure to create a storage account with the names and properties that we specify.</span></span> <span data-ttu-id="17aa6-137">このストレージ アカウントを作成するために実行される実際の手順は、Azure に任されています。</span><span class="sxs-lookup"><span data-stu-id="17aa6-137">The actual steps that are executed to create this storage account are left to Azure.</span></span> <span data-ttu-id="17aa6-138">テンプレートには、パラメーター、変数、リソース、出力の 4 つのセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-138">Templates have four sections: parameters, variables, resources, and outputs.</span></span> <span data-ttu-id="17aa6-139">パラメーターでは、テンプレート内で使用される入力を処理します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-139">Parameters handle input to be used within the template.</span></span> <span data-ttu-id="17aa6-140">変数では、テンプレート全体で使用する値を格納する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="17aa6-140">Variables provide a way to store values for use throughout the template.</span></span> <span data-ttu-id="17aa6-141">リソースは作成されるものであり、出力は作成されたものの詳細をユーザーに提供する手段です。</span><span class="sxs-lookup"><span data-stu-id="17aa6-141">Resources are the things that are being created, and outputs are a way to provide details to the user of what was created.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "accountType": {
            "type": "string",
            "defaultValue": "Standard_RAGRS"
        },
        "kind": {
            "type": "string"
        },
        "accessTier": {
            "type": "string"
        },
        "httpsTrafficOnlyEnabled": {
            "type": "bool",
            "defaultValue": true
        }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('name')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "[parameters('accountType')]"
            },
            "kind": "[parameters('kind')]",
            "properties": {
                "supportsHttpsTrafficOnly": "[parameters('httpsTrafficOnlyEnabled')]",
                "accessTier": "[parameters('accessTier')]",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "storageAccountName": {
            "type": "string",
            "value": "[parameters('name')]"
        }
    }
}
```

<span data-ttu-id="17aa6-142">テンプレートを使用して、Azure のすべてのサービスを作成および操作できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-142">Templates can be used to create and manipulate every service on Azure.</span></span> <span data-ttu-id="17aa6-143">テンプレートは、コード リポジトリに格納してソース管理でき、異なる環境間で共有して、開発対象のインフラストラクチャが運用環境内に実際に存在するものと一致することを保証できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-143">They can be stored in code repositories and source controlled, and shared across environments to ensure that the infrastructure being developed against matches what's actually in production.</span></span> <span data-ttu-id="17aa6-144">展開を自動化して一貫性を保証する優れた方法であり、展開が誤って構成されることがなくなり、運用の速度を上げることができます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-144">They are a great way to automate deployments and help ensure consistency, eliminate deployment misconfigurations, and can increase operational speed.</span></span>

<span data-ttu-id="17aa6-145">インフラストラクチャの展開の自動化は大きな第一歩ですが、仮想マシンを展開するときは、まだ他にもやることがあります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-145">Automating your infrastructure deployment is a great first step, but when deploying virtual machines, there's still more work to do.</span></span> <span data-ttu-id="17aa6-146">展開後の構成を自動化するいくつかの方法を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="17aa6-146">Let's take a look at a couple of approaches to automating configuration post deployment.</span></span>

## <a name="vm-customization-images-vs-post-deployment-configuration"></a><span data-ttu-id="17aa6-147">VM のカスタマイズ: イメージと展開後の構成</span><span class="sxs-lookup"><span data-stu-id="17aa6-147">VM customization: images vs. post-deployment configuration</span></span>

<span data-ttu-id="17aa6-148">仮想マシンの展開の多くでは、ジョブはマシンの実行時に行われません。</span><span class="sxs-lookup"><span data-stu-id="17aa6-148">For many virtual machine deployments, the job isn't done when the machine is running.</span></span> <span data-ttu-id="17aa6-149">VM が意図された目的を実際に提供できるようにするには、多くの場合、事前の追加構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="17aa6-149">It's likely there's additional configuration that's needed before the VM can actually serve its intended purpose.</span></span> <span data-ttu-id="17aa6-150">追加ディスクのフォーマット、ドメインへの VM の参加、管理ソフトウェア用のエージェントのインストールなどが必要になる場合があり、最も可能性が高いのは、実際のワークロードもインストールして構成することです。</span><span class="sxs-lookup"><span data-stu-id="17aa6-150">Additional disks might need formatting, the VM might need to be joined to a domain, maybe an agent for a management software needs to be installed, and most likely the actual workload requires installation and configuration as well.</span></span>

<span data-ttu-id="17aa6-151">VM 自体の構成の一部と見なされる構成作業に対して適用される一般的な戦略が 2 つあり、どちらにも長所と短所があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-151">There are two common strategies applied for the configuration work considered to be part the configuration of the VM itself, both of which have advantages and disadvantages:</span></span>

* <span data-ttu-id="17aa6-152">カスタム イメージ</span><span class="sxs-lookup"><span data-stu-id="17aa6-152">Custom images</span></span>
* <span data-ttu-id="17aa6-153">展開後のスクリプト実行</span><span class="sxs-lookup"><span data-stu-id="17aa6-153">Post-deployment scripting</span></span>

<span data-ttu-id="17aa6-154">カスタム イメージを生成するには、仮想マシンを展開してから、その実行しているインスタンス上でソフトウェアを構成またはインストールします。</span><span class="sxs-lookup"><span data-stu-id="17aa6-154">Custom images are generated by deploying a virtual machine and then configuring or installing software on that running instance.</span></span> <span data-ttu-id="17aa6-155">すべてを正しく構成したら、マシンをシャットダウンすることができ、VM からイメージが作成されます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-155">When everything is configured correctly, the machine can be shut down, and an image is created from the VM.</span></span> <span data-ttu-id="17aa6-156">その後、他の新しい仮想マシンのベースとしてイメージを使用できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-156">The image can then be used as a base for other new virtual machines.</span></span> <span data-ttu-id="17aa6-157">カスタム イメージを使用すると、仮想マシンを展開して実行した後は追加構成の必要がないため、展開全体の時間を短縮できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-157">Working with custom images can speed up the overall time of your deployment as once the virtual machine is deployed and running, no additional configuration would be needed.</span></span> <span data-ttu-id="17aa6-158">展開の速度が重要な要因である場合は、間違いなくカスタム イメージを検討する価値があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-158">If deployment speed is an important factor, custom images are definitely worth exploring.</span></span>

<span data-ttu-id="17aa6-159">展開後のスクリプト実行では、通常、基本的なベース イメージを利用した後、スクリプトや構成管理プラットフォームを使用して、VM が展開された後の構成を行います。</span><span class="sxs-lookup"><span data-stu-id="17aa6-159">Post-deployment scripting typically leverages a basic base image, then relies on scripting or a configuration management platform to do configuration after the VM is deployed.</span></span> <span data-ttu-id="17aa6-160">展開後のスクリプト実行は、Azure スクリプト拡張機能を使用して VM でスクリプトを実行することで、または Azure Automation Desired State Configuration (DSC) などのより堅牢なソリューションを利用して、行うことができます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-160">The post-deployment scripting could be done by executing a script on the VM through the Azure Script Extension or by leveraging a more robust solution such as Azure Automation Desired State Configuration (DSC).</span></span>

<span data-ttu-id="17aa6-161">いずれの方法でも、いくつかの考慮事項に留意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-161">Each approach has some considerations to keep in mind.</span></span> <span data-ttu-id="17aa6-162">イメージを使用する場合は、イメージの更新プログラム、セキュリティ修正プログラム、およびイメージ自体のインベントリ管理を処理するためにプロセスが存在することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-162">When using images, you'll need to ensure there's a process to handle image updates, security patches, and inventory management of the images themselves.</span></span> <span data-ttu-id="17aa6-163">展開後スクリプト実行では、ビルドが完了するまで VM をライブ ワークロードに追加できないため、ビルド時間が長くなることがあります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-163">With post-deployment scripting, build times can be extended since the VM can't be added to live workloads until the build is complete.</span></span> <span data-ttu-id="17aa6-164">これはスタンドアロン システムについては大きな問題ではないかもしれませんが、自動スケーリングを行うサービスを使用しているときは (仮想マシン スケール セットなど)、ビルドの時間が長くなると、スケーリングできる速さに影響する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-164">This may not be a significant issue for standalone systems, but when using services that autoscale (such as virtual machine scale sets), this extended build time can impact how quickly you can scale.</span></span> <span data-ttu-id="17aa6-165">どちらの方法でも、構成のずれに対処する必要あります。新しい構成をロールアウトするときは、既存のシステムが適宜更新されるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-165">With both approaches, you'll want to ensure you address configuration drift; as new configuration is rolled out, you'll need to ensure that existing systems are updated accordingly.</span></span>

<span data-ttu-id="17aa6-166">リソースの展開を自動化することは、環境に対して大きなメリットになる場合があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-166">Automating resource deployment can be a massive benefit to your environment.</span></span> <span data-ttu-id="17aa6-167">時間の短縮とエラーの削減により、運用機能が新しいレベルに移る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-167">The amount of time saved and error reduced can move your operational capabilities to another level.</span></span>

## <a name="automation-of-operational-tasks"></a><span data-ttu-id="17aa6-168">運用タスクのオートメーション</span><span class="sxs-lookup"><span data-stu-id="17aa6-168">Automation of operational tasks</span></span>

<span data-ttu-id="17aa6-169">ソリューションが稼働状態になった後は、継続的な運用アクティビティの中にも自動化できるものがあります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-169">Once your solutions are up and running, there are ongoing operational activities that can also be automated.</span></span> <span data-ttu-id="17aa6-170">Azure Automation でこれらのタスクを自動化すると、手動のワークロードが減り、コンピューティング リソースの構成と更新を管理でき、スケジュール、資格情報、証明書などの共有リソースが集中化され、任意の種類の Azure タスクを実行するためのフレームワークが提供されます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-170">Automating these tasks with Azure Automation reduces manual workloads, enables configuration and update management of compute resources, centralizes shared resources such as schedules, credentials, and certificates, and provides a framework for running any type of Azure task.</span></span>

<span data-ttu-id="17aa6-171">Lamna Healthcare の作業の場合、次のようなものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-171">For your Lamna Healthcare work, this might include:</span></span>
- <span data-ttu-id="17aa6-172">孤立したディスクの定期的な検索</span><span class="sxs-lookup"><span data-stu-id="17aa6-172">Periodically searching for orphaned disks.</span></span>
- <span data-ttu-id="17aa6-173">VM への最新のセキュリティ修正プログラムのインストール</span><span class="sxs-lookup"><span data-stu-id="17aa6-173">Installing the latest security patches on VMs.</span></span>
- <span data-ttu-id="17aa6-174">業務時間外の仮想マシンの検索とシャットダウン</span><span class="sxs-lookup"><span data-stu-id="17aa6-174">Searching for and shutting down virtual machines in off-hours.</span></span>
- <span data-ttu-id="17aa6-175">上級管理者に報告するための日次レポートの実行とダッシュボードの生成</span><span class="sxs-lookup"><span data-stu-id="17aa6-175">Running daily reports and producing a dashboard to report to senior management.</span></span>

![VM の電源状態の自動化](../media-draft/automation-vm-power-state.png)

## <a name="automating-development-environments"></a><span data-ttu-id="17aa6-177">開発環境の自動化</span><span class="sxs-lookup"><span data-stu-id="17aa6-177">Automating development environments</span></span>

<span data-ttu-id="17aa6-178">クラウド インフラストラクチャのパイプラインのもう一方の端は、ビジネスの中核となるアプリケーションとサービスを作成するために開発者が使用する開発用マシンです。</span><span class="sxs-lookup"><span data-stu-id="17aa6-178">At the other end of the pipeline of your cloud infrastructure are the development machines used by developers to write the applications and services that are the core of your business.</span></span> <span data-ttu-id="17aa6-179">Azure DevTest Labs を使用すると、必要とするすべての適切なツールとリポジトリを VM に設定できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-179">You can use Azure DevTest Labs to stamp out VMs with all of the correct tools and repositories that they need.</span></span> <span data-ttu-id="17aa6-180">複数のサービスで作業する開発者は、新しいマシンを自分でプロビジョニングする必要なしに、開発環境を切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-180">Developers working on multiple services can switch between development environments without having to provision a new machine themselves.</span></span> <span data-ttu-id="17aa6-181">これらの開発環境は、使っていないときはシャットダウンし、再び必要になったら再起動できます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-181">These development environments can be shut down when not in use and restarted when they are required again.</span></span>

## <a name="automation-at-lamna-healthcare"></a><span data-ttu-id="17aa6-182">Lamna Healthcare でのオートメーション</span><span class="sxs-lookup"><span data-stu-id="17aa6-182">Automation at Lamna Healthcare</span></span>

<span data-ttu-id="17aa6-183">オートメーションを使用することで Lamna Healthcare がどのように改善されるかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="17aa6-183">Let's take a look at how Lamna Healthcare has improved by using automation.</span></span> <span data-ttu-id="17aa6-184">オートメーションの導入を始める前は、インフラストラクチャの展開とサーバーの構築は完全に手動でした。</span><span class="sxs-lookup"><span data-stu-id="17aa6-184">When you started your journey, infrastructure deployment and server builds were entirely manual.</span></span> <span data-ttu-id="17aa6-185">エンジニアは、ポータルを通してすべてのものを展開していました。</span><span class="sxs-lookup"><span data-stu-id="17aa6-185">Engineers were deploying everything through the portal.</span></span> <span data-ttu-id="17aa6-186">そのため、テスト環境と運用環境の間で不一致とエラーが発生し、相違のため、コードを運用状態にする前に問題を検出する機能が妨げられていました。</span><span class="sxs-lookup"><span data-stu-id="17aa6-186">This was introducing variance and errors between test and production environments, and the differences were hindering their ability to detect problems before code hit production.</span></span>

<span data-ttu-id="17aa6-187">現在は、Resource Manager テンプレートを使用してすべてのインフラストラクチャを展開するようになりました。</span><span class="sxs-lookup"><span data-stu-id="17aa6-187">They now deploy all their infrastructure through Resource Manager templates.</span></span> <span data-ttu-id="17aa6-188">これらのテンプレートは GitHub リポジトリにチェックインされ、展開にリリースされる前にコード レビューが行われます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-188">These templates are checked into a GitHub repository, and a code review happens before they are released for deployment.</span></span> <span data-ttu-id="17aa6-189">開発環境、テスト環境、運用環境の間に同じインフラストラクチャを構築し、すべての環境で構成が検証されるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-189">They're also able to build the same infrastructure between dev, test, and production, ensuring they have validated their configuration across all environments.</span></span>

<span data-ttu-id="17aa6-190">仮想マシンを使用するほとんどのサービスについて、標準のベース イメージを用意し、DSC を使用して展開後のシステムを構成しています。</span><span class="sxs-lookup"><span data-stu-id="17aa6-190">For most services using virtual machines, they have a standard base image and use DSC to configure the systems post deployment.</span></span> <span data-ttu-id="17aa6-191">仮想マシン スケール セットのスケーラビリティを必要とする Web ファームでは、スケール セットで使用できるようにする前に、完全に自動化されたプロセスを使用してコードをチェックインし、必要な構成がすべて組み込まれた新しいイメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="17aa6-191">For web farms where they need the scalability of virtual machine scale sets, they have a fully automated process to check in code and build a new image with all required configuration built in before making it available in their scale sets.</span></span>

<span data-ttu-id="17aa6-192">コストを削減するために勤務時間外に識別された仮想マシンをシャットダウンする Automation ジョブがあり、VM の修正プログラムの適用も自動化されています。</span><span class="sxs-lookup"><span data-stu-id="17aa6-192">They have an Automation job to shut down identified virtual machines in off-hours to reduce costs and have automated their VM patching as well.</span></span>

<span data-ttu-id="17aa6-193">開発者には DevTest Labs にセルフ サービスの環境があり、そこでは最新のイメージと構成に対して開発を行うことができ、開発対象と運用環境の構成が一致することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-193">Developers now have a self-service environment in DevTest Labs where they can develop against the latest images and configuration, ensuring that what they develop against matches the configuration in production.</span></span>

<span data-ttu-id="17aa6-194">このすべてには若干の事前作業がありますが、長い目で見ればメリットの方が上回ります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-194">All of this took some up-front effort, but the benefits have paid off in the long run.</span></span> <span data-ttu-id="17aa6-195">エラーと、環境を維持するために運用チームで必要な労力が、大幅に減少しています。</span><span class="sxs-lookup"><span data-stu-id="17aa6-195">They've dramatically reduced error and the effort required by their operations teams to maintain their environments.</span></span> <span data-ttu-id="17aa6-196">開発者は、開発対象のリソースを簡単にプロビジョニングでき、行ったり来たりして環境を作成する必要がないことを、喜んでいます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-196">Developers love that they can easily go provision resources to develop against, eliminating the back and forth to get environments created.</span></span>

## <a name="summary"></a><span data-ttu-id="17aa6-197">まとめ</span><span class="sxs-lookup"><span data-stu-id="17aa6-197">Summary</span></span>

<span data-ttu-id="17aa6-198">アーキテクチャにオートメーション機能を組み込むいくつかの方法を見てきました。</span><span class="sxs-lookup"><span data-stu-id="17aa6-198">We've taken a look at a number of ways to bring automation capabilities into your architecture.</span></span> <span data-ttu-id="17aa6-199">コードとしてのインフラストラクチャの展開から、ラボ環境での開発者の生産性向上まで、環境の自動化に時間をかけると多くのメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="17aa6-199">From deploying infrastructure as code, to improving developer productivity with lab environments, there's a ton of benefit from taking time to automate your environment.</span></span> <span data-ttu-id="17aa6-200">エラーの削減、差異の減少、運用コストの節約は、組織にとって大きなメリットであり、クラウド環境をレベルアップするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="17aa6-200">Reducing error, reducing variance, and saving operational costs can be a significant benefit to your organization and help take your cloud environment to the next level.</span></span>