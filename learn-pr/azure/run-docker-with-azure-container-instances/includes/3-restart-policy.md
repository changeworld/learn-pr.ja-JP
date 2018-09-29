<span data-ttu-id="1e626-101">Azure Container Instances では、コンテナーのデプロイを簡単かつ迅速に行えるため、ビルド、テスト、イメージ レンダリングなどの一度のみ実行されるタスクの実行に最適です。</span><span class="sxs-lookup"><span data-stu-id="1e626-101">The ease and speed of deploying containers in Azure Container Instances makes it a great fit for executing run-once tasks like build, test, and image rendering.</span></span>

<span data-ttu-id="1e626-102">構成可能な再起動ポリシーを使用して、プロセスが完了したらコンテナーが停止するように指定できます。</span><span class="sxs-lookup"><span data-stu-id="1e626-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="1e626-103">コンテナーのインスタンスは秒単位で課金されるため、タスクを実行するコンテナーの実行中に使用されるコンピューティング リソースのみが課金されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="1e626-104">コンテナーの再起動ポリシー</span><span class="sxs-lookup"><span data-stu-id="1e626-104">Container restart policies</span></span>

<span data-ttu-id="1e626-105">Azure Container Instances には、次の 3 つの再起動ポリシー オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="1e626-105">Azure Container Instances has three restart-policy options:</span></span>

| <span data-ttu-id="1e626-106">再起動ポリシー</span><span class="sxs-lookup"><span data-stu-id="1e626-106">Restart policy</span></span>   | <span data-ttu-id="1e626-107">説明</span><span class="sxs-lookup"><span data-stu-id="1e626-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="1e626-108">コンテナー グループ内のコンテナーを常に再起動する。</span><span class="sxs-lookup"><span data-stu-id="1e626-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="1e626-109">このポリシーは、Web サーバーなどの実行時間の長いタスクに適しています。</span><span class="sxs-lookup"><span data-stu-id="1e626-109">This policy makes sense for long-running tasks such as a web server.</span></span> <span data-ttu-id="1e626-110">これは**既定**の設定で、コンテナー作成時に再起動ポリシーが指定されていない場合に適用されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-110">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="1e626-111">コンテナー グループ内のコンテナーを再起動しない。</span><span class="sxs-lookup"><span data-stu-id="1e626-111">Containers in the container group are never restarted.</span></span> <span data-ttu-id="1e626-112">コンテナーは最大で 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-112">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="1e626-113">コンテナーで実行されたプロセスが失敗 (0 以外の終了コードで終了) した場合にのみ、コンテナー グループ内のコンテナーを再起動する。</span><span class="sxs-lookup"><span data-stu-id="1e626-113">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="1e626-114">コンテナーは少なくとも 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-114">The containers are run at least once.</span></span> <span data-ttu-id="1e626-115">このポリシーは、存続期間の短いタスクを実行するコンテナーに適しています。</span><span class="sxs-lookup"><span data-stu-id="1e626-115">This policy works well for containers that run short-lived tasks.</span></span> |

## <a name="run-to-completion"></a><span data-ttu-id="1e626-116">完了まで実行</span><span class="sxs-lookup"><span data-stu-id="1e626-116">Run to completion</span></span>

<span data-ttu-id="1e626-117">再起動ポリシーが動作しているのを確認するには、*microsoft/aci-wordcount* イメージからコンテナー インスタンスを作成し、*OnFailure* 再起動ポリシーを指定します。</span><span class="sxs-lookup"><span data-stu-id="1e626-117">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="1e626-118">このコンテナー例では、シェイクスピアのハムレットのテキストを解析し、最もよく使われる 10 個の単語を STDOUT に書き込んで終了する Python スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="1e626-118">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="1e626-119">次の `az container create` コマンドでコンテナー例を実行します。</span><span class="sxs-lookup"><span data-stu-id="1e626-119">Run the example container with the following `az container create` command:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --location eastus
```

<span data-ttu-id="1e626-120">Azure Container Instances によってコンテナーが開始され、そのアプリケーション (ここではスクリプト) の終了時に停止されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-120">Azure Container Instances starts the container then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="1e626-121">Azure Container Instances によって、再起動ポリシーが *Never* または *OnFailure* のコンテナーが停止されると、そのコンテナーの状態は **Terminated** に設定されます。</span><span class="sxs-lookup"><span data-stu-id="1e626-121">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="1e626-122">`az container show` コマンドで、コンテナーの状態を確認できます。</span><span class="sxs-lookup"><span data-stu-id="1e626-122">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="1e626-123">コンテナー例の状態が **Terminated** と表示されたら、コンテナー ログを表示してタスクの出力を確認できます。</span><span class="sxs-lookup"><span data-stu-id="1e626-123">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="1e626-124">`az container logs` コマンドを実行して、スクリプトの出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="1e626-124">Run the `az container logs` command to view the script's output:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo
```

<span data-ttu-id="1e626-125">出力: </span><span class="sxs-lookup"><span data-stu-id="1e626-125">Output:</span></span>

```json
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```