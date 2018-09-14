<span data-ttu-id="327ae-101">イベント ハブを作成して構成したら、イベント データ ストリームの送受信ができるようにアプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-101">After you have created and configured your event hub, you'll need to configure applications to send and receive event data streams.</span></span>

<span data-ttu-id="327ae-102">たとえば、支払い処理ソリューションでは、顧客のクレジット カードのデータを収集するために何らかの形式の送信側アプリケーションが使用され、そのクレジット カードが有効であることを確認するために受信側アプリケーションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="327ae-102">For example, a payment processing solution will use some form of sender application to collect customer's credit card data and a receiver application to verify that the credit card is valid.</span></span>

<span data-ttu-id="327ae-103">.NET アプリケーションの場合と比較して、Java アプリケーションの構成方法には違いがありますが、アプリケーションをイベント ハブに接続し、アプリケーションによってメッセージの送受信を正常に行えるようにするには一般的な原則に従います。</span><span class="sxs-lookup"><span data-stu-id="327ae-103">Although there are differences in how a Java application is configured, compared to a .NET application, there are general principles for enabling applications to connect to an event hub, and to successfully send or receive messages.</span></span> <span data-ttu-id="327ae-104">そのため、Java 構成テキスト ファイルの編集プロセスは、Visual Studio を使用して .NET アプリケーションを準備するのとは異なりますが、原則は同じです。</span><span class="sxs-lookup"><span data-stu-id="327ae-104">So, although the process of editing Java configuration text files is different to preparing a .NET application using Visual Studio, the principles are the same.</span></span>

## <a name="what-are-the-minimum-event-hub-application-requirements"></a><span data-ttu-id="327ae-105">イベント ハブ アプリケーションの最小要件とは?</span><span class="sxs-lookup"><span data-stu-id="327ae-105">What are the minimum Event Hub application requirements?</span></span>

<span data-ttu-id="327ae-106">イベント ハブにメッセージを送信できるようにアプリケーションを構成するには、次の情報を指定して、アプリケーションによって接続の資格情報が作成されるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-106">To configure an application to send messages to an event hub, you must provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="327ae-107">イベント ハブ名前空間の名前</span><span class="sxs-lookup"><span data-stu-id="327ae-107">Event hub namespace name</span></span>
- <span data-ttu-id="327ae-108">イベント ハブ名</span><span class="sxs-lookup"><span data-stu-id="327ae-108">Event hub name</span></span>
- <span data-ttu-id="327ae-109">共有アクセス ポリシー名</span><span class="sxs-lookup"><span data-stu-id="327ae-109">Shared access policy name</span></span>
- <span data-ttu-id="327ae-110">プライマリ共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="327ae-110">Primary shared access key</span></span>

<span data-ttu-id="327ae-111">イベント ハブからメッセージを受信できるようにアプリケーションを構成するには、次の情報を指定して、アプリケーションによって接続の資格情報が作成されるようにします。</span><span class="sxs-lookup"><span data-stu-id="327ae-111">To configure an application to receive messages from an event hub, provide the following information, so that the application can create connection credentials:</span></span>

- <span data-ttu-id="327ae-112">イベント ハブ名前空間の名前</span><span class="sxs-lookup"><span data-stu-id="327ae-112">Event hub namespace name</span></span>
- <span data-ttu-id="327ae-113">イベント ハブ名</span><span class="sxs-lookup"><span data-stu-id="327ae-113">Event hub name</span></span>
- <span data-ttu-id="327ae-114">共有アクセス ポリシー名</span><span class="sxs-lookup"><span data-stu-id="327ae-114">Shared access policy name</span></span>
- <span data-ttu-id="327ae-115">プライマリ共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="327ae-115">Primary shared access key</span></span>
- <span data-ttu-id="327ae-116">ストレージ アカウント名</span><span class="sxs-lookup"><span data-stu-id="327ae-116">Storage account name</span></span>
- <span data-ttu-id="327ae-117">ストレージ アカウントの接続文字列</span><span class="sxs-lookup"><span data-stu-id="327ae-117">Storage account connection string</span></span>
- <span data-ttu-id="327ae-118">ストレージ アカウントのコンテナー名</span><span class="sxs-lookup"><span data-stu-id="327ae-118">Storage account container name</span></span>

<span data-ttu-id="327ae-119">Azure Blob Storage 内にメッセージを格納する受信側アプリケーションがある場合は、最初にストレージ アカウントも構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-119">If you have a receiver application that stores messages in Azure Blob Storage, you'll also need to first configure a storage account.</span></span>

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a><span data-ttu-id="327ae-120">汎用の Standard ストレージ アカウントを作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="327ae-120">The Azure CLI commands for creating a general-purpose standard storage account</span></span>

1. <span data-ttu-id="327ae-121">ご利用のリソース グループ内で、およびそのリソース グループを作成するときに使用したのと同じ Azure データセンターの場所内で、ストレージ アカウントを作成します (汎用 V2)。</span><span class="sxs-lookup"><span data-stu-id="327ae-121">Create the Storage account (general-purpose V2) in your resource group, and the same Azure datacenter location that you used when creating the resource group.</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```

1. <span data-ttu-id="327ae-122">このストレージにアクセスするには、ストレージ アカウントのアクセス キーが必要です。</span><span class="sxs-lookup"><span data-stu-id="327ae-122">To access this storage, you'll need a storage account access key.</span></span> <span data-ttu-id="327ae-123">ストレージ アカウントに関連付けられているアクセス キーを表示します。</span><span class="sxs-lookup"><span data-stu-id="327ae-123">View the access keys that are associated with the storage account.</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

1. <span data-ttu-id="327ae-124">**key1** に関連付けられている値を保存します。</span><span class="sxs-lookup"><span data-stu-id="327ae-124">Save the value associated with **key1**.</span></span>

1. <span data-ttu-id="327ae-125">また、ストレージ アカウントに関する接続の詳細も必要になります。</span><span class="sxs-lookup"><span data-stu-id="327ae-125">You will also need the connection details for the storage account.</span></span> <span data-ttu-id="327ae-126">ストレージ アカウント用の接続文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="327ae-126">View the connection string for the storage account.</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

1. <span data-ttu-id="327ae-127">**connectionString** に関連付けられている値を保存します。</span><span class="sxs-lookup"><span data-stu-id="327ae-127">Save the value associated with the **connectionString**.</span></span>

1. <span data-ttu-id="327ae-128">ご利用のストレージ アカウント内のコンテナーにメッセージが格納されるようになります。</span><span class="sxs-lookup"><span data-stu-id="327ae-128">Messages will be stored in a container within your storage account.</span></span> <span data-ttu-id="327ae-129">前の手順からの接続文字列を含む `<connection string>` を使用して、ストレージ アカウント内でコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="327ae-129">Create a container in your storage account, using `<connection string>` with the connection string from the previous step.</span></span>

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a><span data-ttu-id="327ae-130">アプリケーションの GitHub リポジトリを複製するためのシェル コマンド</span><span class="sxs-lookup"><span data-stu-id="327ae-130">Shell command for cloning an application GitHub repository</span></span>

<span data-ttu-id="327ae-131">Git は分散型バージョン管理モデルを使用するコラボレーション ツールであり、ソフトウェア プロジェクトやドキュメント プロジェクトでの共同作業向けに設計されています。</span><span class="sxs-lookup"><span data-stu-id="327ae-131">Git is a collaboration tool that uses a distributed version control model, and is designed for collaborative working on software and documentation projects.</span></span> <span data-ttu-id="327ae-132">Windows を含む複数のプラットフォームで Git クライアントを利用することができ、Git コマンドラインは Azure Bash Cloud Shell に含まれています。</span><span class="sxs-lookup"><span data-stu-id="327ae-132">Git clients are available for multiple platforms, including Windows, and the Git command line is included in the Azure Bash cloud shell.</span></span> <span data-ttu-id="327ae-133">GitHub は Git リポジトリに対する Web ベースのホスティング サービスです。</span><span class="sxs-lookup"><span data-stu-id="327ae-133">GitHub is a web-based hosting service for Git repositories.</span></span> 

<span data-ttu-id="327ae-134">GitHub 内でプロジェクトとしてホストされているアプリケーションがある場合は、**git clone** コマンドを使用してプロジェクトのリポジトリを複製することにより、プロジェクトのローカル コピーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="327ae-134">If you have an application that is hosted as a project in GitHub, you can make a local copy of the project, by cloning its repository using the **git clone** command.</span></span>

1. <span data-ttu-id="327ae-135">ホーム ディレクトリにリポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="327ae-135">Clone a repository to your home directory.</span></span>

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a><span data-ttu-id="327ae-136">アプリケーションを準備するためのシェル コマンド</span><span class="sxs-lookup"><span data-stu-id="327ae-136">Shell commands for preparing an application</span></span>

1. <span data-ttu-id="327ae-137">**nano** などのエディターを使用して、アプリケーションを編集し、ご利用のイベント ハブ名前空間、イベント ハブ名、共有アクセス ポリシー名、およびプライマリ キーを追加できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="327ae-137">You can now use an editor, such as **nano** to edit the application and add your event hub namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="327ae-138">Nano は Linux や他の Unix 系のオペレーティング システムで使用できるシンプルなテキスト エディターです。Azure Bash Cloud Shell 内で使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="327ae-138">Nano is a simple text editor available for Linux and other Unix-like operating systems; it is also available in the Azure Bash cloud shell.</span></span> <span data-ttu-id="327ae-139">たとえば、**nano** エディターでは次のコマンドを使用して Java アプリケーション ファイルを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="327ae-139">For example, use this command to open a Java application file in the **nano** editor.</span></span>

    ```azurecli
    nano <application>.java
    ```

1. <span data-ttu-id="327ae-140">アプリケーションの開発方法によっては、イベント ハブの構成情報を追加した後でアプリケーションをコンパイルまたはビルドすることが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-140">Depending on how your applications are developed, you may need to compile or build the application after you've added the event hub configuration information.</span></span> <span data-ttu-id="327ae-141">たとえば、次のユニットで使用する Java アプリケーションについては、Apache **Maven** などのツールを使用してビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-141">For example, the Java applications used in the next unit need to be built using a tool such as Apache **Maven**.</span></span> <span data-ttu-id="327ae-142">Maven では、.java ファイルが .class ファイルにコンパイルされるか、または .java ファイルが .jar ファイルにパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="327ae-142">Maven compiles .java files into .class files or packages them into .jar files.</span></span> <span data-ttu-id="327ae-143">Maven は Bash Cloud Shell から利用することができます。Java アプリケーションをビルドするには、**mvn** コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="327ae-143">Maven is available from the Bash cloud shell; to build the Java application, you use **mvn** commands.</span></span> <span data-ttu-id="327ae-144">Java アプリケーションをビルドするには、次の mvn コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="327ae-144">Use this mvn command to build a Java  application.</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a><span data-ttu-id="327ae-145">まとめ</span><span class="sxs-lookup"><span data-stu-id="327ae-145">Summary</span></span>

<span data-ttu-id="327ae-146">イベント ハブ環境に関する特定の情報を使用して、送信側アプリケーションと受信側アプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-146">Sender and receiver applications must be configured with specific information about the event hub environment.</span></span> <span data-ttu-id="327ae-147">ご利用の受信側アプリケーションによってメッセージを Blob Storage に格納する場合は、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="327ae-147">You create a storage account if your receiver application stores messages in Blob Storage.</span></span> <span data-ttu-id="327ae-148">ご利用のアプリケーションが GitHub 上でホストされている場合は、それをローカル ディレクトリに複製する必要があります。</span><span class="sxs-lookup"><span data-stu-id="327ae-148">If your application is hosted on GitHub, you have to clone it to your local directory.</span></span> <span data-ttu-id="327ae-149">アプリケーションに名前空間を追加するには、**nano** のようなテキスト エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="327ae-149">Text editors, such as **nano** is used to  add your namespace to the application.</span></span>