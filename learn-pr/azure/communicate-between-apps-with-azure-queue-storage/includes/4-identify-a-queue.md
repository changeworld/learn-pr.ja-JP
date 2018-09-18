<span data-ttu-id="3faf4-101">ストレージ アカウントを作成したので、そのストレージに格納されるキューをどのように使用するかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="3faf4-101">Now that we have a storage account, let's look at how we work with the queue that it will hold.</span></span>

<span data-ttu-id="3faf4-102">キューにアクセスするには、3 つの情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="3faf4-102">To access a queue, you need three pieces of information:</span></span>

 1. <span data-ttu-id="3faf4-103">ストレージ アカウント名</span><span class="sxs-lookup"><span data-stu-id="3faf4-103">Storage account name</span></span>
 2. <span data-ttu-id="3faf4-104">キュー名</span><span class="sxs-lookup"><span data-stu-id="3faf4-104">Queue name</span></span>
 3. <span data-ttu-id="3faf4-105">承認トークン</span><span class="sxs-lookup"><span data-stu-id="3faf4-105">Authorization token</span></span>

<span data-ttu-id="3faf4-106">この情報は、キューにアクセスする両方のアプリケーション (メッセージを追加する Web フロントエンドと、追加されたメッセージを処理する中間層) によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-106">This information is used by both applications that talk to the queue (the web front end that adds messages and the mid-tier that processes them).</span></span>

## <a name="queue-identity"></a><span data-ttu-id="3faf4-107">キュー ID</span><span class="sxs-lookup"><span data-stu-id="3faf4-107">Queue identity</span></span>

<span data-ttu-id="3faf4-108">すべてのキューには、作成時に割り当てる名前があります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-108">Every queue has a name that you assign during creation.</span></span> <span data-ttu-id="3faf4-109">名前は、ストレージ アカウント内で一意である必要があります。しかし、ストレージ アカウント名とは異なり、グローバルに一意である必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3faf4-109">The name must be unique within your storage account but doesn't need to be globally unique (unlike the storage account name).</span></span>

<span data-ttu-id="3faf4-110">ストレージ アカウント名とキュー名の組み合わせによってキューを一意に識別します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-110">The combination of your storage account name and your queue name uniquely identifies a queue.</span></span>

## <a name="access-authorization"></a><span data-ttu-id="3faf4-111">アクセスの承認</span><span class="sxs-lookup"><span data-stu-id="3faf4-111">Access authorization</span></span>

<span data-ttu-id="3faf4-112">キューに対するすべての要求は承認される必要があり、選択肢として複数のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-112">Every request to a queue must be authorized and there are several options to choose from.</span></span>

| <span data-ttu-id="3faf4-113">承認の種類</span><span class="sxs-lookup"><span data-stu-id="3faf4-113">Authorization Type</span></span> | <span data-ttu-id="3faf4-114">説明</span><span class="sxs-lookup"><span data-stu-id="3faf4-114">Description</span></span> |
|--------------------|-------------|
| <span data-ttu-id="3faf4-115">**Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="3faf4-115">**Azure Active Directory**</span></span> | <span data-ttu-id="3faf4-116">ロールベースの認証を使用し、AAD の資格情報に基づいて特定のクライアントを識別できます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-116">You can use role-based authentication and identify specific clients based on AAD credentials.</span></span> |
| <span data-ttu-id="3faf4-117">**共有キー**</span><span class="sxs-lookup"><span data-stu-id="3faf4-117">**Shared Key**</span></span> | <span data-ttu-id="3faf4-118">これはストレージ アカウントに関連付けられた、暗号化されたキー署名です。**アカウント キー**と呼ばれる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-118">Sometimes referred to as an **account key**, this is an encrypted key signature associated with the storage account.</span></span> <span data-ttu-id="3faf4-119">すべてのストレージ アカウントでは、これらのうち 2 つのキーを使用し、アクセスを認証するために各要求と共にキーを渡します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-119">Every storage account has two of these keys that can be passed with each request to authenticate access.</span></span> <span data-ttu-id="3faf4-120">このアプローチを使用することは、ルート パスワードを使用するのに似ています。ルート パスワードにより、ストレージ アカウントへの_フル アクセス_が提供されます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-120">Using this approach is like using a root password - it provides _full access_ to the storage account.</span></span> |
| <span data-ttu-id="3faf4-121">**共有アクセス署名**</span><span class="sxs-lookup"><span data-stu-id="3faf4-121">**Shared access signature**</span></span> | <span data-ttu-id="3faf4-122">共有アクセス署名 (SAS) は、ストレージ アカウントでのオブジェクトに対する制限されたアクセスをクライアントに許可する、生成された URI です。</span><span class="sxs-lookup"><span data-stu-id="3faf4-122">A shared access signature (SAS) is a generated URI that grants limited access to objects in your storage account to clients.</span></span> <span data-ttu-id="3faf4-123">特定のリソース、アクセス許可、スコープへのアクセスをあるデータ範囲に制限し、一定期間後にアクセス権を自動的に無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-123">You can restrict access to specific resources, permissions, and scope to a data range to automatically turn off access after a period of time.</span></span>  |

> [!NOTE]
> <span data-ttu-id="3faf4-124">キューに関する作業を始める最もシンプルな方法であるため、ここではアカウント キー承認を使用します。ただし、運用アプリでは共有アクセス署名 (SAS) または Azure Active Directory (AAD) を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3faf4-124">We will use the account key authorization because it is the simplest way to get started working with queues, however it's recommended that you either use shared access signature (SAS) or Azure Active Directory (AAD) in production apps.</span></span>

### <a name="retrieving-the-account-key"></a><span data-ttu-id="3faf4-125">アカウント キーの取得</span><span class="sxs-lookup"><span data-stu-id="3faf4-125">Retrieving the account key</span></span>
 
<span data-ttu-id="3faf4-126">アカウント キーは、Azure portal のご自分のストレージ アカウントの **[設定] > [アクセス キー]** セクションで確認できます。またはコマンド ラインを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-126">Your account key is available in the **Settings > Access keys** section of your storage account in the Azure portal, or you can retrieve it through the command line:</span></span>

```azurecli
az storage account keys list ...
```

```powershell
Get-AzureRmStorageAccountKey ...
```

## <a name="accessing-queues"></a><span data-ttu-id="3faf4-127">キューへのアクセス</span><span class="sxs-lookup"><span data-stu-id="3faf4-127">Accessing queues</span></span>

<span data-ttu-id="3faf4-128">REST API を使用してキューにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="3faf4-128">You access a queue using a REST API.</span></span> <span data-ttu-id="3faf4-129">そのためには、ストレージ アカウントに指定した名前とドメイン `queue.core.windows.net` およびアクセスするキューへのパスを結合した URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-129">To do this, you'll use a URL that combines the name you gave the storage account with the domain `queue.core.windows.net` and the path to the queue you want to work with.</span></span> <span data-ttu-id="3faf4-130">たとえば、「`http://<storage account>.queue.core.windows.net/<queue name>`」のように入力します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-130">For example: `http://<storage account>.queue.core.windows.net/<queue name>`.</span></span> <span data-ttu-id="3faf4-131">すべての要求に `Authorization` ヘッダーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-131">An `Authorization` header must be included with every request.</span></span> <span data-ttu-id="3faf4-132">値として 3 つの承認スタイルのうちどれでも設定できます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-132">The value can be any of the three authorization styles.</span></span>

### <a name="using-the-azure-storage-client-library-for-net"></a><span data-ttu-id="3faf4-133">.NET 用 Azure Storage クライアント ライブラリの使用</span><span class="sxs-lookup"><span data-stu-id="3faf4-133">Using the Azure Storage Client Library for .NET</span></span>

<span data-ttu-id="3faf4-134">.NET 用 Azure Storage クライアント ライブラリは、Microsoft によって提供された、ユーザーに代わって REST 要求の作成と REST 応答の解析を行うライブラリです。</span><span class="sxs-lookup"><span data-stu-id="3faf4-134">The Azure Storage Client Library for .NET is a library provided by Microsoft that formulates REST requests and parses REST responses for you.</span></span> <span data-ttu-id="3faf4-135">これにより、作成する必要があるコードの量が大幅に減少します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-135">This greatly reduces the amount of code you need to write.</span></span> <span data-ttu-id="3faf4-136">クライアント ライブラリを使用したアクセスでも、同じ情報 (ストレージ アカウント名、キュー名、アカウント キー) が必要です。ただし、形式は異なります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-136">Access using the client library still requires the same pieces of information (storage account name, queue name, and account key); however, they are organized differently.</span></span>

<span data-ttu-id="3faf4-137">クライアント ライブラリでは、**接続文字列**を使用して接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="3faf4-137">The client library uses a **connection string** to establish your connection.</span></span> <span data-ttu-id="3faf4-138">接続文字列は、Azure portal のご自分のストレージ アカウントの **[設定]** セクションで確認するか、Azure CLI と PowerShell を使用して確認できます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-138">Your connection string is available in the **Settings** section of your Storage Account in the Azure portal, or through the Azure CLI and PowerShell.</span></span>

<span data-ttu-id="3faf4-139">接続文字列は、ストレージ アカウント名とアカウント キーを結合した文字列で、ストレージ アカウントにアクセスするためにアプリケーションに知らされる必要があります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-139">A connection string is a string that combines a storage account name and account key and must be known to the application to access the storage account.</span></span> <span data-ttu-id="3faf4-140">形式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-140">The format looks like this:</span></span>

```csharp
string connectionString = "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
```

> [!WARNING]
> <span data-ttu-id="3faf4-141">この接続文字列にアクセスできるユーザーはだれでもキューを操作できるため、この文字列の値は、安全な場所に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3faf4-141">This string value should be stored in a secure location since anyone who has access to this connection string would be able to manipulate the queue.</span></span>

<span data-ttu-id="3faf4-142">接続文字列にキュー名が含まれていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3faf4-142">Notice that the connection string doesn't include the queue name.</span></span> <span data-ttu-id="3faf4-143">キュー名は、キューへの接続を確立するときにコードの中で指定されます。</span><span class="sxs-lookup"><span data-stu-id="3faf4-143">The queue name is supplied in your code when you establish a connection to the queue.</span></span>

<span data-ttu-id="3faf4-144">Azure から接続文字列を取得し、それを使用するために新しいアプリケーションを設定しましょう。</span><span class="sxs-lookup"><span data-stu-id="3faf4-144">Let's get our connection string from Azure and set up a new application to use it.</span></span>