<span data-ttu-id="507c4-101">イベント ハブに対してパブリッシャー アプリケーションとコンシューマー アプリケーションを構成する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="507c4-101">You're now ready to configure your publisher and consumer applications for your event hub.</span></span>

<span data-ttu-id="507c4-102">この演習では、ご利用のイベント ハブを経由してメッセージを送受信できるようにこれらのアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="507c4-102">In this exercise, you'll configure these applications to send or receive messages through your event hub.</span></span> <span data-ttu-id="507c4-103">これらのアプリケーションは GitHub リポジトリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="507c4-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="507c4-104">2 つの異なるアプリケーションを構成します。1 つはメッセージの送信側として機能し (**SimpleSend**)、もう 1 つはメッセージの受信側として機能します (**EventProcessorSample**)。</span><span class="sxs-lookup"><span data-stu-id="507c4-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="507c4-105">これらは、ブラウザー内ですべてのことができるようにするための Java アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="507c4-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="507c4-106">ただし、.NET など、いずれのプラットフォームについても同じ構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="507c4-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="507c4-107">汎用の Standard ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="507c4-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="507c4-108">このユニットで構成する Java 受信側アプリケーションでは、メッセージが Azure Blob Storage に格納されます。</span><span class="sxs-lookup"><span data-stu-id="507c4-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="507c4-109">Blob Storage にはストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="507c4-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="507c4-110">次のコマンドを使用して、リソース グループにストレージ アカウント (汎用 V2) を作成します。</span><span class="sxs-lookup"><span data-stu-id="507c4-110">Create a storage account (general-purpose V2) in the resource group using the following command:</span></span>

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name>  --location <location> --sku Standard_RAGRS --encryption blob
    ```
    |<span data-ttu-id="507c4-111">パラメーター</span><span class="sxs-lookup"><span data-stu-id="507c4-111">Parameter</span></span>      |<span data-ttu-id="507c4-112">説明</span><span class="sxs-lookup"><span data-stu-id="507c4-112">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="507c4-113">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-113">--name (required)</span></span>  |<span data-ttu-id="507c4-114">ストレージ アカウントの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-114">Enter a name for your storage account.</span></span>|
    |<span data-ttu-id="507c4-115">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-115">--resource-group (required)</span></span>  |<span data-ttu-id="507c4-116">前のユニットで作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-116">Enter the resource group you created in the previous unit.</span></span>|
    |<span data-ttu-id="507c4-117">--location (省略可能)</span><span class="sxs-lookup"><span data-stu-id="507c4-117">--location (optional)</span></span>    |<span data-ttu-id="507c4-118">前のユニットでリソース グループを作成するために使用した場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-118">Enter the location you used to create your resource group in the previous unit.</span></span>|
1. <span data-ttu-id="507c4-119">次のコマンドを使用して、ご利用のストレージ アカウントに関連付けられているアクセス キーをすべて一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="507c4-119">List all the access keys associated with your storage account using the following command:</span></span>

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```
    |<span data-ttu-id="507c4-120">パラメーター</span><span class="sxs-lookup"><span data-stu-id="507c4-120">Parameter</span></span>      |<span data-ttu-id="507c4-121">説明</span><span class="sxs-lookup"><span data-stu-id="507c4-121">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="507c4-122">--account-name (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-122">--account-name (required)</span></span>  |<span data-ttu-id="507c4-123">ストレージ アカウントの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-123">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="507c4-124">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-124">--resource-group (required)</span></span>  |<span data-ttu-id="507c4-125">前のユニットで作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-125">Enter the resource group you created in the previous unit.</span></span>|

     <span data-ttu-id="507c4-126">ご利用のストレージ アカウントに関連付けられているアクセス キーが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="507c4-126">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="507c4-127">今後使用するために**キー**の値をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="507c4-127">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="507c4-128">このキーは、ストレージ アカウントにアクセスする場合に必要となります。</span><span class="sxs-lookup"><span data-stu-id="507c4-128">You'll need this key to access your storage account.</span></span>
1. <span data-ttu-id="507c4-129">次のコマンドを使用して、ご利用のストレージ アカウントの接続文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="507c4-129">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```
    |<span data-ttu-id="507c4-130">パラメーター</span><span class="sxs-lookup"><span data-stu-id="507c4-130">Parameter</span></span>      |<span data-ttu-id="507c4-131">説明</span><span class="sxs-lookup"><span data-stu-id="507c4-131">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="507c4-132">-n (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-132">-n (required)</span></span>  |<span data-ttu-id="507c4-133">ストレージ アカウントの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-133">Enter the name for your storage account.</span></span>|
    |<span data-ttu-id="507c4-134">-g (必須)</span><span class="sxs-lookup"><span data-stu-id="507c4-134">-g (required)</span></span>  |<span data-ttu-id="507c4-135">ご利用のリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="507c4-135">Enter the name of your resource group.</span></span>|

    <span data-ttu-id="507c4-136">このコマンドでは、ストレージ アカウントについて接続の詳細が返されます。</span><span class="sxs-lookup"><span data-stu-id="507c4-136">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="507c4-137">**connectionString** の値をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="507c4-137">Copy and save the value of **connectionString**.</span></span>

1. <span data-ttu-id="507c4-138">次のコマンドを使用してストレージ アカウント内に **messages** と呼ばれるコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="507c4-138">Create a container called **messages** in your storage account using the following command.</span></span> <span data-ttu-id="507c4-139">前の手順でコピーした **connectionString** を使用します。</span><span class="sxs-lookup"><span data-stu-id="507c4-139">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="507c4-140">Event Hubs GitHub リポジトリを複製する</span><span class="sxs-lookup"><span data-stu-id="507c4-140">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="507c4-141">次の手順を使用して Event Hubs GitHub リポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="507c4-141">Use the following steps to clone the Event Hubs GitHub repository.</span></span>

1. <span data-ttu-id="507c4-142">Azure Cloud Shell (Bash) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="507c4-142">Sign in to Azure Cloud Shell (Bash).</span></span>

1. <span data-ttu-id="507c4-143">この演習でビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure/azure-event-hubs)にあります。</span><span class="sxs-lookup"><span data-stu-id="507c4-143">The source files for the applications that you'll build in this exercise are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="507c4-144">次のコマンドを使用して、Cloud Shell でご利用のホーム ディレクトリにいることを確認してから、このリポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="507c4-144">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="507c4-145">リポジトリは `/home/<username>/azure-event-hubs` に複製されます。</span><span class="sxs-lookup"><span data-stu-id="507c4-145">The repository is cloned to `/home/<username>/azure-event-hubs`.</span></span>

## <a name="use-nano-to-edit-simplesendjava"></a><span data-ttu-id="507c4-146">Nano を使用して SimpleSend.java を編集する</span><span class="sxs-lookup"><span data-stu-id="507c4-146">Use nano to edit SimpleSend.java</span></span>

<span data-ttu-id="507c4-147">**nano** エディターを使用して、SimpleSend アプリケーションを編集し、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名、およびプライマリ キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="507c4-147">Use the **nano** editor to edit the SimpleSend application and add your Event Hubs namespace, event hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="507c4-148">エディター ウィンドウの下部に主要なコマンドが表示されます。このユニットでは、Ctrl + O キーを使用して編集内容を書き込み、次に Enter キーを押して出力ファイル名を確認し、さらに Ctrl + X キーを使用してエディターを終了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="507c4-148">The main commands are displayed at the bottom of the editor window; in this unit, you'll need to write out your edits using CTRL +O, and then ENTER to confirm the output file name, and exit the editor using CTRL +X.</span></span>

1. <span data-ttu-id="507c4-149">次のコマンドを使用して **SimpleSend** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-149">Change to the **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="507c4-150">次のコマンドを使用して、**nano** エディター内で **SimpleSend.java** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="507c4-150">Open the **SimpleSend.java** file in the **nano** editor using the following command:</span></span>

    ```azurecli
    nano SimpleSend.java
    ```

1. <span data-ttu-id="507c4-151">nano エディターで、次の文字列を検索し置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-151">In the nano editor, locate and replace the following strings:</span></span>
    - <span data-ttu-id="507c4-152">`"Your Event Hubs namespace name"` をご利用のイベント ハブ名前空間の名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-152">`"Your Event Hubs namespace name"` with the name of your event hub namespace.</span></span>
    - <span data-ttu-id="507c4-153">`"Your event hub"` をご利用のイベント ハブの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-153">`"Your event hub"` with the name of your event hub.</span></span>
    - <span data-ttu-id="507c4-154">`"Your primary SAS key"` を、以前に保存したご利用のイベント ハブ名前空間の **primaryKey** キーの値に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-154">`"Your primary SAS key"` with the value of the **primaryKey** key for your event hub namespace that you saved earlier.</span></span>
    - <span data-ttu-id="507c4-155">`"Your policy name"` を **RootManageSharedAccessKey** に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-155">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
 
        <span data-ttu-id="507c4-156">Event Hubs 名前空間を作成すると、**RootManageSharedAccessKey** と呼ばれる 256 ビットの SAS キーが作成されます。このキーには、送信、リッスン、および管理の権限を名前空間に付与するためのプライマリキーとセカンダリ キーのペアが関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="507c4-156">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="507c4-157">前のユニットでは、Azure CLI コマンドを使用してこのキーを表示しました。このキーは、Azure portal 内でご利用の Event Hubs 名前空間の**共有アクセス ポリシー**に関するページを開くことによって検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="507c4-157">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

    ![送信側アプリケーションでの構成の詳細](../media-draft/5-sender-configure.png)

1. <span data-ttu-id="507c4-159">次のコマンドを使用して **SimpleSend.java** を保存し、nano を終了します。</span><span class="sxs-lookup"><span data-stu-id="507c4-159">Save **SimpleSend.java** using the following command, and exit nano:</span></span>

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="507c4-160">Maven を使用して SimpleSend.java をビルドする</span><span class="sxs-lookup"><span data-stu-id="507c4-160">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="507c4-161">今度は、**mvn** コマンドを使用して Java アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="507c4-161">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="507c4-162">次のコマンドを使用して、メインの **SimpleSend** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-162">Change to the main **SimpleSend** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```
1. <span data-ttu-id="507c4-163">次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="507c4-163">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="507c4-164">これにより、そのアプリケーションではご利用のイベント ハブに対する接続の詳細が確実に使用されるようになります。</span><span class="sxs-lookup"><span data-stu-id="507c4-164">This ensures that your application  uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="507c4-165">ビルド プロセスは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="507c4-165">The build process may take several minutes to complete.</span></span> <span data-ttu-id="507c4-166">次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="507c4-166">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![送信側アプリケーションのビルド結果](../media-draft/5-sender-build.png)

## <a name="use-nano-to-edit-eventprocessorsamplejava"></a><span data-ttu-id="507c4-168">nano を使用して EventProcessorSample.java を編集する</span><span class="sxs-lookup"><span data-stu-id="507c4-168">Use nano to edit EventProcessorSample.java</span></span>

<span data-ttu-id="507c4-169">次にイベント ハブからデータを取り込むことができるように**受信側** (**サブスクライバー**または**コンシューマー**とも呼ばれる) アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="507c4-169">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your event hub.</span></span>

<span data-ttu-id="507c4-170">受信側アプリケーションの場合は、**EventHubReceiver** および **EventProcessorHost** の 2 つのメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="507c4-170">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="507c4-171">EventProcessorHost は EventHubReceiver の上にビルドされますが、そのプログラマティック インターフェイスは EventHubReceiver よりシンプルです。</span><span class="sxs-lookup"><span data-stu-id="507c4-171">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="507c4-172">EventProcessorHost では、同じストレージ アカウントを使用して、EventProcessorHost の複数のインスタンスにメッセージ パーティションを自動的に分散することができます。</span><span class="sxs-lookup"><span data-stu-id="507c4-172">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="507c4-173">このユニットでは、EventProcessorHost メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="507c4-173">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="507c4-174">もう一度 nano を使用します。EventProcessorSample アプリケーションを編集して、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名とプライマリ キー、ストレージ アカウント名、接続文字列、およびコンテナー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="507c4-174">You'll again use nano, and edit the EventProcessorSample application to add your Event Hubs namespace, event hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="507c4-175">次のコマンドを使用して、**EventProcessorSample** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-175">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="507c4-176">次のコマンドを使用して、**nano** エディター内で **EventProcessorSample.java** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="507c4-176">Open the **EventProcessorSample.java** file in the **nano** editor using the following command:</span></span>

    ```azurecli
    nano EventProcessorSample.java
    ```
1. <span data-ttu-id="507c4-177">nano エディターで、次の文字列を検索し置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-177">Locate and replace the following strings in the nano editor:</span></span>
    - <span data-ttu-id="507c4-178">`----ServiceBusNamespaceName----` をご利用の Event Hubs 名前空間の名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-178">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="507c4-179">`----EventHubName----` をご利用のイベント ハブの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-179">`----EventHubName----` with the name of your event hub.</span></span>
    - <span data-ttu-id="507c4-180">`----SharedAccessSignatureKeyName----` を **RootManageSharedAccessKey** に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-180">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="507c4-181">`----SharedAccessSignatureKey----` を、以前に保存したご利用の Event Hubs 名前空間の **primaryKey** キーの値に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-181">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="507c4-182">`----AzureStorageConnectionString----` を以前保存したご利用のストレージ アカウント接続文字列に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-182">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="507c4-183">`----StorageContainerName----` を**メッセージ**に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-183">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="507c4-184">`----HostNamePrefix----` をご利用のストレージ アカウントの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="507c4-184">`----HostNamePrefix----` with the name of your storage account.</span></span>

    ![受信側アプリケーションでの構成の詳細](../media-draft/5-receiver-configure.png)

1. <span data-ttu-id="507c4-186">次のコマンドを使用して **EventProcessorSample.java** を保存し、nano を終了します。</span><span class="sxs-lookup"><span data-stu-id="507c4-186">Save **EventProcessorSample.java** using the following command and exit nano:</span></span>

    ```azurecli
    CTRL +O
    ENTER
    CTRL +X
    ```

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="507c4-187">Maven を使用して EventProcessorSample.java をビルドする</span><span class="sxs-lookup"><span data-stu-id="507c4-187">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="507c4-188">次のコマンドを使用して、メインの **EventProcessorSample** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-188">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="507c4-189">次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="507c4-189">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="507c4-190">これにより、そのアプリケーションではご利用のイベント ハブに対する接続の詳細が確実に使用されるようになります。</span><span class="sxs-lookup"><span data-stu-id="507c4-190">This ensures that your application uses the connection details for your event hub:</span></span>

    ```azurecli
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="507c4-191">ビルド プロセスは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="507c4-191">The build process may take several minutes to complete.</span></span> <span data-ttu-id="507c4-192">次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="507c4-192">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![受信側アプリケーションのビルド結果](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="507c4-194">送信側アプリと受信側アプリを起動する</span><span class="sxs-lookup"><span data-stu-id="507c4-194">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="507c4-195">コマンドラインから Java アプリケーションを実行するには、**java** コマンドを使用し、.jar パッケージを指定します。</span><span class="sxs-lookup"><span data-stu-id="507c4-195">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="507c4-196">次のコマンドを使用して、SimpleSend アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-196">Use the following commands to start the SimpleSend application:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="507c4-197">**Send Complete...** と表示されたら、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="507c4-197">When you see **Send Complete...**, press ENTER.</span></span>

    ![送信側アプリケーションの実行結果](../media-draft/5-sender-run.png)

1. <span data-ttu-id="507c4-199">次のコマンドを使用して、EventProcessorSample アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="507c4-199">Start the EventProcessorSample application using the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. <span data-ttu-id="507c4-200">メッセージがコンソールに表示されなくなったら、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="507c4-200">When messages stop being displayed to the console, press ENTER.</span></span>

    ![受信側アプリケーションの実行結果](../media-draft/5-receiver-run.png)

## <a name="summary"></a><span data-ttu-id="507c4-202">まとめ</span><span class="sxs-lookup"><span data-stu-id="507c4-202">Summary</span></span>

<span data-ttu-id="507c4-203">これで、ご利用のイベント ハブにメッセージを送信できるように送信側アプリケーションが構成されました。</span><span class="sxs-lookup"><span data-stu-id="507c4-203">You've now configured a sender application ready to send messages to your event hub.</span></span> <span data-ttu-id="507c4-204">また、ご利用のイベント ハブからメッセージを受信できるように受信側アプリケーションも構成されました。</span><span class="sxs-lookup"><span data-stu-id="507c4-204">You've also configured a receiver application ready to receive messages from your event hub.</span></span>
