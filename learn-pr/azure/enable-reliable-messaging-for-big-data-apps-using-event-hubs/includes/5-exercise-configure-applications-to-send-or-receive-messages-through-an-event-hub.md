<span data-ttu-id="f63d1-101">イベント ハブに対してパブリッシャー アプリケーションとコンシューマー アプリケーションを構成する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="f63d1-101">You're now ready to configure your publisher and consumer applications for your Event Hub.</span></span>

<span data-ttu-id="f63d1-102">このユニットでは、ご利用のイベント ハブを経由してメッセージを送受信するようにこれらのアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-102">In this unit, you'll configure these applications to send or receive messages through your Event Hub.</span></span> <span data-ttu-id="f63d1-103">これらのアプリケーションは GitHub リポジトリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-103">These applications are stored in a GitHub repository.</span></span>

<span data-ttu-id="f63d1-104">2 つの異なるアプリケーションを構成します。1 つはメッセージの送信側として機能し (**SimpleSend**)、もう 1 つはメッセージの受信側として機能します (**EventProcessorSample**)。</span><span class="sxs-lookup"><span data-stu-id="f63d1-104">You'll configure two separate applications; one acts as the message sender (**SimpleSend**), the other as the message receiver (**EventProcessorSample**).</span></span> <span data-ttu-id="f63d1-105">これらは、ブラウザー内ですべてのことができるようにするための Java アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="f63d1-105">These are Java applications, which enable you to do everything within the browser.</span></span> <span data-ttu-id="f63d1-106">ただし、.NET など、いずれのプラットフォームについても同じ構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="f63d1-106">However, the same configuration is needed for any platform, such as .NET.</span></span>

## <a name="create-a-general-purpose-standard-storage-account"></a><span data-ttu-id="f63d1-107">汎用の Standard ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="f63d1-107">Create a general-purpose, standard storage account</span></span>

<span data-ttu-id="f63d1-108">このユニットで構成する Java 受信側アプリケーションでは、メッセージが Azure Blob Storage に格納されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-108">The Java receiver application, that you'll configure in this unit, stores messages in Azure Blob Storage.</span></span> <span data-ttu-id="f63d1-109">Blob Storage にはストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="f63d1-109">Blob Storage requires a storage account.</span></span>

1. <span data-ttu-id="f63d1-110">`storage account create` コマンドを利用し、ストレージ アカウント (汎用 V2) を作成します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-110">Create a storage account (general-purpose V2) using the `storage account create` command.</span></span> <span data-ttu-id="f63d1-111">既定のリソース グループと場所を設定したことを思い出してください。通常、これらのパラメーターは_必須_ですが、オフのままにしてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="f63d1-111">Remember we set a default resource group and location, so even though those parameters are normally _required_, we can leave them off.</span></span>

    |<span data-ttu-id="f63d1-112">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f63d1-112">Parameter</span></span>      |<span data-ttu-id="f63d1-113">説明</span><span class="sxs-lookup"><span data-stu-id="f63d1-113">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="f63d1-114">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="f63d1-114">--name (required)</span></span>  | <span data-ttu-id="f63d1-115">ストレージ アカウントの名前。</span><span class="sxs-lookup"><span data-stu-id="f63d1-115">A name for your storage account.</span></span> |
    |<span data-ttu-id="f63d1-116">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="f63d1-116">--resource-group (required)</span></span>  |<span data-ttu-id="f63d1-117">リソース グループの所有者。</span><span class="sxs-lookup"><span data-stu-id="f63d1-117">The resource group owner.</span></span> <span data-ttu-id="f63d1-118">事前に作成した Sandbox リソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-118">We'll use the pre-created Sandbox resource group.</span></span>|
    |<span data-ttu-id="f63d1-119">--location (省略可能)</span><span class="sxs-lookup"><span data-stu-id="f63d1-119">--location (optional)</span></span>    |<span data-ttu-id="f63d1-120">特定の場所でストレージ アカウントを必要とする場合の、リソース グループの場所ではない任意の場所。</span><span class="sxs-lookup"><span data-stu-id="f63d1-120">An optional location if you want the storage account in a specific place vs. the resource group location.</span></span>|

    <span data-ttu-id="f63d1-121">ストレージ アカウント名を変数に設定します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-121">Set the storage account name into a variable.</span></span> <span data-ttu-id="f63d1-122">小文字と数字だけで作成し、区切り記号にはハイフンを使用します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-122">It must be composed of all lower-case letters, numbers, with hyphen separators allowed.</span></span> <span data-ttu-id="f63d1-123">また、Azure 内で一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-123">It also must be unique within Azure.</span></span>

    ```azurecli
    STORAGE_NAME=[name]
    ```

    <span data-ttu-id="f63d1-124">次に、このコマンドを使用し、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-124">Then use this command to create the storage account.</span></span>

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > <span data-ttu-id="f63d1-125">ストレージ アカウントを作成できなかった場合、環境変数を変更してもう一度お試しください。</span><span class="sxs-lookup"><span data-stu-id="f63d1-125">If the storage account creation fails, change your environment variable and try again.</span></span>

1. <span data-ttu-id="f63d1-126">`account keys list` コマンドを使用し、ご利用のストレージ アカウントに関連付けられているアクセス キーをすべて一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-126">List all the access keys associated with your storage account using the `account keys list` command.</span></span> <span data-ttu-id="f63d1-127">アカウント名とリソース グループ (既定) が必要です。</span><span class="sxs-lookup"><span data-stu-id="f63d1-127">It takes your account name and the resource group (which is defaulted).</span></span>

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     <span data-ttu-id="f63d1-128">ご利用のストレージ アカウントに関連付けられているアクセス キーが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-128">Access keys associated with your storage account are listed.</span></span> <span data-ttu-id="f63d1-129">今後使用するために**キー**の値をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-129">Copy and save the value of **key** for future use.</span></span> <span data-ttu-id="f63d1-130">このキーは、ストレージ アカウントにアクセスする場合に必要となります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-130">You'll need this key to access your storage account.</span></span>

1. <span data-ttu-id="f63d1-131">次のコマンドを使用し、ご利用のストレージ アカウントの接続文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-131">View the connections string for your storage account using the following command:</span></span>

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    <span data-ttu-id="f63d1-132">このコマンドでは、ストレージ アカウントに関する接続の詳細が返されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-132">This command returns the connection details for the storage account.</span></span> <span data-ttu-id="f63d1-133">**connectionString** の_値_をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-133">Copy and save the _value_ of **connectionString**.</span></span> <span data-ttu-id="f63d1-134">次のようなものになります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-134">It should look something like:</span></span>

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. 次のコマンドを使用してストレージ アカウント内に **messages** と呼ばれるコンテナーを作成します。 <span data-ttu-id="f63d1-136">前の手順でコピーした **connectionString** を使用します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-136">Use the **connectionString** you copied in the previous step:</span></span>

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a><span data-ttu-id="f63d1-137">Event Hubs GitHub リポジトリを複製する</span><span class="sxs-lookup"><span data-stu-id="f63d1-137">Clone the Event Hubs GitHub repository</span></span>

<span data-ttu-id="f63d1-138">次の手順を使用し、`git` で Event Hubs GitHub リポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-138">Use the following steps to clone the Event Hubs GitHub repository with `git`.</span></span> <span data-ttu-id="f63d1-139">Cloud Shell でこの権限を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-139">You can execute this right in the Cloud Shell.</span></span>

1. <span data-ttu-id="f63d1-140">このユニットでビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure/azure-event-hubs)にあります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-140">The source files for the applications that you'll build in this unit are located in a [GitHub repository](https://github.com/Azure/azure-event-hubs).</span></span> <span data-ttu-id="f63d1-141">次のコマンドを使用し、Cloud Shell で現在のディレクトリがホーム ディレクトリになっていることを確認してからこのリポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-141">Use the following commands to make sure that you are in your home directory in Cloud Shell, and then to clone this repository:</span></span>

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    <span data-ttu-id="f63d1-142">リポジトリがホーム フォルダーに複製されます</span><span class="sxs-lookup"><span data-stu-id="f63d1-142">The repository is cloned to your home folder.</span></span>

## <a name="edit-simplesendjava"></a><span data-ttu-id="f63d1-143">SimpleSend.java を編集する</span><span class="sxs-lookup"><span data-stu-id="f63d1-143">Edit SimpleSend.java</span></span>

<span data-ttu-id="f63d1-144">組み込みの Cloud Shell Code エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-144">We're going to use the built-in Cloud Shell Code editor.</span></span> <span data-ttu-id="f63d1-145">これは Monaco エディターに基づいており、Visual Studio Code に似ていますが、完全にオンラインです。</span><span class="sxs-lookup"><span data-stu-id="f63d1-145">This is based on the Monaco editor and is similar to Visual Studio Code, but completely online.</span></span>

<span data-ttu-id="f63d1-146">このエディターを使用して SimpleSend アプリケーションを変更し、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名、プライマリ キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-146">We'll use the editor to modify the SimpleSend application and add your Event Hubs namespace, Event Hub name, shared access policy name, and primary key.</span></span> <span data-ttu-id="f63d1-147">エディター ウィンドウの下部に主要なコマンドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-147">The main commands are displayed at the bottom of the editor window.</span></span> 

<span data-ttu-id="f63d1-148"><kbd>Ctrl + O</kbd> キーを使用して編集内容を書き込み、次に <kbd>Enter</kbd> キーを押して出力ファイル名を確定し、<kbd>Ctrl + X</kbd> キーを使用してエディターを終了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-148">You'll need to write out your edits using <kbd>Ctrl+O</kbd>, and then <kbd>ENTER</kbd> to confirm the output file name, and exit the editor using <kbd>Ctrl+X</kbd>.</span></span> <span data-ttu-id="f63d1-149">あるいは、エディターの右上隅にある "..." メニューからすべての編集コマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-149">Alternatively, the editor has a "..." menu in the top/right corner for all the editing commands.</span></span>

1. <span data-ttu-id="f63d1-150">**SimpleSend** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-150">Change to the **SimpleSend** folder.</span></span>

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. <span data-ttu-id="f63d1-151">現在のフォルダーでコード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-151">Open the code editor in the current folder.</span></span> <span data-ttu-id="f63d1-152">左側にファイルの一覧が、右側にエディター スペースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-152">This will show a list of files on the left and an editor space on the right.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="f63d1-153">ファイルの一覧から **SimpleSend.java** ファイルを選択して開きます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-153">Open the **SimpleSend.java** file by selecting it from the file list.</span></span>

1. <span data-ttu-id="f63d1-154">エディターで、次の文字列を検索して置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-154">In the editor, locate and replace the following strings:</span></span>

    - <span data-ttu-id="f63d1-155">`"Your Event Hubs namespace name"` をご利用のイベント ハブ名前空間の名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-155">`"Your Event Hubs namespace name"` with the name of your Event Hub namespace.</span></span>
    - <span data-ttu-id="f63d1-156">`"Your Event Hub"` をご利用のイベント ハブの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-156">`"Your Event Hub"` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="f63d1-157">`"Your policy name"` を **RootManageSharedAccessKey** に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-157">`"Your policy name"` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="f63d1-158">`"Your primary SAS key"` を、以前に保存したご利用のイベント ハブ名前空間の **primaryKey** キーの値に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-158">`"Your primary SAS key"` with the value of the **primaryKey** key for your Event Hub namespace that you saved earlier.</span></span>
 
    > [!TIP]
    > <span data-ttu-id="f63d1-159">ターミナル ウィンドウとは異なり、このエディターではお使いの OS で一般的なコピー/貼り付けキーボード アクセラレータ キーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-159">Unlike the terminal window, the editor can use typical copy/paste keyboard accelerator keys for your OS.</span></span>

    <span data-ttu-id="f63d1-160">キーを思い出せない場合、エディターの下にあるターミナル ウィンドウに切り替え、`echo` コマンドで環境変数の 1 つを一覧表示できます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-160">If you've forgotten some of them, you can switch down to the terminal window below the editor and use the `echo` command to list out one of the environment variables.</span></span> <span data-ttu-id="f63d1-161">例:</span><span class="sxs-lookup"><span data-stu-id="f63d1-161">For example:</span></span>

    ```bash
    echo $NS_NAME
    ```
    <span data-ttu-id="f63d1-162">Event Hubs 名前空間を作成すると、**RootManageSharedAccessKey** と呼ばれる 256 ビットの SAS キーが作成されます。このキーには、送信、リッスン、管理の権限を名前空間に付与するためのプライマリキーとセカンダリ キーのペアが関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="f63d1-162">When you create an Event Hubs namespace, a 256-bit SAS key called **RootManageSharedAccessKey** is created that has an associated pair of primary and secondary keys that grant send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="f63d1-163">前のユニットでは、Azure CLI コマンドを使用してキーを表示しました。このキーは、Azure portal 内でご利用の Event Hubs 名前空間の**共有アクセス ポリシー**に関するページを開くことによって検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-163">In the previous unit, you displayed the key using an Azure CLI command, and you can also find this key by opening the **Shared access policies** page for your Event Hubs namespace in the Azure portal.</span></span>

1. <span data-ttu-id="f63d1-164">[...] メニューまたはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>) を使用して、**SimpleSend.java** を保存します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-164">Save **SimpleSend.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="f63d1-165">"..." メニューまたはアクセラレータ キー <kbd>CTRL + Q</kbd> でエディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-165">Close the editor through the "..." menu, or the accelerator key <kbd>CTRL+Q</kbd>.</span></span>

## <a name="use-maven-to-build-simplesendjava"></a><span data-ttu-id="f63d1-166">Maven を使用して SimpleSend.java をビルドする</span><span class="sxs-lookup"><span data-stu-id="f63d1-166">Use Maven to build SimpleSend.java</span></span>

<span data-ttu-id="f63d1-167">次に、**mvn** コマンドを使用して Java アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f63d1-167">You'll now build the Java application using **mvn** commands.</span></span>

1. <span data-ttu-id="f63d1-168">メインの **SimpleSend** フォルダーに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-168">Change back to the main **SimpleSend** folder.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. <span data-ttu-id="f63d1-169">Java SimpleSend アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f63d1-169">Build the Java SimpleSend application.</span></span> <span data-ttu-id="f63d1-170">これにより、イベント ハブの接続に指定した詳細設定がアプリケーションで確実に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-170">This ensures that your application  uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="f63d1-171">ビルド プロセスは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-171">The build process may take several minutes to complete.</span></span> <span data-ttu-id="f63d1-172">次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-172">Ensure that you see the **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![送信側アプリケーションのビルド結果](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a><span data-ttu-id="f63d1-174">EventProcessorSample.java を編集する</span><span class="sxs-lookup"><span data-stu-id="f63d1-174">Edit EventProcessorSample.java</span></span>

<span data-ttu-id="f63d1-175">次に、イベント ハブからデータを取り込むように**受信側** (**サブスクライバー**または**コンシューマー**とも呼ばれる) アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-175">You'll now configure a **receiver** (also known as **subscribers** or **consumers**) application to ingest data from your Event Hub.</span></span>

<span data-ttu-id="f63d1-176">受信側アプリケーションの場合は、**EventHubReceiver** と **EventProcessorHost** の 2 つのメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-176">For the receiver application, two methods are available; **EventHubReceiver** and **EventProcessorHost**.</span></span> <span data-ttu-id="f63d1-177">EventProcessorHost は EventHubReceiver の上にビルドされますが、そのプログラマティック インターフェイスは EventHubReceiver よりシンプルです。</span><span class="sxs-lookup"><span data-stu-id="f63d1-177">EventProcessorHost is built on top of EventHubReceiver, but provides simpler programmatic interface than EventHubReceiver.</span></span> <span data-ttu-id="f63d1-178">EventProcessorHost では、同じストレージ アカウントを使用して、EventProcessorHost の複数のインスタンスにメッセージ パーティションを自動的に分散することができます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-178">EventProcessorHost can automatically distribute message partitions across multiple instances of EventProcessorHost using the same storage account.</span></span>

<span data-ttu-id="f63d1-179">このユニットでは、EventProcessorHost メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-179">In this unit, you’ll use the EventProcessorHost method.</span></span> <span data-ttu-id="f63d1-180">EventProcessorSample アプリケーションを編集し、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名とプライマリ キー、ストレージ アカウント名、接続文字列、コンテナー名を追加します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-180">You'll edit the EventProcessorSample application to add your Event Hubs namespace, Event Hub name, shared access policy name and primary key, storage account name, connection string, and container name.</span></span>

1. <span data-ttu-id="f63d1-181">次のコマンドを使用し、**EventProcessorSample** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-181">Change to the **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. <span data-ttu-id="f63d1-182">コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-182">Open the code editor.</span></span>

    ```bash
    code .
    ```
    
1. <span data-ttu-id="f63d1-183">**EventProcessorSample.java** ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-183">Select the **EventProcessorSample.java** file.</span></span>

1. <span data-ttu-id="f63d1-184">エディターで次の文字列を検索して、次のように置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-184">Locate and replace the following strings in the editor:</span></span>

    - <span data-ttu-id="f63d1-185">`----ServiceBusNamespaceName----` をご利用の Event Hubs 名前空間の名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-185">`----ServiceBusNamespaceName----` with the name of your Event Hubs namespace.</span></span>
    - <span data-ttu-id="f63d1-186">`----EventHubName----` をご利用のイベント ハブの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-186">`----EventHubName----` with the name of your Event Hub.</span></span>
    - <span data-ttu-id="f63d1-187">`----SharedAccessSignatureKeyName----` を **RootManageSharedAccessKey** に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-187">`----SharedAccessSignatureKeyName----` with **RootManageSharedAccessKey**.</span></span>
    - <span data-ttu-id="f63d1-188">`----SharedAccessSignatureKey----` を、以前に保存したご利用の Event Hubs 名前空間の **primaryKey** キーの値に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-188">`----SharedAccessSignatureKey----` with the value of the **primaryKey** key for your Event Hubs namespace that you saved earlier.</span></span>
    - <span data-ttu-id="f63d1-189">`----AzureStorageConnectionString----` を以前保存したご利用のストレージ アカウント接続文字列に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-189">`----AzureStorageConnectionString----` with your storage account connection string that you saved earlier.</span></span>
    - <span data-ttu-id="f63d1-190">`----StorageContainerName----` を**メッセージ**に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-190">`----StorageContainerName----` with **messages**.</span></span>
    - <span data-ttu-id="f63d1-191">`----HostNamePrefix----` をご利用のストレージ アカウントの名前に置換します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-191">`----HostNamePrefix----` with the name of your storage account.</span></span>

1. <span data-ttu-id="f63d1-192">[...] メニュー、またはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>) を使用し、**EventProcessorSample.java** を保存します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-192">Save **EventProcessorSample.java** either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="f63d1-193">エディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-193">Close the editor.</span></span>

## <a name="use-maven-to-build-eventprocessorsamplejava"></a><span data-ttu-id="f63d1-194">Maven を使用して EventProcessorSample.java をビルドする</span><span class="sxs-lookup"><span data-stu-id="f63d1-194">Use Maven to build EventProcessorSample.java</span></span>

1. <span data-ttu-id="f63d1-195">次のコマンドを使用して、メインの **EventProcessorSample** フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-195">Change to the main **EventProcessorSample** folder using the following command:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. <span data-ttu-id="f63d1-196">次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f63d1-196">Build the Java SimpleSend application using the following command.</span></span> <span data-ttu-id="f63d1-197">これにより、イベント ハブの接続に指定した詳細設定がアプリケーションで確実に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f63d1-197">This ensures that your application uses the connection details for your Event Hub:</span></span>

    ```bash
    mvn clean package -DskipTests
    ```

    <span data-ttu-id="f63d1-198">ビルド プロセスは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f63d1-198">The build process may take several minutes to complete.</span></span> <span data-ttu-id="f63d1-199">次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-199">Ensure that you see a **[INFO] BUILD SUCCESS** message before continuing.</span></span>

    ![受信側アプリケーションのビルド結果](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a><span data-ttu-id="f63d1-201">送信側アプリと受信側アプリを起動する</span><span class="sxs-lookup"><span data-stu-id="f63d1-201">Start the sender and receiver apps</span></span>

1. <span data-ttu-id="f63d1-202">コマンドラインから Java アプリケーションを実行するには、**java** コマンドを使用し、.jar パッケージを指定します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-202">Run Java application from the command line by using the **java** command, and specifying a .jar package.</span></span> <span data-ttu-id="f63d1-203">次のコマンドを使用して SimpleSend アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-203">Use the following commands to start the SimpleSend application:</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="f63d1-204">"**Send Complete...**" と表示されたら、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-204">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. <span data-ttu-id="f63d1-205">次のコマンドを使用して EventProcessorSample アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-205">Start the EventProcessorSample application using the following command.</span></span>

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="f63d1-206">メッセージがコンソールに表示されなくなったら、<kbd>ENTER</kbd> キーを押すか、<kbd>CTRL + C</kbd> キーを押してプログラムを終了します。</span><span class="sxs-lookup"><span data-stu-id="f63d1-206">When messages stop being displayed to the console, press <kbd>ENTER</kbd> or <kbd>CTRL+C</kbd> to end the program.</span></span>

    ```output
    ...
    SAMPLE: Partition 0 checkpointing at 1064,19
    SAMPLE (3,1120,20): "Message 80"
    SAMPLE (3,1176,21): "Message 84"
    SAMPLE (3,1232,22): "Message 88"
    SAMPLE (3,1288,23): "Message 92"
    SAMPLE (3,1344,24): "Message 96"
    SAMPLE: Partition 3 checkpointing at 1344,24
    SAMPLE (2,1120,20): "Message 83"
    SAMPLE (2,1176,21): "Message 87"
    SAMPLE (2,1232,22): "Message 91"
    SAMPLE (2,1288,23): "Message 95"
    SAMPLE (2,1344,24): "Message 99"
    SAMPLE: Partition 2 checkpointing at 1344,24
    SAMPLE: Partition 1 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE (0,1120,20): "Message 81"
    SAMPLE (0,1176,21): "Message 85"
    SAMPLE: Partition 0 batch size was 10 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 got event batch
    SAMPLE (0,1232,22): "Message 89"
    SAMPLE (0,1288,23): "Message 93"
    SAMPLE (0,1344,24): "Message 97"
    SAMPLE: Partition 0 checkpointing at 1344,24
    SAMPLE: Partition 3 batch size was 8 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 2 batch size was 9 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    ```

## <a name="summary"></a><span data-ttu-id="f63d1-207">まとめ</span><span class="sxs-lookup"><span data-stu-id="f63d1-207">Summary</span></span>

<span data-ttu-id="f63d1-208">これで、ご利用のイベント ハブにメッセージを送信できるように送信側アプリケーションが構成されました。</span><span class="sxs-lookup"><span data-stu-id="f63d1-208">You've now configured a sender application ready to send messages to your Event Hub.</span></span> <span data-ttu-id="f63d1-209">また、ご利用のイベント ハブからメッセージを受信できるように受信側アプリケーションが構成されました。</span><span class="sxs-lookup"><span data-stu-id="f63d1-209">You've also configured a receiver application ready to receive messages from your Event Hub.</span></span>