<span data-ttu-id="60bca-101">イベント ハブを作成して構成したら、イベント データ ストリームを送受信できるように、アプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60bca-101">After you have created and configured your Event Hub, you'll need to configure applications to send and receive event data streams.</span></span>

<span data-ttu-id="60bca-102">たとえば、支払い処理ソリューションでは、顧客のクレジット カードのデータを収集するために何らかの形式の送信側アプリケーションが使用され、そのクレジット カードが有効であることを確認するために受信側アプリケーションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="60bca-102">For example, a payment processing solution will use some form of sender application to collect customer's credit card data and a receiver application to verify that the credit card is valid.</span></span>

<span data-ttu-id="60bca-103">.NET アプリケーションの場合と比較して、Java アプリケーションの構成方法には違いがありますが、アプリケーションをイベント ハブに接続し、アプリケーションによってメッセージの送受信を正常に行えるようにするには一般的な原則に従います。</span><span class="sxs-lookup"><span data-stu-id="60bca-103">Although there are differences in how a Java application is configured, compared to a .NET application, there are general principles for enabling applications to connect to an Event Hub, and to successfully send or receive messages.</span></span> <span data-ttu-id="60bca-104">そのため、Java 構成テキスト ファイルの編集プロセスは、Visual Studio を使用して .NET アプリケーションを準備するのとは異なりますが、原則は同じです。</span><span class="sxs-lookup"><span data-stu-id="60bca-104">So, although the process of editing Java configuration text files is different to preparing a .NET application using Visual Studio, the principles are the same.</span></span>

## <a name="what-are-the-minimum-event-hub-application-requirements"></a><span data-ttu-id="60bca-105">イベント ハブ アプリケーションの最小要件とは? </span><span class="sxs-lookup"><span data-stu-id="60bca-105">What are the minimum Event Hub application requirements?</span></span>

<span data-ttu-id="60bca-106">イベント ハブにメッセージを送信できるようにアプリケーションを構成するには、アプリケーションによって接続の資格情報が作成されるように、次の情報を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60bca-106">To configure an application to send messages to an Event Hub, you must provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="60bca-107">イベント ハブ名前空間の名前</span><span class="sxs-lookup"><span data-stu-id="60bca-107">Event Hub namespace name</span></span>
- <span data-ttu-id="60bca-108">イベント ハブ名</span><span class="sxs-lookup"><span data-stu-id="60bca-108">Event Hub name</span></span>
- <span data-ttu-id="60bca-109">共有アクセス ポリシー名</span><span class="sxs-lookup"><span data-stu-id="60bca-109">Shared access policy name</span></span>
- <span data-ttu-id="60bca-110">プライマリ共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="60bca-110">Primary shared access key</span></span>

<span data-ttu-id="60bca-111">イベント ハブからメッセージを受信できるようにアプリケーションを構成するには、アプリケーションによって接続の資格情報が作成されるように、次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="60bca-111">To configure an application to receive messages from an Event Hub, provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="60bca-112">イベント ハブ名前空間の名前</span><span class="sxs-lookup"><span data-stu-id="60bca-112">Event Hub namespace name</span></span>
- <span data-ttu-id="60bca-113">イベント ハブ名</span><span class="sxs-lookup"><span data-stu-id="60bca-113">Event Hub name</span></span>
- <span data-ttu-id="60bca-114">共有アクセス ポリシー名</span><span class="sxs-lookup"><span data-stu-id="60bca-114">Shared access policy name</span></span>
- <span data-ttu-id="60bca-115">プライマリ共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="60bca-115">Primary shared access key</span></span>
- <span data-ttu-id="60bca-116">ストレージ アカウント名</span><span class="sxs-lookup"><span data-stu-id="60bca-116">Storage account name</span></span>
- <span data-ttu-id="60bca-117">ストレージ アカウントの接続文字列</span><span class="sxs-lookup"><span data-stu-id="60bca-117">Storage account connection string</span></span>
- <span data-ttu-id="60bca-118">ストレージ アカウントのコンテナー名</span><span class="sxs-lookup"><span data-stu-id="60bca-118">Storage account container name</span></span>

<span data-ttu-id="60bca-119">Azure Blob Storage 内にメッセージを格納する受信側アプリケーションがある場合は、まずストレージ アカウントも構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60bca-119">If you have a receiver application that stores messages in Azure Blob Storage, you'll also need to first configure a storage account.</span></span>

## <a name="azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a><span data-ttu-id="60bca-120">汎用の標準ストレージ アカウントを作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="60bca-120">Azure CLI commands for creating a general-purpose standard storage account</span></span>

<span data-ttu-id="60bca-121">Azure CLI によって、ストレージ アカウントの作成と管理に使用できるコマンドのセットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="60bca-121">The Azure CLI provides a set of commands you can use to create and manage a storage account.</span></span> <span data-ttu-id="60bca-122">次のユニットではこれらを操作しますが、コマンドの基本的な概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="60bca-122">We'll work with them in the next unit, but here's a basic synopsis of the commands.</span></span> 

> [!TIP]
> <span data-ttu-id="60bca-123">モジュール「**Azure Storage の概要**」から始まる、ストレージ アカウントに対応する MS 学習モジュールがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="60bca-123">There are several MS Learn modules that cover storage accounts, starting in the module **Introduction to Azure Storage**.</span></span>

| <span data-ttu-id="60bca-124">コマンド</span><span class="sxs-lookup"><span data-stu-id="60bca-124">Command</span></span> | <span data-ttu-id="60bca-125">説明</span><span class="sxs-lookup"><span data-stu-id="60bca-125">Description</span></span> |
|---------|-------------|
| `storage account create` | <span data-ttu-id="60bca-126">汎用 v2 ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="60bca-126">Create a general-purpose V2 Storage account.</span></span> |
| `storage account key list` | <span data-ttu-id="60bca-127">ストレージ アカウント キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="60bca-127">Retrieve the storage account key.</span></span> |
| `storage account show-connection-string` | <span data-ttu-id="60bca-128">Azure Storage アカウントへの接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="60bca-128">Retrieve the connection string for an Azure Storage account.</span></span> |
| `storage container create` | <span data-ttu-id="60bca-129">ストレージ アカウントに新しいコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="60bca-129">Creates a new container in a storage account.</span></span> |

## <a name="shell-command-for-cloning-an-application-github-repository"></a><span data-ttu-id="60bca-130">アプリケーションの GitHub リポジトリを複製するためのシェル コマンド</span><span class="sxs-lookup"><span data-stu-id="60bca-130">Shell command for cloning an application GitHub repository</span></span>

<span data-ttu-id="60bca-131">Git は分散型バージョン管理モデルを使用するコラボレーション ツールであり、ソフトウェア プロジェクトやドキュメント プロジェクトでの共同作業向けに設計されています。</span><span class="sxs-lookup"><span data-stu-id="60bca-131">Git is a collaboration tool that uses a distributed version control model, and is designed for collaborative working on software and documentation projects.</span></span> <span data-ttu-id="60bca-132">Windows を含む複数のプラットフォームで Git クライアントを利用することができ、Git コマンドラインは Azure Bash Cloud Shell に含まれています。</span><span class="sxs-lookup"><span data-stu-id="60bca-132">Git clients are available for multiple platforms, including Windows, and the Git command line is included in the Azure Bash cloud shell.</span></span> <span data-ttu-id="60bca-133">GitHub は Git リポジトリに対する Web ベースのホスティング サービスです。</span><span class="sxs-lookup"><span data-stu-id="60bca-133">GitHub is a web-based hosting service for Git repositories.</span></span> 

<span data-ttu-id="60bca-134">GitHub 内でプロジェクトとしてホストされているアプリケーションがある場合は、**git clone** コマンドを使用してプロジェクトのリポジトリを複製することにより、プロジェクトのローカル コピーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="60bca-134">If you have an application that is hosted as a project in GitHub, you can make a local copy of the project, by cloning its repository using the **git clone** command.</span></span> <span data-ttu-id="60bca-135">次のユニットでこの操作を行います。</span><span class="sxs-lookup"><span data-stu-id="60bca-135">We'll do this in the next unit.</span></span>

## <a name="editing-files-in-the-cloud-shell"></a><span data-ttu-id="60bca-136">Cloud Shell でファイルを編集する</span><span class="sxs-lookup"><span data-stu-id="60bca-136">Editing files in the Cloud SHell</span></span>

<span data-ttu-id="60bca-137">アプリケーションを作成して、イベント ハブの名前空間、イベント ハブ名、共有アクセス ポリシー名や主キーを追加するファイルをすべて変更するには、Cloud Shell で組み込みエディターのいずれかを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="60bca-137">You can use one of the built-in editors in the Cloud Shell to modify all the files that make up the application and add your Event Hub namespace, Event Hub name, shared access policy name, and primary key.</span></span> 

<span data-ttu-id="60bca-138">Cloud Shell では **nano**、**vim**、**emacs**、および **code** という名前の Visual Studio Code のようなエディターがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="60bca-138">The Cloud Shell supports **nano**, **vim** and **emacs** as well as a Visual Studio Code-like editor named **code**.</span></span> <span data-ttu-id="60bca-139">目的のエディターの名前を入力するだけで、そのエディターが環境で開始されます。</span><span class="sxs-lookup"><span data-stu-id="60bca-139">Just type the name of the editor you want and it will launch in the environment.</span></span> <span data-ttu-id="60bca-140">次のユニットで **code** エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="60bca-140">We'll use the **code** editor in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="60bca-141">まとめ</span><span class="sxs-lookup"><span data-stu-id="60bca-141">Summary</span></span>

<span data-ttu-id="60bca-142">イベント ハブ環境に関する特定の情報を使用して、送信側アプリケーションと受信側アプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60bca-142">Sender and receiver applications must be configured with specific information about the Event Hub environment.</span></span> <span data-ttu-id="60bca-143">ご利用の受信側アプリケーションによってメッセージを Blob Storage に格納する場合は、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="60bca-143">You create a storage account if your receiver application stores messages in Blob Storage.</span></span> <span data-ttu-id="60bca-144">ご利用のアプリケーションが GitHub 上でホストされている場合は、それをローカル ディレクトリに複製する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60bca-144">If your application is hosted on GitHub, you have to clone it to your local directory.</span></span> <span data-ttu-id="60bca-145">アプリケーションにご自分の名前空間を追加するには、**nano** のようなテキスト エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="60bca-145">Text editors, such as **nano** are used to add your namespace to the application.</span></span>