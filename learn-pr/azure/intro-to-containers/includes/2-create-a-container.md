<span data-ttu-id="589e1-101">コンテナーを実行、一覧表示、削除する一般的な方法について説明する前に、Ubuntu 仮想マシン (VM) でホストされている Docker コンテナーで実行される基本的な Nginx の構成を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="589e1-101">Before we cover some of the common ways to run, list, and delete containers, let's see a basic Nginx configuration running on a Docker container that's hosted on an Ubuntu virtual machine (VM).</span></span>

<span data-ttu-id="589e1-102">このパートで学習する内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="589e1-102">In this part, you'll learn how to:</span></span>

1. <span data-ttu-id="589e1-103">VM の初回起動時に Docker をインストールするように構成された Linux VM を作成する。</span><span class="sxs-lookup"><span data-stu-id="589e1-103">Create a Linux VM that's configured to install Docker when the VM first boots.</span></span>
1. <span data-ttu-id="589e1-104">VM に接続し、Nginx を実行する Docker コンテナーを開始する。</span><span class="sxs-lookup"><span data-stu-id="589e1-104">Connect to your VM and start a Docker container running Nginx.</span></span>
1. <span data-ttu-id="589e1-105">ポート バインドを使用して、VM の外部から Web サーバーにアクセスできるようにする。</span><span class="sxs-lookup"><span data-stu-id="589e1-105">Use port binding to make your web server accessible from outside the VM.</span></span>

## <a name="what-are-containers"></a><span data-ttu-id="589e1-106">コンテナーとは</span><span class="sxs-lookup"><span data-stu-id="589e1-106">What are containers?</span></span>

<span data-ttu-id="589e1-107">コンテナーは仮想化環境ですが、仮想マシンとは異なり、完全なオペレーティング システムを常に含むとは限りません。</span><span class="sxs-lookup"><span data-stu-id="589e1-107">A container is a virtualization environment but, unlike a virtual machine, it does not always include a full operating system.</span></span> <span data-ttu-id="589e1-108">代わりに、そのコンテナーを実行しているホスト環境のオペレーティング システムを参照します。</span><span class="sxs-lookup"><span data-stu-id="589e1-108">Instead, it references the operating system of the host environment that runs that container.</span></span> <span data-ttu-id="589e1-109">たとえば、特定の Linux カーネルを使用するサーバーで 5 つのコンテナーを実行すると、5 つのコンテナーすべてがその同じカーネルで実行されます。</span><span class="sxs-lookup"><span data-stu-id="589e1-109">For example, if you run five containers on a server with a specific Linux kernel, all five containers are running on that same kernel.</span></span>

<span data-ttu-id="589e1-110">コンテナーを使用すると、アプリケーションとその実行に必要なすべてのものを、"_コンテナー イメージ_" と呼ばれるものにパッケージ化できます。</span><span class="sxs-lookup"><span data-stu-id="589e1-110">Containers enable you to package your application and everything that it needs to run into what's known as a _container image_.</span></span> <span data-ttu-id="589e1-111">コンテナーは分離されており、これは同じシステムで実行している他のコンテナーに干渉しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="589e1-111">Containers are isolated, which means they don't interfere with any other container running on the same system.</span></span> <span data-ttu-id="589e1-112">コンテナー イメージはポータブルでもあります。</span><span class="sxs-lookup"><span data-stu-id="589e1-112">Container images are also portable.</span></span> <span data-ttu-id="589e1-113">Docker Hub や Azure Container Registry などのコンテナー レジストリに、イメージをアップロードできます。</span><span class="sxs-lookup"><span data-stu-id="589e1-113">You can upload your images to a container registry, such as Docker Hub or Azure Container Registry.</span></span> <span data-ttu-id="589e1-114">その後、クラウド内でコンテナーを実行でき、開発環境のときとまったく同じように動作することを期待できます。</span><span class="sxs-lookup"><span data-stu-id="589e1-114">You can then run your containers in the cloud and expect them to behave the exact same way as they do in your development environment.</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

<span data-ttu-id="589e1-115">コンテナーを使用すると繰り返しを速くできます。</span><span class="sxs-lookup"><span data-stu-id="589e1-115">Containers enable rapid iteration.</span></span> <span data-ttu-id="589e1-116">コンテナーは、オペレーティング システムを含む必要がないため、通常、完全な VM よりかなり小さくなります。</span><span class="sxs-lookup"><span data-stu-id="589e1-116">Because containers don't need to include an operating system, they're typically much smaller than full VMs.</span></span> <span data-ttu-id="589e1-117">これにより、コンテナーの開始と停止を、多くの場合は秒単位で、迅速に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="589e1-117">This enables you to start and stop containers quickly, often in seconds.</span></span>

## <a name="understand-the-setup"></a><span data-ttu-id="589e1-118">セットアップについて</span><span class="sxs-lookup"><span data-stu-id="589e1-118">Understand the setup</span></span>

<span data-ttu-id="589e1-119">学習が目的なので、作業のためのサンドボックス環境は提供されています。</span><span class="sxs-lookup"><span data-stu-id="589e1-119">For learning purposes, we provide a sandbox environment for you to work in.</span></span> <span data-ttu-id="589e1-120">この環境には、Azure リソースを管理および開発するためのブラウザー ベースのコマンド ライン エクスペリエンスである Azure Cloud Shell が含まれます。</span><span class="sxs-lookup"><span data-stu-id="589e1-120">This environment includes Azure Cloud Shell, a browser-based command-line experience for managing and developing Azure resources.</span></span>

<span data-ttu-id="589e1-121">Cloud Shell から Docker を含む Linux VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="589e1-121">From Cloud Shell, you'll create a Linux VM that includes Docker.</span></span> <span data-ttu-id="589e1-122">Docker コンテナーを対話的に作成および使用できるように、SSH 経由で Linux VM に接続します。</span><span class="sxs-lookup"><span data-stu-id="589e1-122">You'll connect to your Linux VM over SSH so that you can interactively create and use Docker containers.</span></span>

<span data-ttu-id="589e1-123">この Linux VM を、開発環境およびコンテナーについて学習する場所と考えてください。</span><span class="sxs-lookup"><span data-stu-id="589e1-123">Think of this Linux VM as your development environment and a space to learn about containers.</span></span> <span data-ttu-id="589e1-124">実際には、アプリの開発に使用するコンピューターに Docker をインストールします。</span><span class="sxs-lookup"><span data-stu-id="589e1-124">In practice, you would install Docker on the computer you use to develop your apps.</span></span> <span data-ttu-id="589e1-125">Docker は、Windows、macOS、Linux で動作します。</span><span class="sxs-lookup"><span data-stu-id="589e1-125">Docker runs on Windows, macOS, and Linux.</span></span>

<span data-ttu-id="589e1-126">このモジュールの最後には、ローカル開発環境で Docker の実行についてさらに学習できるリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="589e1-126">We'll provide resources where you can learn more about running Docker in your local development environment at the end of this module.</span></span>

## <a name="what-is-nginx"></a><span data-ttu-id="589e1-127">Nginx とは</span><span class="sxs-lookup"><span data-stu-id="589e1-127">What is Nginx?</span></span>

<span data-ttu-id="589e1-128">Nginx (発音は "エンジンエックス") は、UNIX、Linux、macOS、および Windows で実行される、一般的な無料のオープン ソースの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="589e1-128">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="589e1-129">Web サーバーを構成するのは、コンテナーの動作を確認するのによい方法です。</span><span class="sxs-lookup"><span data-stu-id="589e1-129">Configuring a web server is a good way to see containers in action.</span></span> <span data-ttu-id="589e1-130">ここでは、Nginx を使用して基本的な Web ページを提供します。</span><span class="sxs-lookup"><span data-stu-id="589e1-130">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="what-is-cloud-init"></a><span data-ttu-id="589e1-131">cloud-init とは</span><span class="sxs-lookup"><span data-stu-id="589e1-131">What is cloud-init?</span></span>

<span data-ttu-id="589e1-132">cloud-Init を使用すると、初回起動時の Linux VM をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="589e1-132">Cloud-init enables you to customize a Linux VM as it boots for the first time.</span></span> <span data-ttu-id="589e1-133">cloud-init を使って、パッケージをインストールしてファイルを書き込んだり、ユーザーとセキュリティを構成したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="589e1-133">You can use cloud-init to install packages and write files, or to configure users and security.</span></span> <span data-ttu-id="589e1-134">ここでは、cloud-init スクリプトを使って、VM に Docker をインストールします。</span><span class="sxs-lookup"><span data-stu-id="589e1-134">Here you'll use a cloud-init script to install Docker on your VM.</span></span>

## <a name="what-is-port-binding"></a><span data-ttu-id="589e1-135">ポートのバインドとは</span><span class="sxs-lookup"><span data-stu-id="589e1-135">What is port binding?</span></span>

<span data-ttu-id="589e1-136">ポートのバインドを使用すると、ホストからコンテナーにネットワーク トラフィックを転送することができます。</span><span class="sxs-lookup"><span data-stu-id="589e1-136">Port binding enables you to forward network traffic to a container from its host.</span></span>

<span data-ttu-id="589e1-137">Docker コンテナーは、コンテナーのホスト システム上のローカル ネットワークで実行されます。</span><span class="sxs-lookup"><span data-stu-id="589e1-137">A Docker container runs on a local network on the container's host system.</span></span> <span data-ttu-id="589e1-138">この演習では、コンテナーのネットワークは Linux VM 上にあります。</span><span class="sxs-lookup"><span data-stu-id="589e1-138">In this case, the container's network will be on your Linux VM.</span></span>

<span data-ttu-id="589e1-139">通常、Web 要求はポート 80 (HTTP) で動作することはご存じでしょう。</span><span class="sxs-lookup"><span data-stu-id="589e1-139">You know that web requests typically run over port 80 (HTTP).</span></span> <span data-ttu-id="589e1-140">コンテナーは VM 内部のローカル ネットワーク上で動作しているため、コンテナーは外部にはアクセス可できません。</span><span class="sxs-lookup"><span data-stu-id="589e1-140">Because the container is running on a local network inside your VM, the container is not accessible to the outside world.</span></span>

<span data-ttu-id="589e1-141">Docker を使用すると、ポートを発行つまり公開して、VM の外部からポートにアクセスできるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="589e1-141">Docker enables you to publish, or expose, a port to make it accessible from outside the VM.</span></span> <span data-ttu-id="589e1-142">ここでは、VM 上のポート 8080 へのネットワーク トラフィックが、コンテナーのポート 80 に転送されるように、コンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="589e1-142">Here, you'll configure your container so that network traffic to port 8080 on your VM will be forwarded to port 80 on your container.</span></span>

## <a name="create-a-virtual-machine-to-host-docker"></a><span data-ttu-id="589e1-143">Docker をホストする仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="589e1-143">Create a virtual machine to host Docker</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a><span data-ttu-id="589e1-144">cloud-init スクリプトを作成する</span><span class="sxs-lookup"><span data-stu-id="589e1-144">Create the cloud-init script</span></span>

1. <span data-ttu-id="589e1-145">**clouddrive** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="589e1-145">Navigate to the **clouddrive** folder.</span></span>

    ```azurecli
    cd clouddrive
    ```

1. <span data-ttu-id="589e1-146">**vm-config** という名前の新しいフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="589e1-146">Create a new folder named **vm-config**.</span></span>

    ```azurecli
    mkdir vm-config
    ```

1. <span data-ttu-id="589e1-147">新しいフォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="589e1-147">Navigate to the new folder.</span></span>

    ```azurecli
    cd vm-config
    ```

1. <span data-ttu-id="589e1-148">Cloud Shell の統合されたエディターを使用して、新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="589e1-148">Create a new file using Cloud Shell's integrated editor.</span></span>

    ```azurecli
    code .
    ```

1. <span data-ttu-id="589e1-149">この cloud-init スクリプトをエディターに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="589e1-149">Paste this cloud-init script into the editor.</span></span>

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```yaml
    #cloud-config
    package_upgrade: true
    write_files:
      - path: /etc/systemd/system/docker.service.d/docker.conf
        content: |
          [Service]
          ExecStart=
          ExecStart=/usr/bin/dockerd
      - path: /etc/docker/daemon.json
        content: |
          {
            "hosts": ["fd://","tcp://127.0.0.1:2375"]
          }
    runcmd:
      - curl -sSL https://get.docker.com/ | sh
      - usermod -aG docker azureuser
    ```

1. <span data-ttu-id="589e1-150">ファイルを **docker-vm-config.txt** として保存します (<kbd>Ctrl + S</kbd> キーを押すか、右クリックして **[保存]** を選択)。</span><span class="sxs-lookup"><span data-stu-id="589e1-150">Save the file as **docker-vm-config.txt** (<kbd>Ctrl+S</kbd> or right click and select **Save**).</span></span>

1. <span data-ttu-id="589e1-151">エディターを閉じます (<kbd>Ctrl + Q</kbd> キーを押すか、右クリックして **[終了]** を選択)。</span><span class="sxs-lookup"><span data-stu-id="589e1-151">Close the editor (<kbd>Ctrl+Q</kbd> or right click and select **Quit**).</span></span>

### <a name="create-the-vm"></a><span data-ttu-id="589e1-152">VM を作成する</span><span class="sxs-lookup"><span data-stu-id="589e1-152">Create the VM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="589e1-153">通常なら、`az group create` コマンドを使用して関連するすべての Azure リソースに対してリソース グループを作成しますが、以下の演習では、使用するリソース グループが既に作成されています。</span><span class="sxs-lookup"><span data-stu-id="589e1-153">Normally, you would create a resource group for all your related Azure resources with the `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="589e1-154">すべての手順で、自分のリソース グループとして、"**<rgn>[サンドボックス リソース グループ名]</rgn>**" を使用してください。</span><span class="sxs-lookup"><span data-stu-id="589e1-154">Use "**<rgn>[sandbox resource group name]</rgn>**" as your resource group in all the steps.</span></span>

1. <span data-ttu-id="589e1-155">この `az vm create` コマンドを実行して、Linux VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="589e1-155">Run this `az vm create` command to create your Linux VM.</span></span>

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

<span data-ttu-id="589e1-156">`--custom-data` 引数では、VM の初回起動時に Docker をインストールする cloud-init スクリプトを指定します。</span><span class="sxs-lookup"><span data-stu-id="589e1-156">The `--custom-data` argument specifies the cloud-init script that installs Docker when the VM first boots.</span></span> <span data-ttu-id="589e1-157">この処理が完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="589e1-157">The process will take a few minutes to complete.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="589e1-158">VM に接続する</span><span class="sxs-lookup"><span data-stu-id="589e1-158">Connect to the VM</span></span>

<span data-ttu-id="589e1-159">ここでは、VM に SSH で接続して VM を構成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="589e1-159">Here you'll create an SSH connection to your VM so that you can configure it.</span></span>

1. <span data-ttu-id="589e1-160">この `az vm show` コマンドを実行して、VM の IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="589e1-160">Run this `az vm show` command to get your VM's IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="589e1-161">VM に SSH で接続します。</span><span class="sxs-lookup"><span data-stu-id="589e1-161">SSH into the VM.</span></span> <span data-ttu-id="589e1-162">**ip-address** を、実際に使用する値に置換します。</span><span class="sxs-lookup"><span data-stu-id="589e1-162">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="589e1-163">Docker のバージョンを確認します。</span><span class="sxs-lookup"><span data-stu-id="589e1-163">Verify the Docker version.</span></span>

    ```bash
    docker version
    ```

    > [!NOTE]
    > <span data-ttu-id="589e1-164">コマンドが失敗する場合、または Docker デーモン ソケットへの接続に関するエラー メッセージが表示される場合は、cloud-init スクリプトがまだ完了していないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="589e1-164">If the command fails, or you see an error message about connecting to the Docker daemon socket, that means that the cloud-init script hasn't yet completed.</span></span> <span data-ttu-id="589e1-165">スクリプトが完了するまで待つ方法もありますが、ここでは、`exit` を実行して SSH セッションを終了し、1、2 分してから再接続してみます。</span><span class="sxs-lookup"><span data-stu-id="589e1-165">Although there are ways to wait for the script to complete, for now run `exit` to leave your SSH session and try reconnecting in a minute or two.</span></span>

## <a name="start-nginx"></a><span data-ttu-id="589e1-166">Nginx を開始する</span><span class="sxs-lookup"><span data-stu-id="589e1-166">Start Nginx</span></span>

<span data-ttu-id="589e1-167">ここでは、Nginx を実行する Docker コンテナーを開始します。</span><span class="sxs-lookup"><span data-stu-id="589e1-167">Here you'll start a Docker container that's running Nginx.</span></span>

1. <span data-ttu-id="589e1-168">この `docker run` コマンドを実行して、Nginx を実行する Docker コンテナーを開始します。</span><span class="sxs-lookup"><span data-stu-id="589e1-168">Run this `docker run` command to start a Docker container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    <span data-ttu-id="589e1-169">このコマンドは、Nginx で構成済みの Docker イメージを [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true) からダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="589e1-169">The command downloads a Docker image that's preconfigured with Nginx from [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true).</span></span>

    <span data-ttu-id="589e1-170">`--name` 引数は、後で操作できるように、Docker コンテナーに "nginx" という名前を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="589e1-170">The `--name` argument assigns the name "nginx" to your Docker container so you can work with it later.</span></span>

    <span data-ttu-id="589e1-171">`-p` 引数は、VM のポート 8080 へのネットワーク トラフィックを、コンテナーのポート 80 に転送します。</span><span class="sxs-lookup"><span data-stu-id="589e1-171">The `-p` argument forwards network traffic to port 8080 on your VM to port 80 on your container.</span></span>

1. <span data-ttu-id="589e1-172">`curl` を実行し、Nginx が実行していてアクセス可能であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="589e1-172">Run `curl` to verify that Nginx is running and accessible.</span></span>

    ```bash
    curl http://localhost:8080
    ```

1. <span data-ttu-id="589e1-173">SSH セッションを終了します。</span><span class="sxs-lookup"><span data-stu-id="589e1-173">Exit your SSH session.</span></span>

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a><span data-ttu-id="589e1-174">Web ブラウザーから Web サーバーにアクセスする</span><span class="sxs-lookup"><span data-stu-id="589e1-174">Access your web server from a web browser</span></span>

<span data-ttu-id="589e1-175">ポート 8080 へのネットワーク トラフィックがコンテナーのポート 80 に転送されるようにコンテナーを構成したことを、思い出してください。</span><span class="sxs-lookup"><span data-stu-id="589e1-175">Recall that you configured your container so that network traffic on port 8080 is forwarded to port 80 on your container.</span></span>

<span data-ttu-id="589e1-176">ポート マッピングが動作していることを確認するため、ここでは Azure ファイアウォールを通して VM へのポート 8080 を開きます。</span><span class="sxs-lookup"><span data-stu-id="589e1-176">To see port mapping in action, here you'll open port 8080 to your VM through the Azure firewall.</span></span> <span data-ttu-id="589e1-177">その後、ブラウザーから Web サーバーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="589e1-177">Then you'll access your web server from a browser.</span></span>

1. <span data-ttu-id="589e1-178">この `az vm open-port` コマンドを実行し、ファイアウォールを経由して VM のポート 8080 を開きます。</span><span class="sxs-lookup"><span data-stu-id="589e1-178">Run this `az vm open-port` command to open port 8080 on your VM through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. <span data-ttu-id="589e1-179">Web ブラウザーから、自分の VM の IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="589e1-179">From a web browser, navigate to your VM's IP address.</span></span> <span data-ttu-id="589e1-180">必ずポート 8080 を含めます (例: **http://40.121.106.164:8080**)。</span><span class="sxs-lookup"><span data-stu-id="589e1-180">Be sure to include port 8080, for example,  **http://40.121.106.164:8080**.</span></span>

    <span data-ttu-id="589e1-181">既定のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="589e1-181">You see the default home page.</span></span>

    ![ブラウザーで実行されている Web サイト](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a><span data-ttu-id="589e1-183">コンテナーを停止する</span><span class="sxs-lookup"><span data-stu-id="589e1-183">Stop the container</span></span>

<span data-ttu-id="589e1-184">ここではコンテナーを停止します。</span><span class="sxs-lookup"><span data-stu-id="589e1-184">Now let's stop the container.</span></span>

1. <span data-ttu-id="589e1-185">まず、VM に再びログインします。</span><span class="sxs-lookup"><span data-stu-id="589e1-185">First, log back in to your VM.</span></span> <span data-ttu-id="589e1-186">念のためその方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="589e1-186">Here's a refresher.</span></span>

    <span data-ttu-id="589e1-187">IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="589e1-187">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    <span data-ttu-id="589e1-188">VM に SSH で接続します。</span><span class="sxs-lookup"><span data-stu-id="589e1-188">SSH into the VM.</span></span> <span data-ttu-id="589e1-189">**ip-address** を、実際に使用する値に置換します。</span><span class="sxs-lookup"><span data-stu-id="589e1-189">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

1. <span data-ttu-id="589e1-190">`docker stop nginx` を実行して、"nginx" という名前のコンテナーを停止します。</span><span class="sxs-lookup"><span data-stu-id="589e1-190">Run `docker stop nginx` to stop the container named "nginx".</span></span>

    ```bash
    docker stop nginx
    ```

<span data-ttu-id="589e1-191">次のパートのため、SSH 接続は開いたままにします。</span><span class="sxs-lookup"><span data-stu-id="589e1-191">Keep your SSH connection open for the next part.</span></span>

<span data-ttu-id="589e1-192">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="589e1-192">Congratulations!</span></span> <span data-ttu-id="589e1-193">仮想マシンが正常に作成され、その仮想マシンを使用して、Nginx でコンテナー化された Web サーバーをホストしました。</span><span class="sxs-lookup"><span data-stu-id="589e1-193">You've successfully created a virtual machine and used it to host an Nginx containerized web server.</span></span>
