<span data-ttu-id="c5e68-101">オンプレミスの PostgreSQL データベースを使用しているとします。</span><span class="sxs-lookup"><span data-stu-id="c5e68-101">Lets' assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="c5e68-102">セキュリティのあらゆる面を管理していて、標準的な PostgreSQL サーバー レベルのファイアウォール規則を使用してサーバーへのすべてのアクセスをロック ダウンしました。</span><span class="sxs-lookup"><span data-stu-id="c5e68-102">You're managing all security aspects and locked down all access to your servers using the standard PostgreSQL server level firewall rules.</span></span> <span data-ttu-id="c5e68-103">ここまでの説明で、Azure で同一のサーバー レベルのファイアウォール規則を構成する方法について理解を深めることができました。</span><span class="sxs-lookup"><span data-stu-id="c5e68-103">You now have a good understanding of how to configure the same server level firewall rules in Azure.</span></span>

<span data-ttu-id="c5e68-104">ここでは、以前に `psql` を使用して作成した Azure Database for PostgreSQL サーバーの 1 つに接続してみます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-104">You now have the chance to connect to one of the previous Azure Databases for PostgreSQL servers you created using `psql`.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="c5e68-105">Azure サービスへのアクセス許可</span><span class="sxs-lookup"><span data-stu-id="c5e68-105">Allow Azure service access</span></span>

<span data-ttu-id="c5e68-106">作業を開始する前に、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="c5e68-106">Before we begin.</span></span> <span data-ttu-id="c5e68-107">サーバーに接続するのに PowerShell および `psql` を使用する場合は、Azure サービスへのアクセスを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5e68-107">You'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="c5e68-108">アクセスを許可するには 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="c5e68-108">Recall, that you can allow access in two ways.</span></span>

<span data-ttu-id="c5e68-109">1 つ目の方法では、**[Azure サービスへのアクセスを許可]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="c5e68-109">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="c5e68-110">アクセスを許可すると、作成したカスタム規則の一覧に規則が入力されていない場合でも、ファイアウォール規則が作成されます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-110">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="c5e68-111">2 つ目の方法では、すべての IP アドレスへのアクセスを許可するファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-111">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="c5e68-112">この規則には、`psql` の実行に使用する、PowerShell を実行しているクライアントの IP アドレスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-112">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="c5e68-113">**[SSL 接続を強制する]** を無効にする必要もあります。</span><span class="sxs-lookup"><span data-stu-id="c5e68-113">You also need to disable the **Enforce SSL connection**.</span></span>

<span data-ttu-id="c5e68-114">早速始めましょう。</span><span class="sxs-lookup"><span data-stu-id="c5e68-114">Let's begin.</span></span>

<span data-ttu-id="c5e68-115">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="c5e68-115">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span> <span data-ttu-id="c5e68-116">ファイアウォール規則を作成する対象のサーバー リソースに移動します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-116">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

<span data-ttu-id="c5e68-117">**[接続のセキュリティ]** オプションを選択して、右側の接続のセキュリティ ブレードを開きます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-117">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

![PostgreSQL データベースのセキュリティ設定](../media-draft/7-db-security-settings.png)

<span data-ttu-id="c5e68-119">`psql` を実行している PowerShell クライアントへのアクセスを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c5e68-119">Recall, you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="c5e68-120">次のいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-120">You can choose to either:</span></span>

- <span data-ttu-id="c5e68-121">**[Azure サービスへのアクセスを許可]** を **[オン]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-121">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="c5e68-122">**[SSL 接続を強制する]** を **[無効]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-122">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="c5e68-123">**[保存]** をクリックして、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-123">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="c5e68-124">または、すべての IP アドレスへのアクセスを許可するファイアウォール規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-124">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="c5e68-125">次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-125">Use the following values:</span></span>

- <span data-ttu-id="c5e68-126">規則名: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="c5e68-126">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="c5e68-127">開始 IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="c5e68-127">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="c5e68-128">終了 IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="c5e68-128">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="c5e68-129">**[SSL 接続を強制する]** を **[無効]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-129">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="c5e68-130">**[保存]** をクリックして、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-130">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="c5e68-131">このファイアウォール規則を作成すると、インターネット上のすべての IP アドレスがサーバーへの接続を試みることができます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-131">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="c5e68-132">クライアントがユーザー名とパスワードなしではサーバーにアクセスできない場合でも、セキュリティへの影響を確実に理解したうえで、慎重にこの規則を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c5e68-132">Even though clients will not be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications.</span></span> <span data-ttu-id="c5e68-133">運用環境では、特定のクライアント IP アドレスへのアクセスのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-133">In production environments, you'll only allow access to specific client IP addresses.</span></span>

<span data-ttu-id="c5e68-134">次の手順では、Azure Cloud Shell セッションを開始します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-134">For the next steps, you'll start an Azure Cloud Shell session.</span></span> <span data-ttu-id="c5e68-135">このラボでは、コマンドライン環境として `bash` を使用します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-135">This lab uses `bash` as the command-line environment.</span></span>

- <span data-ttu-id="c5e68-136">Azure portal から Cloud Shell を開きます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-136">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="c5e68-137">[Azure portal](https://portal.azure.com?azure-portal=true) に移動し、[Open Cloud Shell]\(Cloud Shell を開く\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c5e68-137">Go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

  ![Cloud Shell ボタン](../media-draft/cloud-shell-button.png)

- <span data-ttu-id="c5e68-139">複数のサブスクリプションがある場合は、適切なサブスクリプションを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-139">In case you have several subscriptions, check to make sure you're using the correct subscription when asked.</span></span> <span data-ttu-id="c5e68-140">次のコマンドを実行して、アクティブなサブスクリプションを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-140">You can also run the following command to set the active subscription.</span></span> <span data-ttu-id="c5e68-141">0 をサブスクリプション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-141">Remember to replace the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- <span data-ttu-id="c5e68-142">次のコマンドを使用して、psql をサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-142">Connect psql to your server using the following command:</span></span>

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   <span data-ttu-id="c5e68-143">`server-name` および `admin-user` は、サーバーの作成時に管理者アカウントに対して選択した値です。</span><span class="sxs-lookup"><span data-stu-id="c5e68-143">Recall, `server-name`, and `admin-user` are the values you chose for the administrator account when you created the server.</span></span> <span data-ttu-id="c5e68-144">`postgres` は、すべての PostgreSQL サーバーの作成に使用される既定の管理データベースです。</span><span class="sxs-lookup"><span data-stu-id="c5e68-144">`postgres` is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="c5e68-145">サーバーの作成時に指定したパスワードを入力するよう求めるプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-145">You'll be prompted for the password you provided when you created the server.</span></span>

- <span data-ttu-id="c5e68-146">正常に接続されたら、`\l` コマンドを実行してすべてのデータベースを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-146">Once successfully connected, execute the `\l` command to list all databases.</span></span>

   <span data-ttu-id="c5e68-147">このコマンドにより、2 つ以上の既定のデータベースが返されます。</span><span class="sxs-lookup"><span data-stu-id="c5e68-147">This command will result in two or more default databases returned from.</span></span>

- <span data-ttu-id="c5e68-148">サーバー上で psql 演算の実行が完了したら、コマンド `\q` を実行して `psql` を終了します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-148">When you're finished executing psql operations on your server, execute the command `\q` to quit `psql`.</span></span>

- <span data-ttu-id="c5e68-149">最後に、Cloud Shell を終了するには、`exit` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c5e68-149">Finally, to exit Cloud Shell, execute the command `exit`.</span></span>
