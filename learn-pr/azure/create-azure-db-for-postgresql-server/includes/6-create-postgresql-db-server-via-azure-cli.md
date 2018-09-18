<span data-ttu-id="7d1a9-101">ランナーのフィットネス デバイスからキャプチャされたルートを格納するための Azure Database for PostgreSQL を作成します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-101">You decide to create an Azure Database for PostgreSQL to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="7d1a9-102">取得されたデータ ボリュームの履歴から、サーバー ストレージの要件を 20 GB に設定する必要があることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="7d1a9-103">この処理要件をサポートするには、仮想コアを 1 個備えた Compute Gen 5 のサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="7d1a9-104">また、データのバックアップ用に 15 日間の保有期間が必要であることもわかっています。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-104">You also know that you require a retention period of 15 days for data backups.</span></span>

> [!TIP]
> <span data-ttu-id="7d1a9-105">Microsoft Learn の演習はすべて無料ですが、自分で始めるときは、Azure サブスクリプションが必要になります。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-105">All of the exercises you do in Microsoft Learn are free, but once you start exploring on your own, you will need an Azure subscription.</span></span> <span data-ttu-id="7d1a9-106">サブスクリプションをお持ちでない場合、[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。これは数分で作成できます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-106">If you don't have one yet, take a couple of minutes and create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="7d1a9-107">早速始めましょう。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-107">Let's begin.</span></span>

<span data-ttu-id="7d1a9-108">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-108">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

<span data-ttu-id="7d1a9-109">Azure Cloud Shell セッションを開始する必要があることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-109">Recall that you'll need to start an Azure Cloud Shell session.</span></span> <span data-ttu-id="7d1a9-110">画面の上部にある Cloud Shell アイコンを選択してセッションを開始します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-110">Select the Cloud Shell icon at the top of the screen to start the session.</span></span>

![Cloud Shell ボタン](../media-draft/cloud-shell-button.png)

<span data-ttu-id="7d1a9-112">Cloud Shell で使用するストレージ アカウントがまだない場合は、初めてアクセスするときに作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-112">If you don't already have a storage account to use with Cloud Shell, you'll need to create one with first access.</span></span> <span data-ttu-id="7d1a9-113">ポータル インターフェイスの手順に従って、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-113">The portal interface will step you through the process of creating a storage account.</span></span>

<span data-ttu-id="7d1a9-114">このラボでは、コマンド ライン環境として `bash` を使用します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-114">This lab uses `bash` as the command-line environment.</span></span>

1. <span data-ttu-id="7d1a9-115">サーバーの作成に使用するサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-115">Select the subscription you'll use to create the server.</span></span>

    <span data-ttu-id="7d1a9-116">複数のサブスクリプションがある場合は、次のコマンドを使用して適切なサブスクリプションをアクティブにします。0 はお使いのサブスクリプション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-116">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    <span data-ttu-id="7d1a9-117">`az account list --output table` コマンドを使用して、すべてのサブスクリプションを一覧表示できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-117">Recall, you can list all your subscriptions using the `az account list --output table` command.</span></span> <span data-ttu-id="7d1a9-118">使用するサブスクリプション ID をこの一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-118">Pick the subscription identifier from this list that you'd like to use.</span></span>

1. <span data-ttu-id="7d1a9-119">前のユニットでまだ行っていない場合は、リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-119">If you haven't already done so in a previous unit, create a resource group.</span></span> <span data-ttu-id="7d1a9-120">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-120">You'll run the following command.</span></span>

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > <span data-ttu-id="7d1a9-121">`az account list-locations` コマンドを使用して、すべての場所の一覧を取得できます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-121">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="7d1a9-122">値 `displayName` または `name` を選択し、`<location>` パラメーターに使用します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-122">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

1. <span data-ttu-id="7d1a9-123">これで、`az postgres server create` コマンドを実行する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-123">You're now ready to run the `az postgres server create` command.</span></span>

    <span data-ttu-id="7d1a9-124">ストレージ サイズ 20 GB、1 仮想コアの Compute Gen 5 サポート、データ バックアップの保有期間 15 日に、サーバーを設定することに留意してください。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-124">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

    <span data-ttu-id="7d1a9-125">指定するパラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-125">There are several parameters that you'll specify:</span></span>

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    <span data-ttu-id="7d1a9-126">下のソリューションを見なくてもコマンドを記述してパラメーターを完了できるかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-126">See if you can write the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="7d1a9-127">`<>` の値は実際の値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-127">Replace the values in `<>` with your own values.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d1a9-128">`az account list-locations` コマンドを使用して、すべての場所の一覧を取得できます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-128">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="7d1a9-129">値 `displayName` または `name` を選択し、`<location>` パラメーターに使用します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-129">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

<span data-ttu-id="7d1a9-130">実行時にはシステムによる情報の処理にしばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-130">You'll see the system take a few moments to process the information when executed.</span></span> <span data-ttu-id="7d1a9-131">サーバーが作成されると、サーバーを記述する JavaScript Object Notation (JSON) 文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-131">A Java Script Object Notation (JSON) string that describes the server is returned if the server was created.</span></span> <span data-ttu-id="7d1a9-132">サーバーが作成されない場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-132">An error message is displayed if the server isn't created.</span></span> <span data-ttu-id="7d1a9-133">このエラー情報を使用し、コマンド パラメーターを確認して修正します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-133">You'll use this error information to review and fix your command parameters.</span></span>

<span data-ttu-id="7d1a9-134">Azure CLI を使用して PostgreSQL サーバーを正常に作成しました。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-134">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="7d1a9-135">次のユニットでは、サーバーのセキュリティ設定を構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7d1a9-135">In the next unit, you'll see how to configure your server's security settings.</span></span>
