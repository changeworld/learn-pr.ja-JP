<span data-ttu-id="078fe-101">ここでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかの基本的なトラブルシューティング操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="078fe-101">Here, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.</span></span> <span data-ttu-id="078fe-102">このモジュールの最後には、コンテナー インスタンスをトラブルシューティングするための基本的な機能を理解できるようになります。</span><span class="sxs-lookup"><span data-stu-id="078fe-102">By the end of this module, you should understand basic capabilities for troubleshooting container instances.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="078fe-103">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="078fe-103">Create a container</span></span>

<span data-ttu-id="078fe-104">まず次のコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="078fe-104">Start by creating the following container:</span></span> 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a><span data-ttu-id="078fe-105">コンテナー インスタンスからログを取得する</span><span class="sxs-lookup"><span data-stu-id="078fe-105">Get logs from a container instance</span></span>

<span data-ttu-id="078fe-106">アプリケーション コードからコンテナー内のログを表示するために、`az container logs` コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="078fe-106">To view logs from your application code within a container, you can use the `az container logs` command:</span></span>

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="078fe-107">出力例:</span><span class="sxs-lookup"><span data-stu-id="078fe-107">Example output:</span></span>

```output
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
Running inside /app/prestart.sh, you could add migrations to this file, e.g.:

#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
```

## <a name="get-container-events"></a><span data-ttu-id="078fe-108">コンテナー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="078fe-108">Get container events</span></span>

<span data-ttu-id="078fe-109">`az container attach` コマンドでは、コンテナーの起動中の診断情報が示されます。</span><span class="sxs-lookup"><span data-stu-id="078fe-109">The `az container attach` command provides diagnostic information during container startup.</span></span> <span data-ttu-id="078fe-110">コンテナーが起動すると、ローカル コンソールに STDOUT と STDERR もストリーミングされます。</span><span class="sxs-lookup"><span data-stu-id="078fe-110">Once the container has started, it also streams STDOUT and STDERR to your local console:</span></span>

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

<span data-ttu-id="078fe-111">出力例:</span><span class="sxs-lookup"><span data-stu-id="078fe-111">Example output:</span></span>

```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-09-21 23:48:14+00:00) pulling image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:09+00:00) Successfully pulled image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:12+00:00) Created container
(count: 1) (last timestamp: 2018-09-21 23:49:13+00:00) Started container

Start streaming logs:
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
```

> [!TIP]
> <span data-ttu-id="078fe-112">接続されているコンテナーから切断するには、<kbd>Ctrl キーを押しながら C キー</kbd>を押します。</span><span class="sxs-lookup"><span data-stu-id="078fe-112">To disconnect from an attached container, press <kbd>Ctrl+C</kbd>.</span></span>

## <a name="execute-a-command-in-a-container"></a><span data-ttu-id="078fe-113">コンテナーでコマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="078fe-113">Execute a command in a container</span></span>

<span data-ttu-id="078fe-114">Azure Container Instances は、実行中のコンテナーでのコマンドの実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="078fe-114">Azure Container Instances supports executing a command in a running container.</span></span> <span data-ttu-id="078fe-115">既に開始されているコンテナー内でのコマンドの実行は、アプリケーションの開発とトラブルシューティング時に特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="078fe-115">Running a command in a container you've already started is especially helpful during application development and troubleshooting.</span></span> <span data-ttu-id="078fe-116">この機能の最も一般的な用途は、対話型シェルを起動して、実行中のコンテナーで発生した問題をデバッグできるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="078fe-116">The most common use of this feature is to launch an interactive shell so that you can debug issues in a running container.</span></span>

<span data-ttu-id="078fe-117">この例では、実行中のコンテナーで対話型のターミナル セッションを開始します。</span><span class="sxs-lookup"><span data-stu-id="078fe-117">This example starts an interactive terminal session with the running container:</span></span>

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

<span data-ttu-id="078fe-118">コマンドが完了すると、実質的にコンテナー内で作業していることになります。</span><span class="sxs-lookup"><span data-stu-id="078fe-118">Once the command has completed, you are effectively working inside of the container.</span></span> <span data-ttu-id="078fe-119">この例では、作業ディレクトリの内容を表示する `ls` コマンドを実行しました。</span><span class="sxs-lookup"><span data-stu-id="078fe-119">In this example, the `ls` command was run to display the contents of the working directory:</span></span>

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

<span data-ttu-id="078fe-120">`exit` を実行してリモート セッションを停止します。</span><span class="sxs-lookup"><span data-stu-id="078fe-120">Run `exit` to stop the remote session.</span></span>

## <a name="monitor-container-cpu-and-memory"></a><span data-ttu-id="078fe-121">コンテナーの CPU とメモリを監視する</span><span class="sxs-lookup"><span data-stu-id="078fe-121">Monitor container CPU and memory</span></span>

<span data-ttu-id="078fe-122">CPU とメモリの使用量のメトリックをプルしたい場合があります。</span><span class="sxs-lookup"><span data-stu-id="078fe-122">You may want to pull metrics on CPU and memory usage.</span></span> <span data-ttu-id="078fe-123">これを行うには、まず、Azure コンテナー インスタンスの ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="078fe-123">To do so, first get the ID of the Azure container instance.</span></span> <span data-ttu-id="078fe-124">この例では、ID は `CONTAINER_ID` という名前の変数に配置されています。</span><span class="sxs-lookup"><span data-stu-id="078fe-124">In this example, the ID is placed in a variable named `CONTAINER_ID`:</span></span>

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

<span data-ttu-id="078fe-125">ここで、`az monitor metrics list` コマンドを使用して、CPU の使用量の情報を戻します。</span><span class="sxs-lookup"><span data-stu-id="078fe-125">Now, use the `az monitor metrics list` command to pull back CPU usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
```

<span data-ttu-id="078fe-126">出力例:</span><span class="sxs-lookup"><span data-stu-id="078fe-126">Example output:</span></span>

```output
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

<span data-ttu-id="078fe-127">メモリ使用量の情報を取得する場合は、次のコマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="078fe-127">The following command can be used to get memory usage information:</span></span>

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
```

<span data-ttu-id="078fe-128">出力例:</span><span class="sxs-lookup"><span data-stu-id="078fe-128">Example output:</span></span>

```output
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

<span data-ttu-id="078fe-129">この情報は、Azure portal でも見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="078fe-129">This information is also available in the Azure portal.</span></span> <span data-ttu-id="078fe-130">CPU とメモリ使用量の情報を図で確認する場合は、コンテナー インスタンスの Azure portal の概要ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="078fe-130">To see a visual representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.</span></span>

![Azure Container Instances の CPU とメモリ使用量の情報を示す Azure portal のビュー](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
