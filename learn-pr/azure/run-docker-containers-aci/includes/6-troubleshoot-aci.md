<span data-ttu-id="63228-101">このユニットでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかの基本的なトラブルシューティング操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="63228-101">In this unit you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="63228-102">このモジュールの最後では、コンテナー インスタンスをトラブルシューティングする基本的な機能を理解できるようになります。</span><span class="sxs-lookup"><span data-stu-id="63228-102">By the end of this module you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="63228-103">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="63228-103">Create a container</span></span>

<span data-ttu-id="63228-104">まず、このユニットで使用するコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="63228-104">Start by creating a container to use in this unit.</span></span> <span data-ttu-id="63228-105">このモジュールで作成した最初のコンテナーがまだある場合は、この手順はスキップします。</span><span class="sxs-lookup"><span data-stu-id="63228-105">If you still have the first container created in this module, skip this step.</span></span>

```azurecli
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="63228-106">コンテナー インスタンスからログを取得する</span><span class="sxs-lookup"><span data-stu-id="63228-106">Get logs from a container instance</span></span>

<span data-ttu-id="63228-107">アプリケーション コードからコンテナー内のログを表示するには、`az container logs` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="63228-107">To view logs from your application code within a container, you can use the `az container logs` command.</span></span>

```azazurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="63228-108">次は、Web アプリが数回アクセスされた後の例のコンテナーからのログ出力です。</span><span class="sxs-lookup"><span data-stu-id="63228-108">The following is log output from the example container after the web app has been accessed a few times.</span></span>

```bash
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## <a name="get-container-events"></a><span data-ttu-id="63228-109">コンテナー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="63228-109">Get container events</span></span>

<span data-ttu-id="63228-110">`az container attach` コマンドは、コンテナーの起動中の診断情報を出力します。</span><span class="sxs-lookup"><span data-stu-id="63228-110">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="63228-111">コンテナーが起動すると、ローカルのコンソール STDOUT と STDERR もストリーミングされます。</span><span class="sxs-lookup"><span data-stu-id="63228-111">Once the container has started, it also streams STDOUT and STDERR to your local console.</span></span>

```azazurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

<span data-ttu-id="63228-112">出力例</span><span class="sxs-lookup"><span data-stu-id="63228-112">Example output.</span></span>


```bash
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="63228-113">コンテナーでコマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="63228-113">Execute a command in a container</span></span>

<span data-ttu-id="63228-114">Azure Container Instances は、実行中のコンテナーでのコマンドの実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="63228-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="63228-115">既に開始されているコンテナー内でのコマンドの実行は、アプリケーションの開発とトラブルシューティング時に特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="63228-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="63228-116">この機能の最も一般的な用途は、対話型シェルを起動して、実行中のコンテナーで発生した問題をデバッグできるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="63228-116">The most common use of this feature is to launch an interactive shell so that you can debug issues in a running container.</span></span>

<span data-ttu-id="63228-117">この例では、実行中のコンテナーで、対話型のターミナル セッションを開始します。</span><span class="sxs-lookup"><span data-stu-id="63228-117">This example starts an interactive terminal session with the running container.</span></span>

```azurecli
az container exec --resource-group myResourceGroup --name mycontainer --exec-command /bin/sh
```

<span data-ttu-id="63228-118">コマンドが完了すると、実質的にコンテナー内で作業していることになります。</span><span class="sxs-lookup"><span data-stu-id="63228-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="63228-119">この例では、作業ディレクトリの内容を表示する `ls` コマンドを実行しました。</span><span class="sxs-lookup"><span data-stu-id="63228-119">In this example, the `ls` command was run to display the contents of the working directory.</span></span>

```bash
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

<span data-ttu-id="63228-120">`exit` と入力し、リモート セッションを停止します。</span><span class="sxs-lookup"><span data-stu-id="63228-120">Type `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="63228-121">コンテナーの CPU とメモリを監視する</span><span class="sxs-lookup"><span data-stu-id="63228-121">Monitor container CPU and Memory</span></span>

<span data-ttu-id="63228-122">CPU とメモリの使用量のメトリックをプルしたい場合があります。</span><span class="sxs-lookup"><span data-stu-id="63228-122">You may want to pull metrics on CPU and Memory usage.</span></span> <span data-ttu-id="63228-123">これを行うには、Azure Container Instances の ID をまず取得します。</span><span class="sxs-lookup"><span data-stu-id="63228-123">To do so, first get the ID of the Azure Container Instance.</span></span> <span data-ttu-id="63228-124">この例では、ID は `CONTAINER_ID` という名前の変数に配置されています。</span><span class="sxs-lookup"><span data-stu-id="63228-124">In this example, the ID is placed in a variable named `CONTAINER_ID`.</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group myResourceGroup --name mycontainer --query id --output tsv)
```

<span data-ttu-id="63228-125">ここで、`az monitor metrics list` コマンドを使用して、CPU の使用量の情報を戻します。</span><span class="sxs-lookup"><span data-stu-id="63228-125">Now, use the `az monitor metrics list` command to pull back CPU usage information.</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

<span data-ttu-id="63228-126">出力例</span><span class="sxs-lookup"><span data-stu-id="63228-126">Example output.</span></span>

```bash
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

<span data-ttu-id="63228-127">メモリの使用率の情報を取得するには、次のコマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="63228-127">The following command can be used to get memory usage information.</span></span>

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

<span data-ttu-id="63228-128">出力例</span><span class="sxs-lookup"><span data-stu-id="63228-128">Example output.</span></span>

```bash
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

<span data-ttu-id="63228-129">この情報は、Azure portal でも見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="63228-129">This information is also avlaible in the Azure portal.</span></span> <span data-ttu-id="63228-130">CPU とメモリ使用量の情報を図で確認するには、コンテナー インスタンスの Azure portal の概要ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="63228-130">To see graphical representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Azure Container Instances の CPU とメモリ使用量の情報の Azure portal でのビュー。](../media-draft/cpu-memory.png)

## <a name="clean-up"></a><span data-ttu-id="63228-132">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="63228-132">Clean up</span></span>

<span data-ttu-id="63228-133">これは、Azure Container Instances (ACI) のラーニング モジュールの最後のユニットです。</span><span class="sxs-lookup"><span data-stu-id="63228-133">This is the last unit of the Azure Container Instances (ACI) learning module.</span></span> <span data-ttu-id="63228-134">この時点で、リソース グループを削除して、作成したリソースをクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="63228-134">At this point, you can clean up the created resources by deleting the resource group.</span></span> <span data-ttu-id="63228-135">これを行うには az group delete コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="63228-135">To do so, use the az group delete command.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="63228-136">まとめ</span><span class="sxs-lookup"><span data-stu-id="63228-136">Summary</span></span>

<span data-ttu-id="63228-137">このユニットでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかのトラブルシューティング操作を学習しました。</span><span class="sxs-lookup"><span data-stu-id="63228-137">In this unit, you learned about several troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span>