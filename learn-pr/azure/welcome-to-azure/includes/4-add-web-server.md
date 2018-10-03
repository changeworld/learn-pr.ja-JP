<span data-ttu-id="3566b-101">これで、VM が稼働したので、これを使って何かしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="3566b-101">Now that your VM is up and running, let's do something with it.</span></span> <span data-ttu-id="3566b-102">ここでは、Web サーバーをインストールし、VM のホスト名を表示する基本的な Web ページを提供します。</span><span class="sxs-lookup"><span data-stu-id="3566b-102">Here you'll install a web server and serve up a basic web page that displays the VM's hostname.</span></span>

<span data-ttu-id="3566b-103">VM を構成するには、いくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="3566b-103">To configure a VM, you have several choices.</span></span> <span data-ttu-id="3566b-104">直接接続して、対話形式でシステムを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="3566b-104">You can connect directly and interactively configure your system.</span></span> <span data-ttu-id="3566b-105">たとえば、Windows システムでは、リモート デスクトップ セッションを作成してリモート Windows コンピューターの UI に接続して、まるで自分のコンピューターのように操作することができます。</span><span class="sxs-lookup"><span data-stu-id="3566b-105">For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it.</span></span> <span data-ttu-id="3566b-106">Linux システムでは、SSH 接続を作成して、ターミナルからご自分のリモート Linux システムを安全に使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3566b-106">On Linux systems, you can create an SSH connection to securely work with your remote Linux system from the terminal.</span></span>

<span data-ttu-id="3566b-107">手動構成は出発点としては有効ですが、システムを追加するときに、デプロイを自動化できます。</span><span class="sxs-lookup"><span data-stu-id="3566b-107">Manual configuration is a good start, but as you add systems, you can automate your deployments.</span></span> <span data-ttu-id="3566b-108">オートメーションには、ユーザーの代わりに面倒な作業を処理するプログラムやスクリプトなどの反復可能なプロセスの実行が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3566b-108">Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.</span></span>

::: zone pivot="windows-cloud"

<span data-ttu-id="3566b-109">ここでは、カスタム スクリプト拡張機能と呼ばれる、Windows ベースの Azure 仮想マシンの機能を使用して、Cloud Shell セッションからリモートで IIS を構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-109">Here, you'll configure IIS remotely from your Cloud Shell session using a feature of Windows-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="linux-cloud"

<span data-ttu-id="3566b-110">ここでは、カスタム スクリプト拡張機能と呼ばれる、Linux ベースの Azure 仮想マシンの機能を使用して、Cloud Shell セッションからリモートで Nginx を構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-110">Here, you'll configure Nginx remotely from your Cloud Shell session using a feature of Linux-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a><span data-ttu-id="3566b-111">IIS とは</span><span class="sxs-lookup"><span data-stu-id="3566b-111">What is IIS?</span></span>

<span data-ttu-id="3566b-112">インターネット インフォメーション サービス (IIS) は、Windows で実行される Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3566b-112">Internet Information Services, or IIS, is a web server that runs on Windows.</span></span> <span data-ttu-id="3566b-113">IIS を使用して、標準的な Web コンテンツ (HTML、CSS、および JavaScript) を提供したり、ASP.NET やその他の Web アプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="3566b-113">You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications.</span></span> <span data-ttu-id="3566b-114">IIS は Windows Server に付属していますが、Web ページの提供を開始するには、アクティブ化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3566b-114">IIS comes with Windows Server, but you need to activate it to start serving web pages.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="3566b-115">カスタム スクリプト拡張機能とは</span><span class="sxs-lookup"><span data-stu-id="3566b-115">What's the Custom Script Extension?</span></span>

<span data-ttu-id="3566b-116">カスタム スクリプト拡張機能は、Azure VM でスクリプトをダウンロードして実行する簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="3566b-116">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="3566b-117">これは、VM が稼働してからシステムを構成できるさまざまな方法の 1 つに過ぎません。</span><span class="sxs-lookup"><span data-stu-id="3566b-117">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="3566b-118">スクリプトは、Azure Storage に格納することも、GitHub などの公開されている場所に格納することもできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-118">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="3566b-119">スクリプトは、手動で実行することも、より自動化されたデプロイの一部として実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-119">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="3566b-120">ここでは、Azure CLI コマンドを実行して、GitHub から PowerShell スクリプトをダウンロードし、VM でそれを実行します。</span><span class="sxs-lookup"><span data-stu-id="3566b-120">Here, you'll run an Azure CLI command to download a PowerShell script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="3566b-121">このスクリプトは IIS を構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-121">The script configures IIS.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="3566b-122">IIS を構成する</span><span class="sxs-lookup"><span data-stu-id="3566b-122">Configure IIS</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="3566b-123">ここでは、カスタム スクリプト拡張機能を使用して、Cloud Shell から VM に IIS をリモートで構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-123">Here you'll use the Custom Script Extension to configure IIS remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="3566b-124">また、ポート 80 (HTTP) で受信ネットワーク アクセスを許可するようにファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-124">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="3566b-125">Cloud Shell から `az vm extension set` コマンドを実行して、IIS をインストールして基本的なホーム ページを構成する PowerShell スクリプトをダウンロードして実行します。</span><span class="sxs-lookup"><span data-stu-id="3566b-125">From Cloud Shell, run this `az vm extension set` command to download and execute a PowerShell script that installs IIS and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    <span data-ttu-id="3566b-126">IIS を構成し、ホーム ページの内容を設定して、サービスを開始するプロセスは、完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="3566b-126">The process to configure IIS, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="3566b-127">それまで、別のブラウザー タブから、[PowerShell スクリプトを調べる](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true)こともできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-127">In the meantime, you can [examine the PowerShell script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="3566b-128">このスクリプトにより、IIS がインストールされ、VM のコンピューター名 "myVM" と共にウェルカム メッセージを表示するようにホーム ページが構成されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-128">The script installs IIS and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="3566b-129">この `az vm open-port` コマンドを実行して、ファイアウォールを経由してポート 80 (HTTP) を開きます。</span><span class="sxs-lookup"><span data-stu-id="3566b-129">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="3566b-130">構成を確認する</span><span class="sxs-lookup"><span data-stu-id="3566b-130">Verify the configuration</span></span>

<span data-ttu-id="3566b-131">これで IIS が設定されたので、実行されていることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="3566b-131">Now that IIS is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="3566b-132">この `az vm list-ip-addresses` コマンドを実行して、VM のパブリック IP を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="3566b-132">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="3566b-133">この `--query` 引数のため、このコマンドは少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="3566b-133">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="3566b-134">しかし、Azure をさらに深く探究すれば、これに関するプロになれます。</span><span class="sxs-lookup"><span data-stu-id="3566b-134">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="3566b-135">自分の VM のパブリック IP アドレス (たとえば 104.211.9.245) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-135">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="3566b-136">新しいブラウザー タブで、自分の VM の IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="3566b-136">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="3566b-137">ウェルカム メッセージと VM の名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-137">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a><span data-ttu-id="3566b-138">Nginx とは</span><span class="sxs-lookup"><span data-stu-id="3566b-138">What is Nginx?</span></span>

<span data-ttu-id="3566b-139">Nginx (発音は "エンジンエックス") は、UNIX、Linux、macOS、および Windows で実行される、一般的な無料のオープン ソースの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3566b-139">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="3566b-140">ここでは、Nginx を使用して基本的な Web ページを提供します。</span><span class="sxs-lookup"><span data-stu-id="3566b-140">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="3566b-141">カスタム スクリプト拡張機能とは</span><span class="sxs-lookup"><span data-stu-id="3566b-141">What's the Custom Script Extension?</span></span>

<span data-ttu-id="3566b-142">カスタム スクリプト拡張機能は、Azure VM でスクリプトをダウンロードして実行する簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="3566b-142">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="3566b-143">これは、VM が稼働してからシステムを構成できるさまざまな方法の 1 つに過ぎません。</span><span class="sxs-lookup"><span data-stu-id="3566b-143">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="3566b-144">スクリプトは、Azure Storage に格納することも、GitHub などの公開されている場所に格納することもできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-144">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="3566b-145">スクリプトは、手動で実行することも、より自動化されたデプロイの一部として実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-145">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="3566b-146">ここでは、Azure CLI コマンドを実行して、GitHub から Bash スクリプトをダウンロードし、VM でそれを実行します。</span><span class="sxs-lookup"><span data-stu-id="3566b-146">Here, you'll run an Azure CLI command to download a Bash script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="3566b-147">このスクリプトは Nginx を構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-147">The script configures Nginx.</span></span>

## <a name="configure-nginx"></a><span data-ttu-id="3566b-148">Nginx を構成する</span><span class="sxs-lookup"><span data-stu-id="3566b-148">Configure Nginx</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="3566b-149">ここでは、カスタム スクリプト拡張機能を使用して、Cloud Shell から VM に Nginx をリモートで構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-149">Here you'll use the Custom Script Extension to configure Nginx remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="3566b-150">また、ポート 80 (HTTP) で受信ネットワーク アクセスを許可するようにファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="3566b-150">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="3566b-151">Cloud Shell から `az vm extension set` コマンドを実行して、Nginx をインストールして基本的なホーム ページを構成する、Bash スクリプトをダウンロードして実行します。</span><span class="sxs-lookup"><span data-stu-id="3566b-151">From Cloud Shell, run this `az vm extension set` command to download and execute a Bash script that installs Nginx and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    <span data-ttu-id="3566b-152">Nginx を構成し、ホーム ページの内容を設定して、サービスを開始するプロセスは、完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="3566b-152">The process to configure Nginx, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="3566b-153">それまで、別のブラウザー タブから、[Bash スクリプトを調べる](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true)こともできます。</span><span class="sxs-lookup"><span data-stu-id="3566b-153">In the meantime, you can [examine the Bash script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="3566b-154">このスクリプトにより、Nginx がインストールされ、VM のコンピューター名 "myVM" と共にウェルカム メッセージを表示するようにホーム ページが構成されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-154">The script installs Nginx and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="3566b-155">この `az vm open-port` コマンドを実行して、ファイアウォールを経由してポート 80 (HTTP) を開きます。</span><span class="sxs-lookup"><span data-stu-id="3566b-155">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="3566b-156">構成を確認する</span><span class="sxs-lookup"><span data-stu-id="3566b-156">Verify the configuration</span></span>

<span data-ttu-id="3566b-157">これで Nginx が設定されたので、実行されていることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="3566b-157">Now that Nginx is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="3566b-158">この `az vm list-ip-addresses` コマンドを実行して、VM のパブリック IP を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="3566b-158">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="3566b-159">この `--query` 引数のため、このコマンドは少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="3566b-159">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="3566b-160">しかし、Azure をさらに深く探究すれば、これに関するプロになれます。</span><span class="sxs-lookup"><span data-stu-id="3566b-160">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="3566b-161">自分の VM のパブリック IP アドレス (たとえば 104.211.9.245) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-161">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="3566b-162">新しいブラウザー タブで、自分の VM の IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="3566b-162">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="3566b-163">ウェルカム メッセージと VM の名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3566b-163">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-nginx-browser.png)

::: zone-end

## <a name="summary"></a><span data-ttu-id="3566b-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="3566b-164">Summary</span></span>

<span data-ttu-id="3566b-165">VM が実行され、Web ページを提供できるようになりましたが、これはあなたにとってどんな意味がありますか。</span><span class="sxs-lookup"><span data-stu-id="3566b-165">Your VM is running and can now serve up web pages, but what does that mean for you?</span></span>

<span data-ttu-id="3566b-166">すべての体験は基本から始まり、企業の規模に関わらず、クラウドから誕生したほぼすべての優れたイノベーションも、始まりはあなたと同様のセットアップであったことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="3566b-166">Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours.</span></span> <span data-ttu-id="3566b-167">アイデアの進化に伴って、ご自分のビジネスとユーザーにプラスの影響が出始めます。</span><span class="sxs-lookup"><span data-stu-id="3566b-167">As your idea evolves, it begins making a positive impact on your business and your users.</span></span>