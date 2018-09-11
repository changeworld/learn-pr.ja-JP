<span data-ttu-id="5a9e2-101">VM 上で最後に試してみたいのは、Web サーバーをインストールすることです。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="5a9e2-102">インストールするのが最も簡単なパッケージの 1 つとして、`nginx` があります。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-102">One of the easiest packages to install is `nginx`.</span></span>

1. <span data-ttu-id="5a9e2-103">使用する Linux 仮想マシンのパブリック IP アドレスを検索します。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-103">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="5a9e2-104">この検索には、`vm list-ip-addresses` コマンドが使用できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-104">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="5a9e2-105">次に、マシンのテストを行ったときと同様に、マシンへの `ssh` 接続を開きます。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-105">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="5a9e2-106">管理者名 ("**aldis**") を渡す必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-106">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="5a9e2-107">提示されたシェルで、次のコマンドを実行して `nginx` Web サーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-107">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="5a9e2-108">Azure Cloud Shell で、`curl` を使用してご利用の Linux Web サーバーから既定のページを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-108">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="5a9e2-109">あるいは、新しいブラウザー タブを開いて、パブリック IP アドレスを参照することもできます。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-109">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 168.61.54.62
```

<span data-ttu-id="5a9e2-110">Linux 仮想マシンでは組み込みのファイアウォールを経由してポート 80 (`http`) が公開されないため、それは失敗します。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-110">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="5a9e2-111">さいわい、Azure CLI にはそれ用のコマンドとして `vm open-port` があります。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-111">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="5a9e2-112">Cloud Shell に次の内容を入力して、ポート 80 を開きます。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-112">Type the following into Cloud Shell to open port 80:</span></span>

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

<span data-ttu-id="5a9e2-113">最後に、`curl` をもう一度お試しください。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-113">Finally, try `curl` again.</span></span> <span data-ttu-id="5a9e2-114">今回は、それによってデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-114">This time it should return data.</span></span> <span data-ttu-id="5a9e2-115">ブラウザーで該当するページを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="5a9e2-115">You can see the page in a browser as well.</span></span>
