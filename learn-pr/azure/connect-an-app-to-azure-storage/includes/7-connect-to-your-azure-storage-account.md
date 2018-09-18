<span data-ttu-id="246c1-101">アプリケーションに必要なクライアント ライブラリを追加し、Azure ストレージ アカウントに接続する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="246c1-101">You have added the required client libraries to your application and are ready to connect to your Azure storage account.</span></span>

<span data-ttu-id="246c1-102">ストレージ アカウントでデータを操作するには、アプリで次の 2 つのデータが必要になります。</span><span class="sxs-lookup"><span data-stu-id="246c1-102">To work with data in a storage account, your app will need two pieces of data:</span></span>

1. [<span data-ttu-id="246c1-103">アクセス キー</span><span class="sxs-lookup"><span data-stu-id="246c1-103">An access key</span></span>](#access-key)
1. [<span data-ttu-id="246c1-104">REST API エンドポイント</span><span class="sxs-lookup"><span data-stu-id="246c1-104">The REST API endpoint</span></span>](#rest-endpoint)

<a name="access-key"></a>

## <a name="security-access-keys"></a><span data-ttu-id="246c1-105">セキュリティ アクセス キー</span><span class="sxs-lookup"><span data-stu-id="246c1-105">Security access keys</span></span>

<span data-ttu-id="246c1-106">各ストレージ アカウントには、ストレージ アカウントをセキュリティで保護するために使用される一意の_アクセス キー_が 2 つあります。</span><span class="sxs-lookup"><span data-stu-id="246c1-106">Each storage account has two unique _access keys_ that are used to secure the storage account.</span></span> <span data-ttu-id="246c1-107">アプリで複数のストレージ アカウントに接続する必要がある場合、そのアプリでは各ストレージ アカウント用のアクセス キーが必要になります。</span><span class="sxs-lookup"><span data-stu-id="246c1-107">If your app needs to connect to multiple storage accounts, then your app will require an access key for each storage account.</span></span>

![クラウド内の異なる 2 つのストレージ アカウントに接続されているアプリケーションを示す図。](..\media\6-multiple-accounts.png)

<a name="rest-endpoint"></a>

## <a name="rest-api-endpoint"></a><span data-ttu-id="246c1-110">REST API エンドポイント</span><span class="sxs-lookup"><span data-stu-id="246c1-110">REST API endpoint</span></span>

<span data-ttu-id="246c1-111">ストレージ アカウントに対する認証用のアクセス キーに加え、アプリでは、REST 要求を発行するためのストレージ サービス エンドポイントを認識する必要があります。</span><span class="sxs-lookup"><span data-stu-id="246c1-111">In addition to access keys for authentication to storage accounts, your app will need to know the storage service endpoints to issue the REST requests.</span></span> 

<span data-ttu-id="246c1-112">REST エンドポイントは、ストレージ アカウントの_名前_、データ型、および既知のドメインを組み合わせたものです。</span><span class="sxs-lookup"><span data-stu-id="246c1-112">The REST endpoint is a combination of your storage account _name_, the data type, and a known domain.</span></span> <span data-ttu-id="246c1-113">例:</span><span class="sxs-lookup"><span data-stu-id="246c1-113">For example:</span></span>

| <span data-ttu-id="246c1-114">データ型</span><span class="sxs-lookup"><span data-stu-id="246c1-114">Data type</span></span> | <span data-ttu-id="246c1-115">エンドポイント例</span><span class="sxs-lookup"><span data-stu-id="246c1-115">Example endpoint</span></span> |
|-----------|------------------|
| <span data-ttu-id="246c1-116">BLOB</span><span class="sxs-lookup"><span data-stu-id="246c1-116">Blobs</span></span>     | `https://[name].blob.core.windows.net/` |
| <span data-ttu-id="246c1-117">キュー</span><span class="sxs-lookup"><span data-stu-id="246c1-117">Queues</span></span>    | `https://[name].queue.core.windows.net/` |
| <span data-ttu-id="246c1-118">テーブル</span><span class="sxs-lookup"><span data-stu-id="246c1-118">Table</span></span>     | `https://[name].table.core.windows.net/` |
| <span data-ttu-id="246c1-119">ファイル</span><span class="sxs-lookup"><span data-stu-id="246c1-119">Files</span></span>     | `https://[name].file.core.windows.net/` |

<span data-ttu-id="246c1-120">Azure にカスタム ドメインを関連付けている場合は、エンドポイント用のカスタム ドメイン URL を作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="246c1-120">If you have a custom domain tied to Azure, then you can also create a custom domain URL for the endpoint.</span></span>

## <a name="connection-strings"></a><span data-ttu-id="246c1-121">接続文字列</span><span class="sxs-lookup"><span data-stu-id="246c1-121">Connection strings</span></span>

<span data-ttu-id="246c1-122">この情報を処理する最もシンプルな方法は、**ストレージ アカウントの接続文字列**を使用することです。接続文字列では、単一のテキスト文字列に必要なすべての接続情報が提供されます。</span><span class="sxs-lookup"><span data-stu-id="246c1-122">The simplest way to handle this information is to use a **storage account connection string** A connection string provides all needed connectivity information in a single text string.</span></span>

<span data-ttu-id="246c1-123">Azure Storage の接続文字列は、下の例と似ていますが、アクセス キーとアカウント名はご利用の特定のストレージ アカウントのものとなります。</span><span class="sxs-lookup"><span data-stu-id="246c1-123">Azure Storage connection strings look similar to the example below but with the access key and account name of your specific storage account:</span></span>

```
DefaultEndpointsProtocol=https;AccountName={your-storage};
   AccountKey={your-access-key};
   EndpointSuffix=core.windows.net
```

## <a name="security"></a><span data-ttu-id="246c1-124">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="246c1-124">Security</span></span>

<span data-ttu-id="246c1-125">アクセス キーは、ストレージ アカウントにアクセスするために重要で、ストレージ アカウントにアクセスを許可したくないシステムまたはユーザーには知らせないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="246c1-125">Access keys are critical to providing access to your storage account and as a result, should not be given to any system or person that you do not wish to have access to your storage account.</span></span> <span data-ttu-id="246c1-126">アクセス キーは、コンピューターにアクセスするためのユーザー名とパスワードと同じです。</span><span class="sxs-lookup"><span data-stu-id="246c1-126">Access keys are the equivalent of a username and password to access your computer.</span></span>

<span data-ttu-id="246c1-127">通常、ストレージ アカウントの接続情報は、環境変数、データベース、または構成ファイル内に格納されます。</span><span class="sxs-lookup"><span data-stu-id="246c1-127">Typically, storage account connectivity information is stored within an environment variable, database, or configuration file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="246c1-128">この情報を構成ファイルに格納すると、そのファイルがソース管理に含められてパブリック リポジトリに格納された場合に危険であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="246c1-128">It is important to note that storing this information in a configuration file can be dangerous if you include that file in source control and store it in a public repository.</span></span> <span data-ttu-id="246c1-129">これはよくある間違いであり、すべてのユーザーがパブリック リポジトリのソース コードと、ストレージ アカウントの接続情報を参照できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="246c1-129">This is a common mistake and means that anyone can browse your source code in the public repository and see your storage account connection information.</span></span>

<span data-ttu-id="246c1-130">各ストレージ アカウントには、アクセス キーが 2 つあります。</span><span class="sxs-lookup"><span data-stu-id="246c1-130">Each storage account has two access keys.</span></span> <span data-ttu-id="246c1-131">これは、ストレージ アカウントを安全に保つためのセキュリティ上のベスト プラクティスの一部として、キーを定期的にローテーション (再生成) できるようにするためです。</span><span class="sxs-lookup"><span data-stu-id="246c1-131">The reason for this is to allow keys to be rotated (regenerated) periodically as part of security best practice in keeping your storage account secure.</span></span> <span data-ttu-id="246c1-132">このプロセスは、Azure portal または Azure CLI / PowerShell コマンド ライン ツールから実行できます。</span><span class="sxs-lookup"><span data-stu-id="246c1-132">This process can be done from the Azure portal or the Azure CLI / PowerShell command line tool.</span></span>

<span data-ttu-id="246c1-133">キーをローテーションすると元のキーの値がするに無効になり、キーを不正に取得したユーザーのアクセスが取り消されます。</span><span class="sxs-lookup"><span data-stu-id="246c1-133">Rotating a key will invalidate the original key value immediately and will revoke access to anyone who obtained the key inappropriately.</span></span> <span data-ttu-id="246c1-134">2 つのキーがサポートされているため、キーのローテーション時に、キーを使用するアプリケーションでダウンタイムが発生しないようにできます。</span><span class="sxs-lookup"><span data-stu-id="246c1-134">With support for two keys, you can rotate keys without causing downtime in your applications that use them.</span></span> <span data-ttu-id="246c1-135">アプリで別のアクセス キーを使用するように切り替えて、その間にもう 1 つのキーを再生成します。</span><span class="sxs-lookup"><span data-stu-id="246c1-135">Your app can switch to using the alternate access key, while the other key is regenerated.</span></span> <span data-ttu-id="246c1-136">このストレージ アカウントを使用するアプリが複数ある場合、すべて同じキーを使用して、この手法をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="246c1-136">If you have multiple apps using this storage account, they should all use the same key to support this technique.</span></span> <span data-ttu-id="246c1-137">基本的な考え方を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="246c1-137">Here's the basic idea:</span></span>

1. <span data-ttu-id="246c1-138">ストレージ アカウントのセカンダリ アクセス キーを参照するように、アプリケーション コードの接続文字列を更新します。</span><span class="sxs-lookup"><span data-stu-id="246c1-138">Update the connection strings in your application code to reference the secondary access key of the storage account.</span></span>
2. <span data-ttu-id="246c1-139">Azure portal またはコマンド ライン ツールを使用して、ストレージ アカウント用のプライマリ アクセス キーを再生成します。</span><span class="sxs-lookup"><span data-stu-id="246c1-139">Regenerate the primary access key for your storage account using the Azure portal or command line tool.</span></span>
3. <span data-ttu-id="246c1-140">新しいプライマリ アクセス キーを参照するように、コードの接続文字列を更新します。</span><span class="sxs-lookup"><span data-stu-id="246c1-140">Update the connection strings in your code to reference the new primary access key.</span></span>
4. <span data-ttu-id="246c1-141">同様に、セカンダリ アクセス キーを再生成します。</span><span class="sxs-lookup"><span data-stu-id="246c1-141">Regenerate the secondary access key in the same manner.</span></span>

> [!TIP]
> <span data-ttu-id="246c1-142">アクセス キーを定期的にローテーションし、パスワードを変更する場合と同じように、プライベートな状態を保つことを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="246c1-142">It's highly recommended that you periodically rotate your access keys to ensure they remain private - just like changing your passwords.</span></span> <span data-ttu-id="246c1-143">サーバー アプリケーションでキーを使用する場合は、**Azure Key Vault** を利用してアクセス キーを自動的に格納することができます。</span><span class="sxs-lookup"><span data-stu-id="246c1-143">If you are using the key in a server application, you can use an **Azure Key Vault** to store the access key for you.</span></span> <span data-ttu-id="246c1-144">Key Vault には、ストレージ アカウントに直接同期し、キーを定期的に自動でローテーションするためのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="246c1-144">Key Vaults include support to synchronize directly to the Storage Account and automatically rotate the keys periodically.</span></span> <span data-ttu-id="246c1-145">Key Vault を使用すると、セキュリティ層が追加されるため、ご利用のアプリでアクセス キーを直接操作する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="246c1-145">Using a Key Vault provides an additional layer of security so your app never has to work directly with an access key.</span></span>

### <a name="shared-access-signatures-sas"></a><span data-ttu-id="246c1-146">共有アクセス署名 (SAS)</span><span class="sxs-lookup"><span data-stu-id="246c1-146">Shared access signatures (SAS)</span></span>

<span data-ttu-id="246c1-147">アクセス キーは、ストレージ アカウントへのアクセスを認証する最も簡単な方法ですが、コンピューター上のルート パスワードと同様に、ストレージ アカウント内のすべてのものに対するフル アクセスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="246c1-147">Access keys are the easiest approach to authenticating access to a storage account, however they provide full access to anything in the storage account - similar to a root password on a computer.</span></span> 

<span data-ttu-id="246c1-148">ストレージ アカウントでは、_共有アクセス署名_と呼ばれる別の認証メカニズムが提供され、制限付きアクセスを許可する必要があるシナリオにおいて制限付きのアクセス許可と有効期限がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="246c1-148">Storage accounts offer a separate authentication mechanism called _shared access signatures_ that support expiration and limited permissions for scenarios where you need to grant limited access.</span></span> <span data-ttu-id="246c1-149">ご自分のストレージ アカウントに他のユーザーがデータを読み書きできるようにする場合は、この方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="246c1-149">You should use this approach when you are allowing other users to read and write data to your storage account.</span></span> <span data-ttu-id="246c1-150">モジュールの最後にこの高度なトピックに関するドキュメントへのリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="246c1-150">There are links to our documentation on this advanced topic at the end of the module.</span></span>
