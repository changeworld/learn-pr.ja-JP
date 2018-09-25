<span data-ttu-id="8dd29-101">VM 上で最後に試してみたいのは、Web サーバーをインストールすることです。</span><span class="sxs-lookup"><span data-stu-id="8dd29-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="8dd29-102">インストールするのが最も簡単なパッケージの 1 つとして、`nginx` があります。</span><span class="sxs-lookup"><span data-stu-id="8dd29-102">One of the easiest packages to install is `nginx`.</span></span>

## <a name="install-nginx-web-server"></a><span data-ttu-id="8dd29-103">NGINX Web サーバーをインストールする</span><span class="sxs-lookup"><span data-stu-id="8dd29-103">Install NGINX web server</span></span>

1. <span data-ttu-id="8dd29-104">使用する Linux 仮想マシンのパブリック IP アドレスを検索します。</span><span class="sxs-lookup"><span data-stu-id="8dd29-104">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="8dd29-105">この検索には、`vm list-ip-addresses` コマンドが使用できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8dd29-105">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="8dd29-106">次に、マシンのテストを行ったときと同様に、マシンへの `ssh` 接続を開きます。</span><span class="sxs-lookup"><span data-stu-id="8dd29-106">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="8dd29-107">管理者名 ("**aldis**") を渡す必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8dd29-107">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="8dd29-108">提示されたシェルで、次のコマンドを実行して `nginx` Web サーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8dd29-108">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="8dd29-109">Secure Shell を終了します。</span><span class="sxs-lookup"><span data-stu-id="8dd29-109">Exit the Secure Shell.</span></span>

```bash
exit
```

## <a name="retrieve-our-default-page"></a><span data-ttu-id="8dd29-110">既定のページを取得する</span><span class="sxs-lookup"><span data-stu-id="8dd29-110">Retrieve our default page</span></span>

1. <span data-ttu-id="8dd29-111">Azure Cloud Shell で、`curl` を使用してご利用の Linux Web サーバーから既定のページを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="8dd29-111">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="8dd29-112">あるいは、新しいブラウザー タブを開いて、パブリック IP アドレスを参照することもできます。</span><span class="sxs-lookup"><span data-stu-id="8dd29-112">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 40.83.165.85
```

<span data-ttu-id="8dd29-113">Linux 仮想マシンでは組み込みのファイアウォールを経由してポート 80 (`http`) が公開されないため、それは失敗します。</span><span class="sxs-lookup"><span data-stu-id="8dd29-113">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="8dd29-114">さいわい、Azure CLI にはそれ用のコマンドとして `vm open-port` があります。</span><span class="sxs-lookup"><span data-stu-id="8dd29-114">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="8dd29-115">Cloud Shell に次の内容を入力して、ポート 80 を開きます。</span><span class="sxs-lookup"><span data-stu-id="8dd29-115">Type the following into Cloud Shell to open port 80:</span></span>

```azurecli
az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

<span data-ttu-id="8dd29-116">ネットワーク ルールを追加し、ファイアウォールを介してポートを開く処理には、しばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="8dd29-116">It will take a moment to add the network rule and open the port through the firewall.</span></span> <span data-ttu-id="8dd29-117">もう一度 `curl` を試みてください。</span><span class="sxs-lookup"><span data-stu-id="8dd29-117">Try `curl` again.</span></span> <span data-ttu-id="8dd29-118">今回は、それによってデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="8dd29-118">This time it should return data.</span></span> <span data-ttu-id="8dd29-119">ブラウザーで該当するページを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="8dd29-119">You can see the page in a browser as well.</span></span>

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
