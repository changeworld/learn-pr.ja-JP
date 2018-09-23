<span data-ttu-id="6e32d-101">あなたは、オンプレミスの PostgreSQL データベースを使用しているとします。</span><span class="sxs-lookup"><span data-stu-id="6e32d-101">Let's assume that you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="6e32d-102">セキュリティのあらゆる面を管理していて、標準的な PostgreSQL サーバー レベルのファイアウォール規則を使用してサーバーへのすべてのアクセスをロック ダウンしました。</span><span class="sxs-lookup"><span data-stu-id="6e32d-102">You're managing all security aspects and you've locked down all access to your servers using the standard PostgreSQL server-level firewall rules.</span></span> <span data-ttu-id="6e32d-103">ここまでの説明で、Azure で同一のサーバー レベルのファイアウォール規則を構成する方法について理解を深めることができました。</span><span class="sxs-lookup"><span data-stu-id="6e32d-103">You now have a good understanding of how to configure the same server-level firewall rules in Azure.</span></span>

<span data-ttu-id="6e32d-104">作成した Azure Database for PostgreSQL サーバーのいずれかに接続しましょう。</span><span class="sxs-lookup"><span data-stu-id="6e32d-104">Let's connect to one of the Azure Database for PostgreSQL servers that you created.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="6e32d-105">Azure サービスへのアクセス許可</span><span class="sxs-lookup"><span data-stu-id="6e32d-105">Allow Azure service access</span></span>

<span data-ttu-id="6e32d-106">始める前に、サーバーに接続するのに PowerShell および `psql` を使用する場合は、Azure サービスへのアクセスを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e32d-106">Before we begin, you'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="6e32d-107">アクセスを許可するには 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="6e32d-107">Recall that you can allow access in two ways.</span></span>

<span data-ttu-id="6e32d-108">1 つ目の方法では、**[Azure サービスへのアクセスを許可]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="6e32d-108">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="6e32d-109">アクセスを許可すると、作成したカスタム規則の一覧に規則が入力されていない場合でも、ファイアウォール規則が作成されます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-109">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="6e32d-110">2 つ目の方法では、すべての IP アドレスへのアクセスを許可するファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-110">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="6e32d-111">この規則には、`psql` の実行に使用する、PowerShell を実行しているクライアントの IP アドレスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-111">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="6e32d-112">**[SSL 接続を強制する]** オプションを無効にする必要もあります。</span><span class="sxs-lookup"><span data-stu-id="6e32d-112">You also need to disable the **Enforce SSL connection** option.</span></span>

### <a name="create-a-firewall-rule"></a><span data-ttu-id="6e32d-113">ファイアウォール規則を作成する</span><span class="sxs-lookup"><span data-stu-id="6e32d-113">Create a firewall rule</span></span>

1. <span data-ttu-id="6e32d-114">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="6e32d-114">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span> 

1. <span data-ttu-id="6e32d-115">ファイアウォール規則を作成する対象のサーバー リソースに移動します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-115">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

1. <span data-ttu-id="6e32d-116">**[接続のセキュリティ]** オプションを選択して、右側の接続のセキュリティ ブレードを開きます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-116">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

    ![PostgreSQL データベース リソース ブレードの [接続のセキュリティ] セクションを示す Azure portal のスクリーン ショット](../media/7-db-security-settings.png)

<span data-ttu-id="6e32d-118">`psql` を実行している PowerShell クライアントへのアクセスを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e32d-118">Recall that you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="6e32d-119">次のいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-119">You can choose to either:</span></span>

- <span data-ttu-id="6e32d-120">**[Azure サービスへのアクセスを許可]** を **[オン]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-120">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="6e32d-121">**[SSL 接続を強制する]** を **[無効]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-121">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="6e32d-122">**[保存]** をクリックして、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-122">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="6e32d-123">または、すべての IP アドレスへのアクセスを許可するファイアウォール規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-123">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="6e32d-124">次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-124">Use the following values:</span></span>

- <span data-ttu-id="6e32d-125">規則名: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="6e32d-125">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="6e32d-126">開始 IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="6e32d-126">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="6e32d-127">終了 IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="6e32d-127">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="6e32d-128">**[SSL 接続を強制する]** を **[無効]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-128">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="6e32d-129">**[保存]** をクリックして、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-129">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="6e32d-130">このファイアウォール規則を作成すると、インターネット上のすべての IP アドレスがサーバーへの接続を試みることができます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-130">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="6e32d-131">運用環境では、アクセスを特定の IP アドレスに限定します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-131">In production environments, you'll want to restrict access to specific IP addresses.</span></span>

### <a name="connect-to-the-database-with-psql"></a><span data-ttu-id="6e32d-132">psql を使用してデータベースに接続する</span><span class="sxs-lookup"><span data-stu-id="6e32d-132">Connect to the database with psql</span></span>

1. <span data-ttu-id="6e32d-133">右側の Azure Cloud Shell で、次のコマンドを使用して PSQL をご自分のサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-133">In the Azure Cloud Shell on the right, connect PSQL to your server using the following command.</span></span> <span data-ttu-id="6e32d-134">サーバー名と管理者名は必ず置換してください。</span><span class="sxs-lookup"><span data-stu-id="6e32d-134">Make sure to replace the server name and admin name.</span></span>

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```
    
    <span data-ttu-id="6e32d-135">`server-name` および `admin-user` に選択した値を使用します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-135">Use the values you chose for the `server-name`, and `admin-user`.</span></span> 

1. <span data-ttu-id="6e32d-136">**postgres** は、すべての PostgreSQL サーバーの作成に使用される既定の管理データベースです。</span><span class="sxs-lookup"><span data-stu-id="6e32d-136">**postgres** is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="6e32d-137">サーバーの作成時に指定したパスワードを入力するよう求めるプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-137">You'll be prompted for the password you provided when you created the server.</span></span>

1. <span data-ttu-id="6e32d-138">正常に接続されたら、<kbd>\l</kbd> コマンドを実行してすべてのデータベースを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-138">Once successfully connected, execute the <kbd>\l</kbd> command to list all databases.</span></span> <span data-ttu-id="6e32d-139">このコマンドにより、2 つ以上の既定のデータベースが返されます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-139">This command will result in two or more default databases returned.</span></span>

1. <span data-ttu-id="6e32d-140"><kbd>q</kbd> キーを押してリストを終了します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-140">Hit <kbd>q</kbd> to exit the list.</span></span>

1. <span data-ttu-id="6e32d-141">他の PSQL コマンドを試すことができます。</span><span class="sxs-lookup"><span data-stu-id="6e32d-141">You can try other PSQL commands.</span></span>
    - <kbd>\?</kbd> <span data-ttu-id="6e32d-142">ヘルプを表示します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-142">to get help.</span></span>
    - <span data-ttu-id="6e32d-143"><kbd>\dt</kbd>: テーブルを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-143"><kbd>\dt</kbd> to list the tables.</span></span>

1. <span data-ttu-id="6e32d-144">サーバー上で PSQL 演算の実行が完了したら、コマンド <kbd>\q</kbd> を実行して PSQL を終了します。</span><span class="sxs-lookup"><span data-stu-id="6e32d-144">When you're finished executing PSQL operations on your server, execute the command <kbd>\q</kbd> to quit PSQL.</span></span>
