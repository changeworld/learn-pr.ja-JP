<span data-ttu-id="04eaa-101">Azure Container Instances ではコンテナー デプロイを簡単にすばやく行えるため、コンテナー インスタンスでのビルド、テスト、イメージ レンダリングなどの一度のみ実行されるタスクの実行に優れたプラットフォームを提供します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-101">The ease and speed of deploying containers in Azure Container Instances provides a compelling platform for executing run-once tasks like build, test, and image rendering in a container instance.</span></span>

<span data-ttu-id="04eaa-102">構成可能な再起動ポリシーを使用して、プロセスが完了したらコンテナーが停止するように指定できます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-102">With a configurable restart policy, you can specify that your containers are stopped when their processes have completed.</span></span> <span data-ttu-id="04eaa-103">コンテナーのインスタンスは秒単位で課金されるため、タスクを実行するコンテナーの実行中に使用されるコンピューティング リソースのみが課金されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-103">Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.</span></span>

## <a name="container-restart-policies"></a><span data-ttu-id="04eaa-104">コンテナー再起動ポリシー</span><span class="sxs-lookup"><span data-stu-id="04eaa-104">Container restart policies</span></span>

<span data-ttu-id="04eaa-105">Azure Container Instances でコンテナーを作成する場合、3 つの再起動ポリシー設定のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-105">When you create a container in Azure Container Instances, you can specify one of three restart policy settings:</span></span>

| <span data-ttu-id="04eaa-106">再起動ポリシー</span><span class="sxs-lookup"><span data-stu-id="04eaa-106">Restart policy</span></span>   | <span data-ttu-id="04eaa-107">説明</span><span class="sxs-lookup"><span data-stu-id="04eaa-107">Description</span></span> |
| ---------------- | :---------- |
| `Always` | <span data-ttu-id="04eaa-108">コンテナー グループ内のコンテナーを常に再起動する。</span><span class="sxs-lookup"><span data-stu-id="04eaa-108">Containers in the container group are always restarted.</span></span> <span data-ttu-id="04eaa-109">これは**既定**の設定で、コンテナー作成時に再起動ポリシーが指定されていない場合に適用されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-109">This is the **default** setting applied when no restart policy is specified at container creation.</span></span> |
| `Never` | <span data-ttu-id="04eaa-110">コンテナー グループ内のコンテナーを再起動しない。</span><span class="sxs-lookup"><span data-stu-id="04eaa-110">Containers in the container group are never restarted.</span></span> <span data-ttu-id="04eaa-111">コンテナーは最大で 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-111">The containers run at most once.</span></span> |
| `OnFailure` | <span data-ttu-id="04eaa-112">コンテナーで実行されたプロセスが失敗 (0 以外の終了コードで終了) した場合にのみ、コンテナー グループ内のコンテナーを再起動する。</span><span class="sxs-lookup"><span data-stu-id="04eaa-112">Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code).</span></span> <span data-ttu-id="04eaa-113">コンテナーは少なくとも 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-113">The containers are run at least once.</span></span> |

<span data-ttu-id="04eaa-114">このモジュールの前のユニットでは、再起動ポリシーを指定せずに、コンテナーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="04eaa-114">In the previous unit of this module, a container was created without a specified restart policy.</span></span> <span data-ttu-id="04eaa-115">既定では、このコンテナーで *Always* 再起動ポリシーを受け取りました。</span><span class="sxs-lookup"><span data-stu-id="04eaa-115">By default, this container received the *Always* restart policy.</span></span> <span data-ttu-id="04eaa-116">コンテナー内のワークロードは長時間実行されているため (Web サーバー)、このポリシーは理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="04eaa-116">Because the workload in the container is long running (a web server), this policy makes sense.</span></span>

## <a name="run-to-completion"></a><span data-ttu-id="04eaa-117">完了まで実行</span><span class="sxs-lookup"><span data-stu-id="04eaa-117">Run to completion</span></span>

<span data-ttu-id="04eaa-118">再起動ポリシーが動作しているのを確認するには、*microsoft/aci-wordcount* イメージからコンテナー インスタンスを作成し、*OnFailure* 再起動ポリシーを指定します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-118">To see the restart policy in action, create a container instance from the *microsoft/aci-wordcount* image and specify the *OnFailure* restart policy.</span></span> <span data-ttu-id="04eaa-119">このコンテナー例では、シェイクスピアのハムレットのテキストを解析し、最もよく使われる 10 個の単語を STDOUT に書き込んで終了する Python スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-119">This example container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to STDOUT, and then exits.</span></span>

<span data-ttu-id="04eaa-120">次の `az container create` コマンドでコンテナー例を実行します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-120">Run the example container with the following `az container create` command:</span></span>

```azureclu
az container create \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

<span data-ttu-id="04eaa-121">Azure Container Instances によってコンテナーが開始され、そのアプリケーション (ここではスクリプト) の終了時に停止されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-121">Azure Container Instances starts the container and then stops it when its application (or script, in this case) exits.</span></span> <span data-ttu-id="04eaa-122">Azure Container Instances によって、再起動ポリシーが *Never* または *OnFailure* のコンテナーが停止されると、そのコンテナーの状態は **Terminated** に設定されます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-122">When Azure Container Instances stops a container whose restart policy is *Never* or *OnFailure*, the container's status is set to **Terminated**.</span></span>

<span data-ttu-id="04eaa-123">`az container show` コマンドで、コンテナーの状態を確認できます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-123">You can check a container's status with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

<span data-ttu-id="04eaa-124">コンテナー例の状態が **Terminated** と表示されたら、コンテナー ログを表示してタスクの出力を確認できます。</span><span class="sxs-lookup"><span data-stu-id="04eaa-124">Once the example container's status shows **Terminated**, you can see its task output by viewing the container logs.</span></span> <span data-ttu-id="04eaa-125">**az container logs** コマンドを実行して、スクリプトの出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-125">Run the **az container logs** command to view the script's output:</span></span>

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer-restart-demo
```

<span data-ttu-id="04eaa-126">出力:</span><span class="sxs-lookup"><span data-stu-id="04eaa-126">Output:</span></span>

```bash
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

## <a name="summary"></a><span data-ttu-id="04eaa-127">まとめ</span><span class="sxs-lookup"><span data-stu-id="04eaa-127">Summary</span></span>

<span data-ttu-id="04eaa-128">このユニットでは、*OnFailure* の再起動ポリシーを使用して、コンテナー インスタンスを作成しました。</span><span class="sxs-lookup"><span data-stu-id="04eaa-128">In this unit, you created a container instance with a restart policy of *OnFailure*.</span></span> <span data-ttu-id="04eaa-129">この構成は、存続期間の短いタスクを実行するコンテナーに適しています。</span><span class="sxs-lookup"><span data-stu-id="04eaa-129">This configuration works well for containers that run short-lived tasks.</span></span>

<span data-ttu-id="04eaa-130">次のユニットでは、Azure Container Instances で環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="04eaa-130">In the next unit, you will set environment variables in Azure Container Instances.</span></span>
