<span data-ttu-id="1683f-101">データ サイエンス仮想マシンが稼働状態になったので、最初のディープ ラーニング モデルをトレーニングすることにします。</span><span class="sxs-lookup"><span data-stu-id="1683f-101">Now that you have your Data Science Virtual Machine up and running, you decide to train your first deep learning model.</span></span> <span data-ttu-id="1683f-102">サーバー上の他のすべてのものから分離して実験する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1683f-102">You want to run your experiments in isolation from everything else on your server.</span></span> <span data-ttu-id="1683f-103">そのために、Docker コンテナー内で実験を実行します。</span><span class="sxs-lookup"><span data-stu-id="1683f-103">To do that, you run them within a Docker container.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="1683f-104">SSH を使って VM に接続する</span><span class="sxs-lookup"><span data-stu-id="1683f-104">Connect to the VM with ssh</span></span>

<span data-ttu-id="1683f-105">まだ SSH で VM に接続していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1683f-105">Make sure you're still connected to your VM through ssh.</span></span> <span data-ttu-id="1683f-106">接続していない場合は、もう一度次のコマンドを実行して仮想マシンにリモート接続します。</span><span class="sxs-lookup"><span data-stu-id="1683f-106">If not, just run this command again to remote back into the virtual machine.</span></span>

1. <span data-ttu-id="1683f-107">Azure Cloud Shell で次のコマンドを実行して VM にサインインします。</span><span class="sxs-lookup"><span data-stu-id="1683f-107">Execute the following command in Azure Cloud Shell to sign into the VM.</span></span>

    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 
    
    <span data-ttu-id="1683f-108">`<USERNAME>` を、VM 作成時に定義した管理者アカウント名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1683f-108">Replace  `<USERNAME>` with the admin account name you defined when creating the VM.</span></span> <span data-ttu-id="1683f-109">`<IP>` を、前の演習で保存した VM の IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1683f-109">Replace `<IP>` with the IP address of the VM you saved in the preceding exercise.</span></span>  

1. <span data-ttu-id="1683f-110">メッセージが表示されたら、管理者アカウントのパスワードを入力して、サインイン プロセスを完了します。</span><span class="sxs-lookup"><span data-stu-id="1683f-110">When prompted, enter your password for the admin account to complete the sign-in process.</span></span>

## <a name="run-a-jupyter-notebook-server-in-a-docker-container-in-the-vm"></a><span data-ttu-id="1683f-111">VM で Docker コンテナー内の Jupyter Notebook サーバーを実行する</span><span class="sxs-lookup"><span data-stu-id="1683f-111">Run a Jupyter Notebook server in a Docker container in the VM</span></span>

> [!NOTE]
> <span data-ttu-id="1683f-112">VM の管理者アカウントつまりルート アカウントしか持っていないので、`sudo` を使用してすべての Docker コマンドをルートとして実行する必要があります</span><span class="sxs-lookup"><span data-stu-id="1683f-112">Since we only have an admin, or root, account on our VM, we have to run all Docker commands as root using `sudo`</span></span>

1. <span data-ttu-id="1683f-113">VM 上に存在する Docker コンテナーを確認するには、コマンド プロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1683f-113">To see what Docker containers exist on the VM, run the following command at the command prompt.</span></span>

    ```azurecli 
    sudo docker ps
    ```

1. <span data-ttu-id="1683f-114">コマンド プロンプトで次のコマンドを実行して、実験用の新しいコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1683f-114">Run the following command at the command prompt to create a new container for our experiments.</span></span>

    ```azurecli 
    sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
    'conda install jupyter matplotlib -y &&\
    curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
    ``` 

    <span data-ttu-id="1683f-115">このコマンドの実行には時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="1683f-115">This command will run for quite a while.</span></span> <span data-ttu-id="1683f-116">そのため、しばらく時間があるので、コマンドの機能について説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="1683f-116">So, while we have some time, let's discuss what the command does.</span></span> 
    - <span data-ttu-id="1683f-117">`docker run` は、新しいコンテナーでコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1683f-117">`docker run` runs a command in a new container.</span></span> <span data-ttu-id="1683f-118">使用されている Docker イメージは、pytorch/pytorch です。</span><span class="sxs-lookup"><span data-stu-id="1683f-118">The Docker image being used is pytorch/pytorch.</span></span> <span data-ttu-id="1683f-119">最初に指定されたイメージの上に書き込み可能なコンテナー レイヤーを作成した後、指定されたコマンドを使用してそれを起動します。</span><span class="sxs-lookup"><span data-stu-id="1683f-119">It first creates a writeable container layer over the specified image, and then starts it using the specified command.</span></span>
    - <span data-ttu-id="1683f-120">`--rm` は、終了したコンテナーを削除します。</span><span class="sxs-lookup"><span data-stu-id="1683f-120">`--rm` will remove the container once it exits.</span></span> <span data-ttu-id="1683f-121">コンテナーを残しておく必要がある場合は、この引数を削除します。</span><span class="sxs-lookup"><span data-stu-id="1683f-121">If you want to keep the container around, drop this argument.</span></span> 
    - <span data-ttu-id="1683f-122">`--entrypoint 'bin/sh'` は、イメージの既定のエントリ ポイントを Bash シェルに上書きします。</span><span class="sxs-lookup"><span data-stu-id="1683f-122">`--entrypoint 'bin/sh'` overwrites the default entry point of the image to be the Bash shell</span></span>
    - <span data-ttu-id="1683f-123">`-c` は、コンテナーの起動時に実行するコマンドを定義します。</span><span class="sxs-lookup"><span data-stu-id="1683f-123">`-c` defines what command to run when the container starts.</span></span> <span data-ttu-id="1683f-124">ここでは、次の 3 つのコマンドを実行しています。</span><span class="sxs-lookup"><span data-stu-id="1683f-124">In this case, it's running three commands:</span></span>
        - <span data-ttu-id="1683f-125">Jupyter と matplotlib をインストールします</span><span class="sxs-lookup"><span data-stu-id="1683f-125">Installs Jupyter and matplotlib</span></span>
        - <span data-ttu-id="1683f-126">ノートブック (cifar10_tutorial.ipynb) を pytorch.org から `first_pytorch_classifier.ipynb` という名前のコンテナー内のファイルにコピーします</span><span class="sxs-lookup"><span data-stu-id="1683f-126">Copies a notebook (cifar10_tutorial.ipynb) from pytorch.org to a file in the container called `first_pytorch_classifier.ipynb`</span></span>
        - <span data-ttu-id="1683f-127">前の演習と同じ方法で、コンテナー内の Notebook サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="1683f-127">Starts the notebook server in the container in the same way as the preceding exercise.</span></span>  <span data-ttu-id="1683f-128">ブラウザーは開始されず、ノートブックにはルートからアクセスでき、ノートブックはすべてのポートでリッスンできます。</span><span class="sxs-lookup"><span data-stu-id="1683f-128">No browser is started, allow the notebook to be accessed by root and listen on all ports.</span></span> 
    
    <span data-ttu-id="1683f-129">Notebook サーバーは、そのコンテナーのすべてのポートでリッスンします。</span><span class="sxs-lookup"><span data-stu-id="1683f-129">The notebook server listens on all ports for that container.</span></span> <span data-ttu-id="1683f-130">しかし、コンテナー外からのトラフィックはどのように送られるのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="1683f-130">But, how will traffic come from outside the container?</span></span> <span data-ttu-id="1683f-131">`-p 8888:8888` 引数は、コンテナーのポート `8888` をホスト マシンの TCP ポート 8888 にバインドします。</span><span class="sxs-lookup"><span data-stu-id="1683f-131">The `-p 8888:8888` argument binds port `8888` of the container to TCP port 8888 on the host machine.</span></span> <span data-ttu-id="1683f-132">そのため、ポート 8888 経由で VM に送信されるトラフィックは、コンテナーによってピックアップされます。</span><span class="sxs-lookup"><span data-stu-id="1683f-132">So, traffic coming to the VM over port 8888 will be picked up by the container.</span></span> 

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a><span data-ttu-id="1683f-133">リモート ブラウザーから Jupyter Notebook サーバーに接続する</span><span class="sxs-lookup"><span data-stu-id="1683f-133">Connect to the Jupyter Notebook server from a remote browser</span></span> 

<span data-ttu-id="1683f-134">Jupyter Notebook がコンテナーで実行されると、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1683f-134">Once the Jupyter notebook is running in the container, you'll  see a message similar to the following message.</span></span> 

> <span data-ttu-id="1683f-135">*Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken} (初めて接続するときは、この URL をコピーしてブラウザーに貼り付け、トークンを使用してログインします: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken})*</span><span class="sxs-lookup"><span data-stu-id="1683f-135">*Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}*</span></span>

1. <span data-ttu-id="1683f-136">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the URL with the Fully Qualified Domain Name (FQDN) or the IP address of the VM and navigate to the address in a new a tab of your browser. (URL の http://(5b8783e7911d または 127.0.0.1) の部分を完全修飾ドメイン名 (FQDN) または VM の IP アドレスに置き換え、ブラウザーの新しいタブでアドレスに移動します。)</span><span class="sxs-lookup"><span data-stu-id="1683f-136">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the URL with the Fully Qualified Domain Name (FQDN) or the IP address of the VM and navigate to the address in a new a tab of your browser.</span></span>

    ![<span data-ttu-id="1683f-137">Jupyter Notebook のダッシュボードのスクリーンショット。</span><span class="sxs-lookup"><span data-stu-id="1683f-137">Screenshot showing Jupyter Notebooks dashboard.</span></span> ](../media/notebook-in-docker.png)

    > [!TIP]
    > <span data-ttu-id="1683f-138">お使いの VM の FQDN と IP アドレスは、次のコマンドを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="1683f-138">You can get the FQDN and IP address of your VM with the following command:</span></span>
    > 
    > `az vm show -d --name <HOSTNAME> --resource-group <rgn>[sandbox resource group name]</rgn> --output table`
    >
    > <span data-ttu-id="1683f-139">`<HOSTNAME>` は VM に付けた名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="1683f-139">Remember to replace `<HOSTNAME>` with the name you gave your VM.</span></span> 
    
    <span data-ttu-id="1683f-140">今度は、ノートブックが 1 つだけ表示されます。</span><span class="sxs-lookup"><span data-stu-id="1683f-140">This time we only see a single notebook.</span></span> <span data-ttu-id="1683f-141">今はコンテナー内にいて、このノートブックのみをコピーしたためです。</span><span class="sxs-lookup"><span data-stu-id="1683f-141">That's because we're in a container and only copied down this notebook.</span></span> <span data-ttu-id="1683f-142">次の演習では、このノートブックを調べます。</span><span class="sxs-lookup"><span data-stu-id="1683f-142">In the next exercise, we'll experiment with this notebook.</span></span> 
    
    > [!TIP]
    > <span data-ttu-id="1683f-143">まだ Notebook サーバーをシャットダウンしないでください。</span><span class="sxs-lookup"><span data-stu-id="1683f-143">Don't shut down the notebook server just yet.</span></span> <span data-ttu-id="1683f-144">次の演習で `first_pytorch_classifier.ipynb` ノートブックを調べます。</span><span class="sxs-lookup"><span data-stu-id="1683f-144">We'll look at the `first_pytorch_classifier.ipynb` notebook in the next exercise.</span></span>