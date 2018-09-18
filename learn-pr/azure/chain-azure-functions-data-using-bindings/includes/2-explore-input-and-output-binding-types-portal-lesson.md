<span data-ttu-id="113ba-101">多くのソフトウェア ソリューションにおいて、データへのアクセスとデータの処理は重要なタスクです。</span><span class="sxs-lookup"><span data-stu-id="113ba-101">Accessing and processing data are key tasks in many software solutions.</span></span> <span data-ttu-id="113ba-102">次のシナリオを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="113ba-102">Consider some of these scenarios:</span></span>

* <span data-ttu-id="113ba-103">受信データを Blob Storage から Azure Cosmos DB に移動する方法を実装するよう依頼された。</span><span class="sxs-lookup"><span data-stu-id="113ba-103">You've been asked to implement a way to move incoming data from blob storage to Azure Cosmos DB.</span></span>
* <span data-ttu-id="113ba-104">社内の別のコンポーネントで処理するために、受信メッセージをキューに送信したい。</span><span class="sxs-lookup"><span data-stu-id="113ba-104">You want to post incoming messages to a queue for processing by another component in your enterprise.</span></span>
* <span data-ttu-id="113ba-105">サービスでキューからゲーマー スコアを取得し、オンライン スコアボードを更新する必要がある。</span><span class="sxs-lookup"><span data-stu-id="113ba-105">Your service needs to grab gamer scores from a queue and update an online scoreboard.</span></span>

<span data-ttu-id="113ba-106">これらの例はすべて、データの移動に関するものです。</span><span class="sxs-lookup"><span data-stu-id="113ba-106">All of these examples are about moving data.</span></span> <span data-ttu-id="113ba-107">データ ソースと送信先はシナリオによって異なりますが、パターンは似ています。</span><span class="sxs-lookup"><span data-stu-id="113ba-107">The data source and destinations differ from scenario to scenario, but the pattern is similar.</span></span> <span data-ttu-id="113ba-108">データ ソースに接続し、データを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="113ba-108">You connect to a data source, you read and write data.</span></span> <span data-ttu-id="113ba-109">Azure Functions では、バインディングを使用してデータやサービスとの統合できるようにします。</span><span class="sxs-lookup"><span data-stu-id="113ba-109">Azure Functions helps you integrate with data and services using bindings.</span></span> 

## <a name="what-is-a-binding"></a><span data-ttu-id="113ba-110">バインディングとは</span><span class="sxs-lookup"><span data-stu-id="113ba-110">What is a binding?</span></span>

<span data-ttu-id="113ba-111">関数では、バインディングによって、コード内からデータに接続する宣言型の方法が提供されます。</span><span class="sxs-lookup"><span data-stu-id="113ba-111">In functions, bindings provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="113ba-112">バインディングにより、関数内で一貫した方法でデータ ストリームと簡単に統合できるようになります。</span><span class="sxs-lookup"><span data-stu-id="113ba-112">They make it easier to integrate with data streams consistently in a function.</span></span> <span data-ttu-id="113ba-113">トリガーでは関数を呼び出す方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="113ba-113">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="113ba-114">使用できるトリガーは 1 つだけですが、1 つの関数で複数のバインディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="113ba-114">You can only have one trigger, but you can have multiple bindings in a function.</span></span> <span data-ttu-id="113ba-115">これが強力なのは、"*接続文字列*" などの値をハード コードしなくても、データ ソースに接続できるためです。</span><span class="sxs-lookup"><span data-stu-id="113ba-115">This is powerful because you can connect to your data sources without having to hard code the values, like for instance, the *connection string*.</span></span>

### <a name="two-kinds-of-bindings"></a><span data-ttu-id="113ba-116">2 種類のバインディング</span><span class="sxs-lookup"><span data-stu-id="113ba-116">Two kinds of bindings</span></span>

<span data-ttu-id="113ba-117">関数に使用できるバインディングには、次の 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="113ba-117">There are two kinds of bindings you can use for your functions:</span></span>

1. <span data-ttu-id="113ba-118">**入力バインディング**: 入力バインディングとは、データ **ソース**への接続です。</span><span class="sxs-lookup"><span data-stu-id="113ba-118">**Input binding** An input binding is a connection to a data **source**.</span></span> <span data-ttu-id="113ba-119">関数はこのソースからデータを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="113ba-119">Our function reads data from this source.</span></span>

1. <span data-ttu-id="113ba-120">**出力バインディング**: 出力バインディングとは、データの**送信先**への接続です。</span><span class="sxs-lookup"><span data-stu-id="113ba-120">**Output binding** An output binding is a connection to a data **destination**.</span></span> <span data-ttu-id="113ba-121">関数はこの送信先にデータを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="113ba-121">Our function writes data to this destination.</span></span>

### <a name="types-of-supported-bindings"></a><span data-ttu-id="113ba-122">サポートされるバインディングの種類</span><span class="sxs-lookup"><span data-stu-id="113ba-122">Types of supported bindings</span></span>

<span data-ttu-id="113ba-123">バインディングの "*種類*" では、データの読み取り先または送信先を定義します。</span><span class="sxs-lookup"><span data-stu-id="113ba-123">The *type* of binding defines where we are reading or sending data.</span></span> <span data-ttu-id="113ba-124">Web 要求に応答するためのバインディングと、ストレージと対話するためのさまざまなバインディングがあります。</span><span class="sxs-lookup"><span data-stu-id="113ba-124">There is a binding to respond to web requests and a large selection of bindings to interact with storage.</span></span>

<span data-ttu-id="113ba-125">よく使用されるバインディングを次に示します。</span><span class="sxs-lookup"><span data-stu-id="113ba-125">Here's a list of commonly used bindings:</span></span>
- <span data-ttu-id="113ba-126">BLOB</span><span class="sxs-lookup"><span data-stu-id="113ba-126">Blob</span></span>
- <span data-ttu-id="113ba-127">キュー</span><span class="sxs-lookup"><span data-stu-id="113ba-127">Queue</span></span>
- <span data-ttu-id="113ba-128">CosmosDB</span><span class="sxs-lookup"><span data-stu-id="113ba-128">CosmosDB</span></span>
- <span data-ttu-id="113ba-129">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="113ba-129">Event Hubs</span></span>
- <span data-ttu-id="113ba-130">外部ファイル</span><span class="sxs-lookup"><span data-stu-id="113ba-130">External Files</span></span>
- <span data-ttu-id="113ba-131">外部テーブル</span><span class="sxs-lookup"><span data-stu-id="113ba-131">External Tables</span></span>
- <span data-ttu-id="113ba-132">HTTP</span><span class="sxs-lookup"><span data-stu-id="113ba-132">HTTP</span></span>

### <a name="binding-properties"></a><span data-ttu-id="113ba-133">バインディングのプロパティ</span><span class="sxs-lookup"><span data-stu-id="113ba-133">Binding properties</span></span>

<span data-ttu-id="113ba-134">すべてのバインディングで必要な 4 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="113ba-134">There are four properties that are required in all bindings.</span></span> <span data-ttu-id="113ba-135">使用しているバインディングやストレージの種類に基づいて、追加のプロパティを提供することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="113ba-135">You may have to supply additional properties based on the type of binding and/or storage you are using.</span></span>

- <span data-ttu-id="113ba-136">**名前**: `name` プロパティでは、データにアクセスするために使用する関数パラメーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="113ba-136">**Name** The `name` property defines the function parameter through which you access the data.</span></span> <span data-ttu-id="113ba-137">たとえば、キュー入力バインディングでは、これはキュー メッセージの内容を受け取る関数パラメーターの名前になります。</span><span class="sxs-lookup"><span data-stu-id="113ba-137">For example, in a queue input binding, this is the name of the function parameter that receives the queue message content.</span></span> 

- <span data-ttu-id="113ba-138">**種類**: `type` プロパティでは、バインディングの種類 (対話するデータまたはサービスの種類) を指定します。</span><span class="sxs-lookup"><span data-stu-id="113ba-138">**Type** The `type` property identifies the type of binding, i.e., the type of data or service we want to interact with.</span></span>

- <span data-ttu-id="113ba-139">**方向**: `direction` プロパティでは、データが流れる方向 (入力バインディングか、出力バインディングか) を指定します。</span><span class="sxs-lookup"><span data-stu-id="113ba-139">**Direction** The `direction` property identifies the direction data is flowing ie. is it an input or output binding?</span></span>

- <span data-ttu-id="113ba-140">**接続**: `connection` プロパティは、接続文字列自体ではなく、接続文字列を含むアプリ設定の名前です。</span><span class="sxs-lookup"><span data-stu-id="113ba-140">**Connection** The `connection` property is the name of an app setting that contains the connection string, not the connection string itself.</span></span> <span data-ttu-id="113ba-141">バインディングでは、"function.json にはサービス シークレットを含めない" というベスト プラクティスを適用するために、アプリ設定に保存された接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="113ba-141">Bindings use connection strings stored in app settings to enforce the best practice that function.json does not contain service secrets.</span></span>

## <a name="create-a-binding"></a><span data-ttu-id="113ba-142">バインディングを作成する</span><span class="sxs-lookup"><span data-stu-id="113ba-142">Create a binding</span></span>

<span data-ttu-id="113ba-143">バインディングは JSON で定義されます。</span><span class="sxs-lookup"><span data-stu-id="113ba-143">Bindings are defined in JSON.</span></span> <span data-ttu-id="113ba-144">バインディングは関数の構成ファイルで構成されます。このファイルは `function.json` という名前で、関数コードと同じフォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="113ba-144">A binding is configured in your function's configuration file, which is named `function.json` and lives in the same folder as your function code.</span></span>

 <span data-ttu-id="113ba-145">"*入力バインディング*" のサンプルを詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="113ba-145">Let's examine a sample of an *input binding*:</span></span>

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

<span data-ttu-id="113ba-146">このバインディングを作成するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="113ba-146">To create this binding we:</span></span>

1. <span data-ttu-id="113ba-147">`function.json` ファイルにバインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="113ba-147">Create a binding in our `function.json` file.</span></span>

1. <span data-ttu-id="113ba-148">`name` 変数の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="113ba-148">Provide the value for the `name` variable.</span></span> <span data-ttu-id="113ba-149">この例では、変数に BLOB データが保持されます。</span><span class="sxs-lookup"><span data-stu-id="113ba-149">In this example, the variable will hold the blob data.</span></span>

1. <span data-ttu-id="113ba-150">ストレージの `type` を指定します。前の例では、Blob Storage を使用しています。</span><span class="sxs-lookup"><span data-stu-id="113ba-150">Provide the storage `type`, in the preceding example, we are using Blob storage.</span></span>

1. <span data-ttu-id="113ba-151">コンテナーとそこに格納される項目名を指定する、`path` を指定します。</span><span class="sxs-lookup"><span data-stu-id="113ba-151">Provide the `path`, which specifies the container and the item name which goes in it.</span></span> <span data-ttu-id="113ba-152">BLOB の場合、`path` プロパティは必須です。</span><span class="sxs-lookup"><span data-stu-id="113ba-152">The `path` property is required for Blobs.</span></span>

1. <span data-ttu-id="113ba-153">アプリケーションの設定ファイルで定義されている `connection` 文字列設定名を指定します。</span><span class="sxs-lookup"><span data-stu-id="113ba-153">Provide the `connection` string setting name defined in the application's settings file.</span></span> <span data-ttu-id="113ba-154">これは、ストレージに接続する際の接続文字列を見つけるための参照として使用されます。</span><span class="sxs-lookup"><span data-stu-id="113ba-154">It's used as a lookup to find the connection string to connect to your storage.</span></span>

1. <span data-ttu-id="113ba-155">BLOB からデータを読み取るため、`direction` を `in` として定義します。</span><span class="sxs-lookup"><span data-stu-id="113ba-155">Define the `direction` as `in`, it will read data from the Blob.</span></span>

<span data-ttu-id="113ba-156">バインディングは、関数へのデータに接続するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="113ba-156">Bindings are used to connect to data into your function.</span></span> <span data-ttu-id="113ba-157">この例では、入力バインディングを使用して、関数によってサムネイルとして処理されるユーザー画像に接続しました。</span><span class="sxs-lookup"><span data-stu-id="113ba-157">In the example we looked at, we used an input binding to connect user images to be processed by our function as thumbnails.</span></span>