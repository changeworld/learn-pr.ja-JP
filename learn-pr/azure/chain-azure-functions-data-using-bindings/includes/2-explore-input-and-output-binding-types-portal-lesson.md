<span data-ttu-id="8f43d-101">多くのソフトウェア ソリューションにおいて、データへのアクセスとデータの処理は重要なタスクです。</span><span class="sxs-lookup"><span data-stu-id="8f43d-101">Accessing and processing data are key tasks in many software solutions.</span></span> <span data-ttu-id="8f43d-102">次のシナリオを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="8f43d-102">Consider some of these scenarios:</span></span>

* <span data-ttu-id="8f43d-103">受信データを Blob Storage から Azure Cosmos DB に移動する方法を実装するよう依頼された。</span><span class="sxs-lookup"><span data-stu-id="8f43d-103">You've been asked to implement a way to move incoming data from Blob storage to Azure Cosmos DB.</span></span>
* <span data-ttu-id="8f43d-104">社内の別のコンポーネントで処理するために、受信メッセージをキューに送信したい。</span><span class="sxs-lookup"><span data-stu-id="8f43d-104">You want to post incoming messages to a queue for processing by another component in your enterprise.</span></span>
* <span data-ttu-id="8f43d-105">サービスでキューからゲーマー スコアを取得し、オンライン スコアボードを更新する必要がある。</span><span class="sxs-lookup"><span data-stu-id="8f43d-105">Your service needs to grab gamer scores from a queue and update an online scoreboard.</span></span>

<span data-ttu-id="8f43d-106">これらの例はすべて、データの移動に関するものです。</span><span class="sxs-lookup"><span data-stu-id="8f43d-106">All of these examples are about moving data.</span></span> <span data-ttu-id="8f43d-107">データ ソースと送信先はシナリオによって異なりますが、パターンは似ています。</span><span class="sxs-lookup"><span data-stu-id="8f43d-107">The data source and destinations differ from scenario to scenario, but the pattern is similar.</span></span> <span data-ttu-id="8f43d-108">データ ソースに接続し、データを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="8f43d-108">You connect to a data source, and you read and write data.</span></span> <span data-ttu-id="8f43d-109">Azure Functions では、バインディングを使用してデータやサービスと統合できるようにします。</span><span class="sxs-lookup"><span data-stu-id="8f43d-109">Azure Functions helps you integrate with data and services by using bindings.</span></span> 

## <a name="what-is-a-binding"></a><span data-ttu-id="8f43d-110">バインディングとは</span><span class="sxs-lookup"><span data-stu-id="8f43d-110">What is a binding?</span></span>

<span data-ttu-id="8f43d-111">Azure Functions では、バインディングによって、コード内からデータに接続する宣言型の方法が提供されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-111">In Azure Functions, bindings provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="8f43d-112">バインディングにより、関数内で一貫した方法でデータ ストリームと簡単に統合できるようになります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-112">They make it easier to integrate with data streams consistently in a function.</span></span> <span data-ttu-id="8f43d-113">さまざまなデータ要素へのアクセスを提供する複数のバインディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-113">You can have multiple bindings providing access to different data elements.</span></span> <span data-ttu-id="8f43d-114">これが強力なのは、特定の接続ロジック (データベース接続や Web API インターフェイスなど) をコーディングしなくても、データソースに接続できるためです。</span><span class="sxs-lookup"><span data-stu-id="8f43d-114">This is powerful because you can connect to your data sources without having to code specific connection logic (like database connections or web API interfaces).</span></span>

### <a name="types-of-bindings"></a><span data-ttu-id="8f43d-115">バインディングの種類</span><span class="sxs-lookup"><span data-stu-id="8f43d-115">Types of bindings</span></span>

<span data-ttu-id="8f43d-116">関数で使用できるバインディングには、次の 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-116">There are two kinds of bindings you can use with functions:</span></span>

1. <span data-ttu-id="8f43d-117">**入力バインディング**: 入力バインディングとは、データ **ソース**への接続です。</span><span class="sxs-lookup"><span data-stu-id="8f43d-117">**Input binding** An input binding is a connection to a data **source**.</span></span> <span data-ttu-id="8f43d-118">関数は、これらの入力からデータを読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-118">Our function can read data from these inputs.</span></span>

1. <span data-ttu-id="8f43d-119">**出力バインディング**: 出力バインディングとは、データの**送信先**への接続です。</span><span class="sxs-lookup"><span data-stu-id="8f43d-119">**Output binding** An output binding is a connection to a data **destination**.</span></span> <span data-ttu-id="8f43d-120">関数は、これらの送信先にデータを書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-120">Our function can write data to these destinations.</span></span>

<span data-ttu-id="8f43d-121">トリガーもあります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-121">There are also triggers.</span></span> <span data-ttu-id="8f43d-122">トリガーは、関数を実行させる特殊な種類の入力バインディングです。</span><span class="sxs-lookup"><span data-stu-id="8f43d-122">Triggers are special types of input bindings that cause a function to execute.</span></span> <span data-ttu-id="8f43d-123">たとえば、Azure Event Grid の通知をトリガーとして構成できます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-123">For example, an Azure Event Grid notification can be configured as a trigger.</span></span> <span data-ttu-id="8f43d-124">イベントが発生すると、関数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-124">When an event occurs, the function will run.</span></span>

### <a name="types-of-supported-bindings"></a><span data-ttu-id="8f43d-125">サポートされるバインディングの種類</span><span class="sxs-lookup"><span data-stu-id="8f43d-125">Types of supported bindings</span></span>

<span data-ttu-id="8f43d-126">バインディングの "*種類*" では、データの読み取り先または送信先を定義します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-126">The *type* of binding defines where we are reading or sending data.</span></span> <span data-ttu-id="8f43d-127">Web 要求に応答するためのバインディングと、各種 Azure サービスやサード パーティのサービスと直接やり取りするためのさまざまなバインディングがあります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-127">There is a binding to respond to web requests and a large selection of bindings to interact directly with various Azure services as well as third-party services.</span></span>

<span data-ttu-id="8f43d-128">バインディングの種類は、入力、出力、またはその両方として使用できます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-128">A binding type can be used as an input, an output or both.</span></span> <span data-ttu-id="8f43d-129">たとえば、関数は Azure Blob Storage 出力バインディングに書き込むことができますが、Blob Storage の更新によって別の関数をトリガーすることができます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-129">For example, a function can write to Azure Blob Storage output binding, but a blob storage update could trigger another function.</span></span>

<span data-ttu-id="8f43d-130">一般的なバインドの種類を次に示します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-130">Some common binding types are listed below:</span></span>
- <span data-ttu-id="8f43d-131">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8f43d-131">Blob Storage</span></span>
- <span data-ttu-id="8f43d-132">Azure Service Bus キュー</span><span class="sxs-lookup"><span data-stu-id="8f43d-132">Azure Service Bus Queues</span></span>
- <span data-ttu-id="8f43d-133">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8f43d-133">Azure Cosmos DB</span></span>
- <span data-ttu-id="8f43d-134">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="8f43d-134">Azure Event Hubs</span></span>
- <span data-ttu-id="8f43d-135">外部ファイル</span><span class="sxs-lookup"><span data-stu-id="8f43d-135">External Files</span></span>
- <span data-ttu-id="8f43d-136">外部テーブル</span><span class="sxs-lookup"><span data-stu-id="8f43d-136">External Tables</span></span>
- <span data-ttu-id="8f43d-137">HTTP エンドポイント</span><span class="sxs-lookup"><span data-stu-id="8f43d-137">HTTP endpoints</span></span>

<span data-ttu-id="8f43d-138">これらの種類はほんの一部にすぎません。</span><span class="sxs-lookup"><span data-stu-id="8f43d-138">These types are just a sample.</span></span> <span data-ttu-id="8f43d-139">ほかにも多くの種類があります。また、関数には、さらに多くのバインディングを追加する拡張モデルもあります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-139">There are more, plus functions have an extensibility model to add more bindings.</span></span>

### <a name="binding-properties"></a><span data-ttu-id="8f43d-140">バインディングのプロパティ</span><span class="sxs-lookup"><span data-stu-id="8f43d-140">Binding properties</span></span>

<span data-ttu-id="8f43d-141">どのバインディングでも 3 つのプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="8f43d-141">Three properties are required in all bindings.</span></span> <span data-ttu-id="8f43d-142">使用しているバインディングとストレージの種類に基づいて、追加のプロパティを提供することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-142">You may have to supply additional properties based on the type of binding and storage you are using.</span></span>

1. <span data-ttu-id="8f43d-143">**名前**: データにアクセスするために使用する関数パラメーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-143">**Name** Defines the function parameter through which you access the data.</span></span> <span data-ttu-id="8f43d-144">たとえば、キュー入力バインディングでは、これはキュー メッセージの内容を受け取る関数パラメーターの名前になります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-144">For example, in a queue input binding, this is the name of the function parameter that receives the queue message content.</span></span> 

1. <span data-ttu-id="8f43d-145">**種類**: バインディングの種類 (対話するデータまたはサービスの種類) を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-145">**Type** Identifies the type of binding, i.e., the type of data or service we want to interact with.</span></span>

1. <span data-ttu-id="8f43d-146">**方向**: データが流れる方向 (入力バインディングか、出力バインディングか) を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-146">**Direction** Indicates the direction data is flowing, i.e., is it an input or output binding?</span></span>

<span data-ttu-id="8f43d-147">さらに、ほとんどのバインドの種類では、4 つ目のプロパティも必要です。</span><span class="sxs-lookup"><span data-stu-id="8f43d-147">Additionally, most binding types also need a fourth property:</span></span> 

4. <span data-ttu-id="8f43d-148">**接続**: 接続文字列が含まれたアプリ設定キーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-148">**Connection** Provides the name of an app setting key that contains the connection string.</span></span> <span data-ttu-id="8f43d-149">バインディングでは、アプリ設定に保存された接続文字列を使用して、関数コードの外部にシークレットを保持します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-149">Bindings use connection strings stored in app settings to keep secrets out of the function code.</span></span> <span data-ttu-id="8f43d-150">これにより、コードをより柔軟に構成できるようになり、コードの安全性も高まります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-150">This makes your code more configurable and secure.</span></span>

## <a name="create-a-binding"></a><span data-ttu-id="8f43d-151">バインディングを作成する</span><span class="sxs-lookup"><span data-stu-id="8f43d-151">Create a binding</span></span>

<span data-ttu-id="8f43d-152">バインディングは JSON で定義されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-152">Bindings are defined in JSON.</span></span> <span data-ttu-id="8f43d-153">バインディングは関数の構成ファイルで構成されます。このファイルは *function.json* という名前で、関数コードと同じフォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-153">A binding is configured in your function's configuration file, which is named *function.json* and lives in the same folder as your function code.</span></span>

 <span data-ttu-id="8f43d-154">"*入力バインディング*" のサンプルを詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="8f43d-154">Let's examine a sample *input binding*:</span></span>

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

<span data-ttu-id="8f43d-155">このバインディングを作成するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="8f43d-155">To create this binding, we:</span></span>

1. <span data-ttu-id="8f43d-156">*function.json* ファイルにバインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-156">Create a binding in our *function.json* file.</span></span>

1. <span data-ttu-id="8f43d-157">`name` 変数の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-157">Provide the value for the `name` variable.</span></span> <span data-ttu-id="8f43d-158">この例では、変数に BLOB データが保持されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-158">In this example, the variable holds the blob data.</span></span>

1. <span data-ttu-id="8f43d-159">ストレージの `type` を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-159">Provide the storage `type`.</span></span> <span data-ttu-id="8f43d-160">前の例では、Blob Storage を使用しています。</span><span class="sxs-lookup"><span data-stu-id="8f43d-160">In the preceding example, we are using Blob storage.</span></span>

1. <span data-ttu-id="8f43d-161">コンテナーとそこに格納される項目名を指定する、`path` を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-161">Provide the `path`, which specifies the container and the item name that goes in it.</span></span> <span data-ttu-id="8f43d-162">BLOB の場合、`path` プロパティは必須です。</span><span class="sxs-lookup"><span data-stu-id="8f43d-162">The `path` property is required for blobs.</span></span>

1. <span data-ttu-id="8f43d-163">アプリケーションの設定ファイルで定義されている `connection` 文字列設定名を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-163">Provide the `connection` string setting name defined in the application's settings file.</span></span> <span data-ttu-id="8f43d-164">これは、ストレージ アカウントに接続する際の接続文字列を見つけるためのキーとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-164">It's used as a key to find the connection string to connect to your storage account.</span></span>

1. <span data-ttu-id="8f43d-165">`direction` を `in` として定義します。</span><span class="sxs-lookup"><span data-stu-id="8f43d-165">Define the `direction` as `in`.</span></span> <span data-ttu-id="8f43d-166">これにより、BLOB からデータを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="8f43d-166">It reads data from the blob.</span></span>

<span data-ttu-id="8f43d-167">バインディングは、関数でデータに接続するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8f43d-167">Bindings are used to connect to data in your function.</span></span> <span data-ttu-id="8f43d-168">この例では、入力バインディングを使用して、関数によってサムネイルとして処理されるユーザー画像に接続しました。</span><span class="sxs-lookup"><span data-stu-id="8f43d-168">In this example, we used an input binding to connect user images to be processed by our function as thumbnails.</span></span>
