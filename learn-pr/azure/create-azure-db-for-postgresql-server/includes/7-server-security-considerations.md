<span data-ttu-id="97de9-101">オンプレミスの PostgreSQL データベースを使用しているものとします。</span><span class="sxs-lookup"><span data-stu-id="97de9-101">Lets' assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="97de9-102">セキュリティのあらゆる面を管理しており、標準的な PostgreSQL サーバー レベルのファイアウォール規則を使用してサーバーへのすべてのアクセスをロック ダウンしました。</span><span class="sxs-lookup"><span data-stu-id="97de9-102">You're managing all security aspects and locked down all access to your servers using the standard PostgreSQL server level firewall rules.</span></span> <span data-ttu-id="97de9-103">今度は、Azure で同じサーバー レベルのファイアウォール規則を構成できることを確認しようと考えています。</span><span class="sxs-lookup"><span data-stu-id="97de9-103">You now want to make sure that you can configure the same server level firewall rules in Azure.</span></span>

## <a name="server-security-considerations-and-connection-methods"></a><span data-ttu-id="97de9-104">サーバーのセキュリティに関する考慮事項と接続方法</span><span class="sxs-lookup"><span data-stu-id="97de9-104">Server Security Considerations and Connection Methods</span></span>

<span data-ttu-id="97de9-105">Azure Database for PostgreSQL サーバーとデータベースへのアクセスを制限するには、多くのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="97de9-105">You have a number of options to restrict access to your Azure Database for PostgreSQL server and databases.</span></span> <span data-ttu-id="97de9-106">ネットワーク アクセスは、ネットワーク、サーバー、またはデータベースのレベルで制限できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-106">Network access can be restricted at a network, server, or database level.</span></span> <span data-ttu-id="97de9-107">次のオプションをどれでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-107">You can use any of the following options:</span></span>

- <span data-ttu-id="97de9-108">データベース アクセスを制限するためのユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="97de9-108">User accounts to restrict database access</span></span>
- <span data-ttu-id="97de9-109">ネットワーク アクセスを制限するための仮想ネットワーク</span><span class="sxs-lookup"><span data-stu-id="97de9-109">Virtual networks to restrict network access</span></span>
- <span data-ttu-id="97de9-110">サーバー アクセスを制限するためのファイアウォール規則</span><span class="sxs-lookup"><span data-stu-id="97de9-110">Firewall rules to restrict server access</span></span>

### <a name="authentication-and-authorization"></a><span data-ttu-id="97de9-111">認証と認可</span><span class="sxs-lookup"><span data-stu-id="97de9-111">Authentication and authorization</span></span>

<span data-ttu-id="97de9-112">Azure Database for PostgreSQL サーバーは、ネイティブ PostgreSQL 認証をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="97de9-112">Azure Database for PostgreSQL server supports native PostgreSQL authentication.</span></span> <span data-ttu-id="97de9-113">サーバーの管理者ログインを使用してサーバーに接続し、認証を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="97de9-113">You can connect and authenticate to server with the server's admin login.</span></span> <span data-ttu-id="97de9-114">また、特定のデータベースに接続するためのユーザーを作成して、アクセスを制限することもあります。</span><span class="sxs-lookup"><span data-stu-id="97de9-114">You'll also create users to connect to specific databases to limit access.</span></span>

### <a name="what-is-a-virtual-network"></a><span data-ttu-id="97de9-115">仮想ネットワークとは</span><span class="sxs-lookup"><span data-stu-id="97de9-115">What is a Virtual Network?</span></span>

<span data-ttu-id="97de9-116">仮想ネットワークとは、Azure ネットワーク内に作成された、論理的に分離されているネットワークです。</span><span class="sxs-lookup"><span data-stu-id="97de9-116">A virtual network is a logically isolated network created within the Azure network.</span></span> <span data-ttu-id="97de9-117">仮想ネットワークを使用して、他のリソースに接続できる Azure リソースを制御できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-117">You can use a virtual network to control what Azure resources can connect to other resources.</span></span>

<span data-ttu-id="97de9-118">データベースに接続する Web アプリケーションを実行しているものとします。</span><span class="sxs-lookup"><span data-stu-id="97de9-118">Imagine you're running a web application that connects to a database.</span></span> <span data-ttu-id="97de9-119">サブネットを使用して、ネットワークのさまざまな部分を分離します。</span><span class="sxs-lookup"><span data-stu-id="97de9-119">You'll use subnets to isolate different parts of the network.</span></span> <span data-ttu-id="97de9-120">サブネットとは、IP アドレスの範囲に基づくネットワークの一部分です。</span><span class="sxs-lookup"><span data-stu-id="97de9-120">A subnet is a part of a network based upon a range of IP addresses.</span></span>

<span data-ttu-id="97de9-121">これらのサブネットを構成するには、仮想ネットワークを作成して、ネットワークをサブネットに再分割します。</span><span class="sxs-lookup"><span data-stu-id="97de9-121">To configure these subnets, you'll create a virtual network and then subdivide the network into subnets.</span></span> <span data-ttu-id="97de9-122">Web アプリケーションはあるサブネット上で動作し、データベースは別のサブネット上で動作します。</span><span class="sxs-lookup"><span data-stu-id="97de9-122">The web application will operate on one subnet and the database on another subnet.</span></span> <span data-ttu-id="97de9-123">各サブネットには、他のネットワークと双方向に通信するための独自の規則があります。</span><span class="sxs-lookup"><span data-stu-id="97de9-123">Each subnet would have its own rules for communicating to and from the other network.</span></span> <span data-ttu-id="97de9-124">これらの規則によって、データベースから Web アプリケーションへのアクセスを制限できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-124">These rules give you the ability to restrict access from the database to the web application.</span></span>

<span data-ttu-id="97de9-125">仮想ネットワークの作成はこのモジュールの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="97de9-125">Creating a virtual network is beyond the scope of this module.</span></span> <span data-ttu-id="97de9-126">詳細については、仮想ネットワークに関する他の学習モジュールを調べてください。</span><span class="sxs-lookup"><span data-stu-id="97de9-126">If you need more information, please explore other learning modules related to virtual networks.</span></span>

### <a name="what-is-a-firewall"></a><span data-ttu-id="97de9-127">ファイアウォールとは</span><span class="sxs-lookup"><span data-stu-id="97de9-127">What is a firewall?</span></span>

<span data-ttu-id="97de9-128">ファイアウォールとは、各要求の送信元 IP アドレスに基づいてサーバーへのアクセスを許可するサービスです。</span><span class="sxs-lookup"><span data-stu-id="97de9-128">A firewall is a service that grants server access based on the originating IP address of each request.</span></span> <span data-ttu-id="97de9-129">IP アドレスの範囲を指定するファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="97de9-129">You create firewall rules that specify ranges of IP addresses.</span></span> <span data-ttu-id="97de9-130">これらの許可された IP アドレスからのクライアントのみが、サーバーへのアクセスを許可されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-130">Only clients from these granted IP addresses, will be allowed to access the server.</span></span> <span data-ttu-id="97de9-131">一般に、ファイアウォール規則には特定のネットワーク プロトコルとポート情報も含まれます。</span><span class="sxs-lookup"><span data-stu-id="97de9-131">Firewall rules generally speaking also includes specific network protocol and port information.</span></span> <span data-ttu-id="97de9-132">たとえば、既定では、PostgreSQL サーバーはポート 5432 で TCP 要求をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="97de9-132">For example, a PostgreSQL server by default listens to TCP requests on port 5432.</span></span>

### <a name="azure-database-for-postgresql-server-firewall"></a><span data-ttu-id="97de9-133">Azure Database for PostgreSQL サーバーのファイアウォール</span><span class="sxs-lookup"><span data-stu-id="97de9-133">Azure Database for PostgreSQL server firewall</span></span>

<span data-ttu-id="97de9-134">Azure Database for PostgreSQL サーバーのファイアウォールは、ユーザーがアクセス許可を持つコンピューターを指定するまで、データベース サーバーへのすべてのアクセスを禁止します。</span><span class="sxs-lookup"><span data-stu-id="97de9-134">The Azure Database for PostgreSQL server firewall prevents all access to your database server until you specify which computers have permission.</span></span> <span data-ttu-id="97de9-135">ファイアウォールの構成を使用して、サーバーへの接続を許可する IP アドレスの範囲を指定できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-135">The firewall configuration allows you to specify a range of IP addresses that are allowed to connect to the server.</span></span> <span data-ttu-id="97de9-136">サーバーは常に、PostgreSQL の既定の接続情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="97de9-136">The server always uses the default PostgreSQL connection information.</span></span>

![Azure ファイアウォールの機能の図](../media-draft/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a><span data-ttu-id="97de9-138">Azure Database for PostgreSQL サーバーの SSL 接続</span><span class="sxs-lookup"><span data-stu-id="97de9-138">Azure Database for PostgreSQL server SSL connections</span></span>

<span data-ttu-id="97de9-139">Azure Database for PostgreSQL では、クライアント アプリケーションが PostgreSQL サービスに接続するときに、Secure Sockets Layer (SSL) を使用することが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-139">Azure Database for PostgreSQL prefers your client applications connects to the PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="97de9-140">データベース サーバーとクライアント アプリケーション間に SSL 接続を適用すると、サーバーとクライアント間のデータが暗号化されて、"man in the middle" などの攻撃から保護されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-140">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" and similar attacks by encrypting the data between the server and client.</span></span> <span data-ttu-id="97de9-141">SSL を有効にすると、クライアントとサーバー間の接続が機能するためには、キーの交換と厳密な認証が必要になります。</span><span class="sxs-lookup"><span data-stu-id="97de9-141">Enabling SSL requires the exchange of keys and strict authentication between client and server for the connection to work.</span></span> <span data-ttu-id="97de9-142">SSL の使用に関する詳細については、この学習モジュールの範囲を超えています。</span><span class="sxs-lookup"><span data-stu-id="97de9-142">Details about using SSL are beyond the scope of this learning module.</span></span> <span data-ttu-id="97de9-143">詳細については、SSL に関する他の学習モジュールを調べてください。</span><span class="sxs-lookup"><span data-stu-id="97de9-143">If you need more information, please explore other learning modules related to SSL.</span></span>

## <a name="configure-connection-security"></a><span data-ttu-id="97de9-144">接続のセキュリティを構成する</span><span class="sxs-lookup"><span data-stu-id="97de9-144">Configure Connection Security</span></span>

<span data-ttu-id="97de9-145">Azure Database for PostgreSQL サーバーのファイアウォールを構成するときの決定事項と手順を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="97de9-145">Let's look at the decisions and steps you make to configure an Azure Database for PostgreSQL server firewall.</span></span> <span data-ttu-id="97de9-146">前に作成したサーバーに接続する方法も確認します。</span><span class="sxs-lookup"><span data-stu-id="97de9-146">You'll also see how to connect to the server you've created earlier.</span></span>

<span data-ttu-id="97de9-147">最初に、[Azure portal](https://portal.azure.com?azure-portal=true) を開き、ファイアウォール規則を作成する対象のサーバー リソースに移動します。</span><span class="sxs-lookup"><span data-stu-id="97de9-147">First, you'll open the [Azure portal](https://portal.azure.com?azure-portal=true) and navigate to the server resource for which you would like to create a firewall rule.</span></span>

<span data-ttu-id="97de9-148">次に、**[接続のセキュリティ]** オプションを選択して、右側の接続のセキュリティ ブレードを開きます。</span><span class="sxs-lookup"><span data-stu-id="97de9-148">Then you'll select the **Connection Security** option to open the connection security blade to the right.</span></span>

![PostgreSQL データベースのセキュリティ設定](../media-draft/7-db-security-settings.png)

<span data-ttu-id="97de9-150">この画面には、いくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="97de9-150">On this screen, you have several options.</span></span> <span data-ttu-id="97de9-151">次のことが行えます。</span><span class="sxs-lookup"><span data-stu-id="97de9-151">You can:</span></span>

- <span data-ttu-id="97de9-152">**[+ クライアント IP の追加]** ボタンをクリックして、ポータルへのアクセスに使用する IP アドレスをファイアウォールのエントリとして追加します</span><span class="sxs-lookup"><span data-stu-id="97de9-152">Add the IP address you use to access the portal as a firewall entry by clicking on the **+ Add client IP** button</span></span>
- <span data-ttu-id="97de9-153">Azure サービスへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="97de9-153">Allow access to Azure services.</span></span> <span data-ttu-id="97de9-154">既定では、すべての Azure サービスは PostgreSQL サーバーにアクセス**できません**</span><span class="sxs-lookup"><span data-stu-id="97de9-154">By default all Azure services **don't** have access to the PostgreSQL server</span></span>
- <span data-ttu-id="97de9-155">IP アドレスの範囲を入力して、ファイアウォール規則を追加します</span><span class="sxs-lookup"><span data-stu-id="97de9-155">Add firewall rules by entering ranges of IP addresses</span></span>
- <span data-ttu-id="97de9-156">SSL 接続を適用します。</span><span class="sxs-lookup"><span data-stu-id="97de9-156">Enforce SSL connections.</span></span> <span data-ttu-id="97de9-157">このオプションを指定すると、クライアントは SSL 証明書を使用してサーバーに接続することを強制されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-157">This option forces you client to connect to the server using an SSL certificate.</span></span>

<span data-ttu-id="97de9-158">変更を行ったら、忘れずに入力フィールドの上の **[保存]** アイコンをクリックして更新された構成を保存してください。</span><span class="sxs-lookup"><span data-stu-id="97de9-158">Always remember to click on the **Save** icon above the entry fields to save the updated configuration once you've made changes.</span></span>

### <a name="allow-access-to-azure-services"></a><span data-ttu-id="97de9-159">Azure サービスへのアクセスを許可</span><span class="sxs-lookup"><span data-stu-id="97de9-159">Allow access to Azure services</span></span>

<span data-ttu-id="97de9-160">Azure Cloud Shell を使用してサーバーにアクセスしたりサーバーを構成したりするには、**[Azure サービスへのアクセスを許可]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="97de9-160">To use Azure Cloud Shell to access or configure your server, make sure to enable **Allow Access to Azure Services**.</span></span> <span data-ttu-id="97de9-161">この手順では、Cloud Shell からのアクセスを許可するように、サーバーの構成にファイアウォール規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="97de9-161">This step is going to add a firewall rule to the server configuration to allow access from Cloud Shell.</span></span> <span data-ttu-id="97de9-162">ただし、このルールはユーザーが追加するカスタム ルールの 1 つとしては表示されません。</span><span class="sxs-lookup"><span data-stu-id="97de9-162">This rule will not show as one of the custom rules you add though.</span></span>

<span data-ttu-id="97de9-163">**[SSL 接続を強制する]** を無効にする必要もあります。</span><span class="sxs-lookup"><span data-stu-id="97de9-163">You also need to disable **Enforce SSL connection**.</span></span> <span data-ttu-id="97de9-164">クライアント接続に SSL が必要な場合、PowerShell はサーバーに接続できません。</span><span class="sxs-lookup"><span data-stu-id="97de9-164">PowerShell cann't connect to the server if SSL is required for client connections.</span></span>

<span data-ttu-id="97de9-165">これらのオプションはどちらも、正しく構成しないと、コマンド ラインにエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-165">Both of these options will result in an error message displayed on the command line if not configured correctly.</span></span>

<span data-ttu-id="97de9-166">たとえば、Azure サービスへのアクセスを許可しないで、SSL 接続の強制を有効にした場合、ファイアウォールがアクセスをブロックすると次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="97de9-166">For example, if access is not allowed to Azure services and enforce SSL connections is enabled then you'll see something similar to this error when the firewall is blocking access.</span></span>

> <span data-ttu-id="97de9-167">psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL: SSL connection is required. (psql: 致命的: ホスト "123.45.67.89"、ユーザー "adminuser"、データベース "postgres" のエントリが pg_hba.conf にありません。SSL オン 致命的: SSL 接続が必要です。)</span><span class="sxs-lookup"><span data-stu-id="97de9-167">psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL:  SSL connection is required.</span></span> <span data-ttu-id="97de9-168">Please specify SSL options and retry. (SSL のオプションを指定して再試行してください。)</span><span class="sxs-lookup"><span data-stu-id="97de9-168">Please specify SSL options and retry.</span></span>

### <a name="create-a-firewall-rule-using-the-portal"></a><span data-ttu-id="97de9-169">ポータルを使用してファイアウォール規則を作成する</span><span class="sxs-lookup"><span data-stu-id="97de9-169">Create a firewall rule using the portal</span></span>

<span data-ttu-id="97de9-170">たとえば、任意の IP アドレスからのアクセスを許可するファイアウォール規則を作成したいものとします。</span><span class="sxs-lookup"><span data-stu-id="97de9-170">Let's say, you want to create a firewall rule that provides access from any IP address.</span></span>

> [!WARNING]
> <span data-ttu-id="97de9-171">このファイアウォール規則を作成すると、インターネット上のすべての IP アドレスがサーバーへの接続を試みることができます。</span><span class="sxs-lookup"><span data-stu-id="97de9-171">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="97de9-172">クライアントがユーザー名とパスワードなしではサーバーにアクセスできない場合でも、セキュリティへの影響を確実に理解したうえで、慎重にこの規則を有効にしてください。</span><span class="sxs-lookup"><span data-stu-id="97de9-172">Eventhough clients will not be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications.</span></span>

<span data-ttu-id="97de9-173">それぞれのラベルの付いたフィールドに次のデータを入力して、新しいファイアウォール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="97de9-173">You create a new firewall rule by entering the following data in the labeled fields:</span></span>

- <span data-ttu-id="97de9-174">規則名: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="97de9-174">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="97de9-175">開始 IP: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="97de9-175">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="97de9-176">終了 IP: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="97de9-176">End IP: `255.255.255.255`</span></span>

<span data-ttu-id="97de9-177">ファイアウォール規則を削除するには、削除するルールの末尾にある省略記号をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97de9-177">To remove a firewall rule, you'll click the ellipsis at the end of the rule you want to delete.</span></span> <span data-ttu-id="97de9-178">[削除] ボタンをクリックして、規則を削除します。</span><span class="sxs-lookup"><span data-stu-id="97de9-178">Click the Delete button to delete the rule.</span></span>

<span data-ttu-id="97de9-179">入力フィールドの上にある **[保存]** アイコンをクリックして、規則の削除をコミットします。</span><span class="sxs-lookup"><span data-stu-id="97de9-179">Click on the **Save** icon above the entry fields to commit the deletion of the rule.</span></span>

### <a name="create-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="97de9-180">Azure CLI を使用してファイアウォール規則を作成する</span><span class="sxs-lookup"><span data-stu-id="97de9-180">Create a firewall rule using the Azure CLI</span></span>

<span data-ttu-id="97de9-181">Azure Cloud Shell で Azure CLI の `az postgres server firewall-rule create` コマンドを使用してファイアウォール規則をサーバーに追加します。</span><span class="sxs-lookup"><span data-stu-id="97de9-181">You use the Azure CLI to add firewall rules to your server with the `az postgres server firewall-rule create` command using Azure CloudShell.</span></span>

<span data-ttu-id="97de9-182">上記と同じ規則を作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="97de9-182">Let's say you want to create the same rules as above You'' use the following command:</span></span>

  ```bash
  az postgres server firewall-rule create --resource-group <resource_group_name> --server <server-name> --name AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
  ```

<span data-ttu-id="97de9-183">サーバーからファイアウォール規則を削除するには、`az postgres server firewall-rule delete` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="97de9-183">You remove firewall rules from your server with the command `az postgres server firewall-rule delete`.</span></span>

<span data-ttu-id="97de9-184">作成したファイアウォールを削除するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="97de9-184">Let's say you want to delete the firewall you created then use the following command:</span></span>

  ```bash
  az postgres server firewall-rule delete --name AllowAll --resource-group <resource_group_name> --server-name <server-name>
  ```

## <a name="connecting-to-your-server"></a><span data-ttu-id="97de9-185">サーバーへの接続</span><span class="sxs-lookup"><span data-stu-id="97de9-185">Connecting to your server</span></span>

<span data-ttu-id="97de9-186">他の最新のデータベースと同じように、PostgreSQL でも最適なパフォーマンスを実現するには定期的なサーバー管理が必要です。</span><span class="sxs-lookup"><span data-stu-id="97de9-186">Like any modern database, PostgreSQL requires regularly server administration to achieve best performance.</span></span> <span data-ttu-id="97de9-187">Azure Database for PostgreSQL サーバーに接続して管理するには、多くのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="97de9-187">You have a number of options to connect and manage your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="97de9-188">ここでは、`psql` を使用してサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="97de9-188">We'll use `psql` to connect to the server.</span></span>

### <a name="what-is-psql"></a><span data-ttu-id="97de9-189">psql とは</span><span class="sxs-lookup"><span data-stu-id="97de9-189">What is psql?</span></span>

<span data-ttu-id="97de9-190">`psql` という名前のコマンド ライン ツールは、PostgreSQL サーバーとデータベースを操作するための PostgreSQL の分散対話型ターミナルです。</span><span class="sxs-lookup"><span data-stu-id="97de9-190">The command-line tool called `psql` is the PostgreSQL distributed interactive terminal for working with PostgreSQL server and databases.</span></span> <span data-ttu-id="97de9-191">`psql` は他の PostgreSQL 実装と同じように Azure Database for PostgreSQL で動作し、Azure Cloud Shell に付属します。</span><span class="sxs-lookup"><span data-stu-id="97de9-191">`psql` works with Azure Database for PostgreSQL the same as with any other PostgreSQL implementation and is included with the Azure Cloud Shell.</span></span> <span data-ttu-id="97de9-192">`psql` ツールでは、データベースを管理できるだけでなく、これらのデータベースに対してクエリを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="97de9-192">The `psql` tool allows you to manage databases as well as execute structure queries against these databases.</span></span>

<span data-ttu-id="97de9-193">`psql` を使うには、PostgreSQL サーバーに正常に接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="97de9-193">Using `psql` requires a successful connection to a PostgreSQL server.</span></span> <span data-ttu-id="97de9-194">`psql` では多数のコマンド ライン パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-194">There are a number of command-line parameters available for use when working with `psql`.</span></span>

- <span data-ttu-id="97de9-195">`--host` - 接続するホスト</span><span class="sxs-lookup"><span data-stu-id="97de9-195">`--host` - the host to which you'd like to connect</span></span>
- <span data-ttu-id="97de9-196">`--username` - 接続に使用する</span><span class="sxs-lookup"><span data-stu-id="97de9-196">`--username` - the user name/i.d.</span></span> <span data-ttu-id="97de9-197">ユーザー名/ID</span><span class="sxs-lookup"><span data-stu-id="97de9-197">with which to connect</span></span>
- <span data-ttu-id="97de9-198">`--dbname` - 接続先のデータベースの名前。</span><span class="sxs-lookup"><span data-stu-id="97de9-198">`--dbname` - the name of the database to connect to.</span></span>

> [!Note]
> <span data-ttu-id="97de9-199">サーバー アクセスとデータベース構成を管理するときは、通常、`postgres` 管理データベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="97de9-199">You'll typically connect to the `postgres` management database when managing your server access and databases configuration.</span></span>

<span data-ttu-id="97de9-200">完全なコマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="97de9-200">Here is the complete command:</span></span>

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=<database>
  ```

<span data-ttu-id="97de9-201">接続するとコマンド プロンプトが表示され、サーバーとデータベースに対してコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="97de9-201">Once connected, you'll be presented with a command prompt and can execute commands to your server and databases.</span></span>

<span data-ttu-id="97de9-202">Azure Database for PostgreSQL のセキュリティ設定を構成する手順を見てきました。</span><span class="sxs-lookup"><span data-stu-id="97de9-202">You've now seen the steps you take to configure an Azure Database for PostgreSQL security settings.</span></span> <span data-ttu-id="97de9-203">次のユニットでは、Azure Database for PostgreSQL のセキュリティ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="97de9-203">In the next unit, you'll configure an  Azure Database for PostgreSQL security settings.</span></span> <span data-ttu-id="97de9-204">また、Cloud Shell を使用してサーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="97de9-204">You'll also connect to the server using Cloud Shell.</span></span>