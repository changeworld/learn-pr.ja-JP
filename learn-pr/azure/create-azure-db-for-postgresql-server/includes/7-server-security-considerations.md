<span data-ttu-id="6c808-101">あなたはオンプレミスの PostgreSQL データベースを使っているものとしましょう。</span><span class="sxs-lookup"><span data-stu-id="6c808-101">Let's assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="6c808-102">セキュリティのあらゆる面を管理していて、標準的な PostgreSQL サーバー レベルのファイアウォール規則を使用してサーバーへのすべてのアクセスをロック ダウンしました。</span><span class="sxs-lookup"><span data-stu-id="6c808-102">You're managing all security aspects and you've locked down all access to your servers using the standard PostgreSQL server-level firewall rules.</span></span> <span data-ttu-id="6c808-103">今度は、Azure で同じサーバー レベルのファイアウォール規則を構成できることを確認しようと考えています。</span><span class="sxs-lookup"><span data-stu-id="6c808-103">Now you want to make sure that you can configure the same server-level firewall rules in Azure.</span></span>

## <a name="server-security-considerations-and-connection-methods"></a><span data-ttu-id="6c808-104">サーバーのセキュリティに関する考慮事項と接続方法</span><span class="sxs-lookup"><span data-stu-id="6c808-104">Server security considerations and connection methods</span></span>

<span data-ttu-id="6c808-105">Azure Database for PostgreSQL サーバーとデータベースへのアクセスを制限するには、多くのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6c808-105">You have a number of options to restrict access to your Azure Database for PostgreSQL server and databases.</span></span> <span data-ttu-id="6c808-106">ネットワーク アクセスは、ネットワーク、サーバー、またはデータベースのレベルで制限できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-106">Network access can be restricted at a network, server, or database level.</span></span> <span data-ttu-id="6c808-107">次のオプションをどれでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-107">You can use any of the following options:</span></span>

- <span data-ttu-id="6c808-108">データベース アクセスを制限するためのユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="6c808-108">User accounts to restrict database access</span></span>
- <span data-ttu-id="6c808-109">ネットワーク アクセスを制限するための仮想ネットワーク</span><span class="sxs-lookup"><span data-stu-id="6c808-109">Virtual networks to restrict network access</span></span>
- <span data-ttu-id="6c808-110">サーバー アクセスを制限するためのファイアウォール規則</span><span class="sxs-lookup"><span data-stu-id="6c808-110">Firewall rules to restrict server access</span></span>

### <a name="authentication-and-authorization"></a><span data-ttu-id="6c808-111">認証と認可</span><span class="sxs-lookup"><span data-stu-id="6c808-111">Authentication and authorization</span></span>

<span data-ttu-id="6c808-112">Azure Database for PostgreSQL サーバーは、ネイティブ PostgreSQL 認証をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="6c808-112">The Azure Database for PostgreSQL server supports native PostgreSQL authentication.</span></span> <span data-ttu-id="6c808-113">サーバーの管理者ログインを使用してサーバーに接続し、認証を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="6c808-113">You can connect and authenticate to the server with the server's admin login.</span></span> <span data-ttu-id="6c808-114">また、特定のデータベースに接続するためのユーザーを作成して、アクセスを制限することもあります。</span><span class="sxs-lookup"><span data-stu-id="6c808-114">You'll also create users to connect to specific databases to limit access.</span></span>

### <a name="what-is-a-virtual-network"></a><span data-ttu-id="6c808-115">仮想ネットワークとは</span><span class="sxs-lookup"><span data-stu-id="6c808-115">What is a virtual network?</span></span>

<span data-ttu-id="6c808-116">仮想ネットワークは、Azure ネットワーク内で作成された論理的に分離されたネットワークです。</span><span class="sxs-lookup"><span data-stu-id="6c808-116">A virtual network is a logically isolated network that's created within the Azure network.</span></span> <span data-ttu-id="6c808-117">仮想ネットワークを使用して、他のリソースに接続できる Azure リソースを制御できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-117">You can use a virtual network to control what Azure resources can connect to other resources.</span></span>

<span data-ttu-id="6c808-118">データベースに接続する Web アプリケーションを実行しているとします。</span><span class="sxs-lookup"><span data-stu-id="6c808-118">Imagine you're running a web application that connects to a database.</span></span> <span data-ttu-id="6c808-119">サブネットを使用して、ネットワークのさまざまな部分を分離します。</span><span class="sxs-lookup"><span data-stu-id="6c808-119">You'll use subnets to isolate different parts of the network.</span></span> <span data-ttu-id="6c808-120">サブネットは、IP アドレスの範囲に基づくネットワークの一部です。</span><span class="sxs-lookup"><span data-stu-id="6c808-120">A subnet is a part of a network that's based on a range of IP addresses.</span></span>

<span data-ttu-id="6c808-121">これらのサブネットを構成するには、仮想ネットワークを作成して、ネットワークをサブネットに再分割します。</span><span class="sxs-lookup"><span data-stu-id="6c808-121">To configure these subnets, you'll create a virtual network and then subdivide the network into subnets.</span></span> <span data-ttu-id="6c808-122">Web アプリケーションはあるサブネット上で動作し、データベースは別のサブネット上で動作します。</span><span class="sxs-lookup"><span data-stu-id="6c808-122">The web application will operate on one subnet and the database on another subnet.</span></span> <span data-ttu-id="6c808-123">各サブネットには、他のネットワークと通信するための独自の規則があります。</span><span class="sxs-lookup"><span data-stu-id="6c808-123">Each subnet will have its own rules for communicating to and from the other network.</span></span> <span data-ttu-id="6c808-124">これらの規則によって、データベースから Web アプリケーションへのアクセスを制限できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-124">These rules give you the ability to restrict access from the database to the web application.</span></span>

### <a name="what-is-a-firewall"></a><span data-ttu-id="6c808-125">ファイアウォールとは</span><span class="sxs-lookup"><span data-stu-id="6c808-125">What is a firewall?</span></span>

<span data-ttu-id="6c808-126">ファイアウォールとは、各要求の送信元 IP アドレスに基づいてサーバーへのアクセスを許可するサービスです。</span><span class="sxs-lookup"><span data-stu-id="6c808-126">A firewall is a service that grants server access based on the originating IP address of each request.</span></span> <span data-ttu-id="6c808-127">IP アドレスの範囲を指定するファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="6c808-127">You create firewall rules that specify ranges of IP addresses.</span></span> <span data-ttu-id="6c808-128">これらの許可された IP アドレスからのクライアントのみが、サーバーへのアクセスを許可されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-128">Only clients from these granted IP addresses will be allowed to access the server.</span></span> <span data-ttu-id="6c808-129">一般に、ファイアウォール規則には特定のネットワーク プロトコルとポート情報も含まれます。</span><span class="sxs-lookup"><span data-stu-id="6c808-129">Firewall rules, generally speaking, also include specific network protocol and port information.</span></span> <span data-ttu-id="6c808-130">たとえば、既定では、PostgreSQL サーバーはポート 5432 で TCP 要求をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="6c808-130">For example, a PostgreSQL server by default listens to TCP requests on port 5432.</span></span>

### <a name="azure-database-for-postgresql-server-firewall"></a><span data-ttu-id="6c808-131">Azure Database for PostgreSQL サーバーのファイアウォール</span><span class="sxs-lookup"><span data-stu-id="6c808-131">Azure Database for PostgreSQL server firewall</span></span>

<span data-ttu-id="6c808-132">Azure Database for PostgreSQL サーバーのファイアウォールは、ユーザーがアクセス許可を持つコンピューターを指定するまで、データベース サーバーへのすべてのアクセスを禁止します。</span><span class="sxs-lookup"><span data-stu-id="6c808-132">The Azure Database for PostgreSQL server firewall prevents all access to your database server until you specify which computers have permission.</span></span> <span data-ttu-id="6c808-133">ファイアウォールの構成を使用して、サーバーへの接続を許可する IP アドレスの範囲を指定できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-133">The firewall configuration allows you to specify a range of IP addresses that are allowed to connect to the server.</span></span> <span data-ttu-id="6c808-134">サーバーは常に、PostgreSQL の既定の接続情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="6c808-134">The server always uses the default PostgreSQL connection information.</span></span>

![受信したすべての要求の IP アドレスをスキャンする Azure Database for PostgreSQL サーバーのファイアウォールを示す図。](../media/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a><span data-ttu-id="6c808-137">Azure Database for PostgreSQL サーバーの SSL 接続</span><span class="sxs-lookup"><span data-stu-id="6c808-137">Azure Database for PostgreSQL server SSL connections</span></span>

<span data-ttu-id="6c808-138">Azure Database for PostgreSQL では、クライアント アプリケーションが PostgreSQL サービスに接続するときに、Secure Sockets Layer (SSL) を使用することが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-138">Azure Database for PostgreSQL prefers that your client applications connect to the PostgreSQL service using the Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="6c808-139">データベース サーバーとクライアント アプリケーション間に SSL 接続を適用すると、サーバーとクライアント間のデータが暗号化されて、"中間者攻撃" などの攻撃から保護されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-139">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" and similar attacks by encrypting the data between the server and client.</span></span> <span data-ttu-id="6c808-140">SSL を有効にすると、クライアントとサーバー間の接続が機能するためには、キーの交換と厳密な認証が必要になります。</span><span class="sxs-lookup"><span data-stu-id="6c808-140">Enabling SSL requires the exchange of keys and strict authentication between client and server for the connection to work.</span></span> <span data-ttu-id="6c808-141">SSL の使用については、この学習モジュールでは詳しく説明しません。</span><span class="sxs-lookup"><span data-stu-id="6c808-141">Details about using SSL are beyond the scope of this learning module.</span></span>

## <a name="configure-connection-security"></a><span data-ttu-id="6c808-142">接続のセキュリティを構成する</span><span class="sxs-lookup"><span data-stu-id="6c808-142">Configure connection security</span></span>

<span data-ttu-id="6c808-143">Azure Database for PostgreSQL サーバーのファイアウォールを構成するときの決定事項と手順を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="6c808-143">Let's look at the decisions and steps you make to configure an Azure Database for PostgreSQL server firewall.</span></span> <span data-ttu-id="6c808-144">前に作成したサーバーに接続する方法も確認します。</span><span class="sxs-lookup"><span data-stu-id="6c808-144">You'll also see how to connect to the server that you created earlier.</span></span>

<span data-ttu-id="6c808-145">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="6c808-145">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span> <span data-ttu-id="6c808-146">ファイアウォール規則を作成する対象のサーバー リソースに移動します。</span><span class="sxs-lookup"><span data-stu-id="6c808-146">Navigate to the server resource for which you'd like to create a firewall rule.</span></span>

<span data-ttu-id="6c808-147">次に、**[接続のセキュリティ]** オプションを選択して、右側に接続のセキュリティ ブレードを開きます。</span><span class="sxs-lookup"><span data-stu-id="6c808-147">Then, you'll select the **Connection Security** option to open the connection security blade to the right.</span></span>

![PostgreSQL データベース リソース ブレードの [接続のセキュリティ] セクションを示す Azure portal のスクリーン ショット](../media/7-db-security-settings.png)

<span data-ttu-id="6c808-149">この画面には、いくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6c808-149">On this screen, you have several options.</span></span> <span data-ttu-id="6c808-150">次のことが行えます。</span><span class="sxs-lookup"><span data-stu-id="6c808-150">You can:</span></span>

- <span data-ttu-id="6c808-151">**[クライアント IP の追加]** ボタンをクリックして、ポータルへのアクセスに使用する IP アドレスをファイアウォールのエントリとして追加します。</span><span class="sxs-lookup"><span data-stu-id="6c808-151">Add the IP address that you use to access the portal as a firewall entry by clicking on the **Add client IP** button.</span></span>
- <span data-ttu-id="6c808-152">Azure サービスへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="6c808-152">Allow access to Azure services.</span></span> <span data-ttu-id="6c808-153">既定では、すべての Azure サービスは PostgreSQL サーバーにアクセス**できません**。</span><span class="sxs-lookup"><span data-stu-id="6c808-153">By default, all Azure services **don't** have access to the PostgreSQL server.</span></span>
- <span data-ttu-id="6c808-154">IP アドレスの範囲を入力して、ファイアウォール規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="6c808-154">Add firewall rules by entering ranges of IP addresses.</span></span>
- <span data-ttu-id="6c808-155">SSL 接続を適用します。</span><span class="sxs-lookup"><span data-stu-id="6c808-155">Enforce SSL connections.</span></span> <span data-ttu-id="6c808-156">このオプションを指定すると、クライアントは SSL 証明書を使用してサーバーに接続することを強制されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-156">This option forces your client to connect to the server using an SSL certificate.</span></span>

<span data-ttu-id="6c808-157">変更を行ったら、忘れずに入力フィールドの上の **[保存]** アイコンをクリックして更新された構成を保存してください。</span><span class="sxs-lookup"><span data-stu-id="6c808-157">Always remember to click on the **Save** icon above the entry fields to save the updated configuration after you've made changes.</span></span>

### <a name="allow-access-to-azure-services"></a><span data-ttu-id="6c808-158">Azure サービスへのアクセスを許可する</span><span class="sxs-lookup"><span data-stu-id="6c808-158">Allow access to Azure services</span></span>

<span data-ttu-id="6c808-159">Azure Cloud Shell を使用してサーバーにアクセスしたりサーバーを構成したりするには、**[Azure サービスへのアクセスを許可]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6c808-159">To use Azure Cloud Shell to access or configure your server, make sure to enable **Allow Access to Azure Services**.</span></span> <span data-ttu-id="6c808-160">この手順では、Cloud Shell からのアクセスを許可するように、サーバーの構成にファイアウォール規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="6c808-160">This step is going to add a firewall rule to the server configuration to allow access from Cloud Shell.</span></span> <span data-ttu-id="6c808-161">この規則は、ユーザーが追加するカスタム規則の 1 つとして表示されることはありません。</span><span class="sxs-lookup"><span data-stu-id="6c808-161">This rule won't show as one of the custom rules that you add.</span></span>

<span data-ttu-id="6c808-162">**[SSL 接続を強制する]** を無効にする必要もあります。</span><span class="sxs-lookup"><span data-stu-id="6c808-162">You also need to disable **Enforce SSL connection**.</span></span> <span data-ttu-id="6c808-163">クライアント接続に SSL が必要な場合、PowerShell はサーバーに接続できません。</span><span class="sxs-lookup"><span data-stu-id="6c808-163">PowerShell can't connect to the server if SSL is required for client connections.</span></span>

<span data-ttu-id="6c808-164">これらのオプションはどちらも、正しく構成しないと、コマンド ラインにエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-164">Both of these options will result in an error message that's displayed on the command line if not configured correctly.</span></span>

<span data-ttu-id="6c808-165">たとえば、Azure サービスへのアクセスを許可しないで、SSL 接続の強制を有効にした場合、ファイアウォールがアクセスをブロックすると次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c808-165">For example, if access is not allowed to Azure services and enforce SSL connections is enabled, then you'll see something similar to this error when the firewall is blocking access:</span></span>

```output
psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL:  SSL connection is required. Please specify SSL options and retry.
```

### <a name="create-a-firewall-rule-using-the-portal"></a><span data-ttu-id="6c808-166">ポータルを使用してファイアウォール規則を作成する</span><span class="sxs-lookup"><span data-stu-id="6c808-166">Create a firewall rule using the portal</span></span>

<span data-ttu-id="6c808-167">たとえば、任意の IP アドレスからのアクセスを許可するファイアウォール規則を作成するとします。</span><span class="sxs-lookup"><span data-stu-id="6c808-167">Let's say you want to create a firewall rule that provides access from any IP address.</span></span>

> [!WARNING]
> <span data-ttu-id="6c808-168">このファイアウォール規則を作成すると、インターネット上のすべての IP アドレスがサーバーへの接続を試みることができます。</span><span class="sxs-lookup"><span data-stu-id="6c808-168">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="6c808-169">クライアントがユーザー名とパスワードなしではサーバーにアクセスできない場合でも、セキュリティへの影響を確実に理解したうえで、慎重にこの規則を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6c808-169">Even though clients won't be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications.</span></span>

<span data-ttu-id="6c808-170">それぞれのラベルの付いたフィールドに次のデータを入力して、新しいファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="6c808-170">You create a new firewall rule by entering the following data in the labeled fields:</span></span>

- <span data-ttu-id="6c808-171">規則名: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="6c808-171">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="6c808-172">開始 IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="6c808-172">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="6c808-173">終了 IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="6c808-173">End IP: `255.255.255.255`</span></span>

<span data-ttu-id="6c808-174">ファイアウォール規則を削除するには、削除するルールの末尾にある省略記号 (...) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c808-174">To remove a firewall rule, you'll click the ellipsis (...) at the end of the rule that you want to delete.</span></span> <span data-ttu-id="6c808-175">**[削除]** ボタンをクリックして、規則を削除します。</span><span class="sxs-lookup"><span data-stu-id="6c808-175">Click the **Delete** button to delete the rule.</span></span>

<span data-ttu-id="6c808-176">入力フィールドの上にある **[保存]** アイコンをクリックして、規則の削除をコミットします。</span><span class="sxs-lookup"><span data-stu-id="6c808-176">Click on the **Save** icon above the entry fields to commit the deletion of the rule.</span></span>

### <a name="create-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="6c808-177">Azure CLI を使用してファイアウォール規則を作成する</span><span class="sxs-lookup"><span data-stu-id="6c808-177">Create a firewall rule using the Azure CLI</span></span>

<span data-ttu-id="6c808-178">Azure CLI で `az postgres server firewall-rule create` コマンドを使用して、ファイアウォール規則をサーバーに追加できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-178">You can use the Azure CLI to add firewall rules to your server with the `az postgres server firewall-rule create` command.</span></span> <span data-ttu-id="6c808-179">上記から規則を作成する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="6c808-179">Here's an example that creates the rule from above.</span></span>

```azurecli
az postgres server firewall-rule create \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --server <server-name> \
  --name AllowAll \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 255.255.255.255
```

<span data-ttu-id="6c808-180">サーバーからファイアウォール規則を削除するには、`az postgres server firewall-rule delete` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6c808-180">You remove firewall rules from your server with the command `az postgres server firewall-rule delete`.</span></span> <span data-ttu-id="6c808-181">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="6c808-181">Here's an example:</span></span>

```azurecli
az postgres server firewall-rule delete \
  --name AllowAll \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --server-name <server-name>
```

## <a name="connecting-to-your-server"></a><span data-ttu-id="6c808-182">サーバーへの接続</span><span class="sxs-lookup"><span data-stu-id="6c808-182">Connecting to your server</span></span>

<span data-ttu-id="6c808-183">他の最新のデータベースと同じように、PostgreSQL でも最適なパフォーマンスを実現するには通常のサーバー管理が必要です。</span><span class="sxs-lookup"><span data-stu-id="6c808-183">Like any modern database, PostgreSQL requires regular server administration to achieve best performance.</span></span> <span data-ttu-id="6c808-184">Azure Database for PostgreSQL サーバーに接続して管理するには、多くのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6c808-184">You have a number of options to connect and manage your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="6c808-185">ここでは、`psql` を使用してサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="6c808-185">We'll use `psql` to connect to the server.</span></span>

### <a name="what-is-psql"></a><span data-ttu-id="6c808-186">psql とは</span><span class="sxs-lookup"><span data-stu-id="6c808-186">What is psql?</span></span>

<span data-ttu-id="6c808-187">`psql` という名前のコマンド ライン ツールは、PostgreSQL サーバーとデータベースを操作するための PostgreSQL の分散対話型ターミナルです。</span><span class="sxs-lookup"><span data-stu-id="6c808-187">The command-line tool called `psql` is the PostgreSQL distributed interactive terminal for working with PostgreSQL servers and databases.</span></span> <span data-ttu-id="6c808-188">`psql` は他の PostgreSQL 実装と同じように Azure Database for PostgreSQL で動作し、Azure Cloud Shell に付属します。</span><span class="sxs-lookup"><span data-stu-id="6c808-188">`psql` works with Azure Database for PostgreSQL the same as with any other PostgreSQL implementation and is included with Azure Cloud Shell.</span></span> <span data-ttu-id="6c808-189">`psql` ツールでは、データベースを管理できるだけでなく、これらのデータベースに対してクエリを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="6c808-189">The `psql` tool allows you to manage databases as well as execute structure queries against these databases.</span></span>

<span data-ttu-id="6c808-190">`psql` を使うには、PostgreSQL サーバーに正常に接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6c808-190">Using `psql` requires a successful connection to a PostgreSQL server.</span></span> <span data-ttu-id="6c808-191">`psql` では多数のコマンド ライン パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-191">There are a number of command-line parameters available for use when working with `psql`.</span></span>

- <span data-ttu-id="6c808-192">`--host` - 接続先のホスト。</span><span class="sxs-lookup"><span data-stu-id="6c808-192">`--host` - The host to which you'd like to connect.</span></span>
- <span data-ttu-id="6c808-193">`--username` - 接続するためのユーザー名/ID。</span><span class="sxs-lookup"><span data-stu-id="6c808-193">`--username` - The user name/ID with which to connect.</span></span>
- <span data-ttu-id="6c808-194">`--dbname` - 接続先のデータベースの名前。</span><span class="sxs-lookup"><span data-stu-id="6c808-194">`--dbname` - The name of the database to connect to.</span></span>

> [!TIP]
> <span data-ttu-id="6c808-195">サーバー アクセスとデータベース構成を管理するときは、通常、`postgres` 管理データベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="6c808-195">You'll typically connect to the `postgres` management database when managing your server access and database configurations.</span></span>

<span data-ttu-id="6c808-196">完全なコマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="6c808-196">Here is the complete command:</span></span>

```bash
psql --host=<server-name>.postgres.database.azure.com
      --username=<admin-user>@<server-name>
      --dbname=<database>
```

<span data-ttu-id="6c808-197">接続されると、コマンド プロンプトが表示され、サーバーとデータベースに対してコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="6c808-197">After you're connected, you'll be presented with a command prompt and can execute commands to your server and databases.</span></span>

<span data-ttu-id="6c808-198">Azure Database for PostgreSQL のセキュリティ設定を構成する手順を見てきました。</span><span class="sxs-lookup"><span data-stu-id="6c808-198">You've now seen the steps that you take to configure Azure Database for PostgreSQL security settings.</span></span> <span data-ttu-id="6c808-199">次のユニットでは、Azure Database for PostgreSQL のセキュリティ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="6c808-199">In the next unit, you'll configure Azure Database for PostgreSQL security settings.</span></span> <span data-ttu-id="6c808-200">また、Cloud Shell を使用してサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="6c808-200">You'll also connect to the server using Cloud Shell.</span></span>
