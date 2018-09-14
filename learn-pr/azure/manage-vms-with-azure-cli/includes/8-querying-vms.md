<span data-ttu-id="0a877-101">仮想マシンが作成されたら、他のコマンドを使用してその仮想マシンに関する情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-101">Now that a virtual machine has been created, we can get information about it through other commands.</span></span>

<span data-ttu-id="0a877-102">`vm list` から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="0a877-102">Let's start with `vm list`.</span></span>

```azurecli
az vm list
```

<span data-ttu-id="0a877-103">このコマンドは、このサブスクリプションで定義されている_すべて_の仮想マシンを返します。</span><span class="sxs-lookup"><span data-stu-id="0a877-103">This command will return _all_ virtual machines defined in this subscription.</span></span> <span data-ttu-id="0a877-104">出力は、`--resource-group` パラメーターを使用して特定のリソース グループにフィルター処理することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-104">The output can be filtered to a specific resource group through the `--resource-group` parameter.</span></span> 

## <a name="output-types"></a><span data-ttu-id="0a877-105">出力の種類</span><span class="sxs-lookup"><span data-stu-id="0a877-105">Output types</span></span>
<span data-ttu-id="0a877-106">これまでに行ったすべてのコマンドに対する既定の応答の種類は、JSON でした。</span><span class="sxs-lookup"><span data-stu-id="0a877-106">Notice that the default response type for all the commands we've done so far is JSON.</span></span> <span data-ttu-id="0a877-107">これはスクリプトには優れていますが、ほとんどの人が読みにくいと感じます。</span><span class="sxs-lookup"><span data-stu-id="0a877-107">This is great for scripting - but most people find it harder to read.</span></span> <span data-ttu-id="0a877-108">`--output` フラグを使用して、任意の応答の出力スタイルを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-108">You can change the output style for any response through the `--output` flag.</span></span> <span data-ttu-id="0a877-109">たとえば、異なる出力スタイルを確認するため、Azure Cloud Shell で次のコマンドを試してください。</span><span class="sxs-lookup"><span data-stu-id="0a877-109">For example, try the following command in Azure Cloud Shell to see the different output style.</span></span>

```azurecli
az vm list --output table
```

<span data-ttu-id="0a877-110">`table` と共に、`json` (既定)、`jsonc` (色付けされた JSON)、または `tsv` (テーブル区切り値) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-110">Along with `table`, you can specify `json` (the default), `jsonc` (colorized JSON), or `tsv` (Table-Separated Values).</span></span> <span data-ttu-id="0a877-111">違いを確認するため、上記のコマンドを使用していくつかのバリエーションを試してください。</span><span class="sxs-lookup"><span data-stu-id="0a877-111">Try a few variations with the above command to see the difference.</span></span>

## <a name="getting-the-ip-address"></a><span data-ttu-id="0a877-112">IP アドレスを取得する</span><span class="sxs-lookup"><span data-stu-id="0a877-112">Getting the IP address</span></span>

<span data-ttu-id="0a877-113">もう 1 つの便利なコマンドが `vm list-ip-addresses` です。これは、VM のパブリック IP アドレスとプライベート IP アドレスを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="0a877-113">Another useful command is `vm list-ip-addresses`, which will list the public and private IP addresses for a VM.</span></span> <span data-ttu-id="0a877-114">IP アドレスは、変更になった場合、または作成時にキャプチャしなかった場合、いつでも取得することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-114">If they change, or you didn't capture them during creation, you can retrieve them at any time.</span></span>

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

<span data-ttu-id="0a877-115">次が返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-115">This returns:</span></span>

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!TIP]
> <span data-ttu-id="0a877-116">`--output` フラグの略式の構文として `-o` を使用していることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="0a877-116">Notice that we are using a shorthand syntax for the `--output` flag as `-o`.</span></span> <span data-ttu-id="0a877-117">Azure CLI コマンドのほとんどのパラメーターは、単一のダッシュと文字に短縮できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-117">Most parameters to Azure CLI commands can be shortened to a single dash and letter.</span></span> <span data-ttu-id="0a877-118">たとえば、`--name` は `-n`に、`--resource-group` は `-g` に短縮できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-118">For example, `--name` can be shortened to `-n` and `--resource-group` to `-g`.</span></span> <span data-ttu-id="0a877-119">これは入力するには便利ですが、スクリプトではわかりやすくするために完全なオプション名を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0a877-119">This is handy for typing, but we recommend using the full option name in scripts for clarity.</span></span> <span data-ttu-id="0a877-120">各コマンドの詳細については、ドキュメントで確認してください。</span><span class="sxs-lookup"><span data-stu-id="0a877-120">Check the documentation for details on each command.</span></span>

## <a name="getting-vm-details"></a><span data-ttu-id="0a877-121">VM の詳細を取得する</span><span class="sxs-lookup"><span data-stu-id="0a877-121">Getting VM details</span></span>

<span data-ttu-id="0a877-122">`vm show` コマンドを使用して、名前または ID で特定の仮想マシンに関するより詳細な情報を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-122">We can get more detailed information about a specific virtual machine by name or ID using the `vm show` command.</span></span>

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM
```

<span data-ttu-id="0a877-123">これにより、VM に関するあらゆる情報 (接続されている記憶域デバイス、ネットワーク インターフェイス、および VM が接続されているリソースのすべてのオブジェクト ID など) を含むかなり大きな JSON ブロックが返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-123">This will return a fairly large JSON block with all sorts of information about the VM, including attached storage devices, network interfaces, and all of the object IDs for resources that the VM is connected to.</span></span> <span data-ttu-id="0a877-124">テーブル形式に変更することはできますが、それにより興味深いデータのほとんどが省略されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-124">Again, we could change to a table format, but that omits almost all of the interesting data.</span></span> <span data-ttu-id="0a877-125">代わりに、[JMESPath](http://jmespath.org/) と呼ばれる JSON 用の組み込みクエリ言語を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-125">Instead, we can turn to a built-in query language for JSON called [JMESPath](http://jmespath.org/).</span></span>

## <a name="adding-filters-to-queries-with-jmespath"></a><span data-ttu-id="0a877-126">JMESPath を使用してクエリにフィルターを追加する</span><span class="sxs-lookup"><span data-stu-id="0a877-126">Adding filters to queries with JMESPath</span></span>

<span data-ttu-id="0a877-127">JMESPath は、JSON オブジェクトを中心に構築された業界標準クエリ言語です。</span><span class="sxs-lookup"><span data-stu-id="0a877-127">JMESPath is an industry-standard query language built around JSON objects.</span></span> <span data-ttu-id="0a877-128">最もシンプルなクエリは、JSON オブジェクト内のキーを選択する_識別子_を指定することです。</span><span class="sxs-lookup"><span data-stu-id="0a877-128">The simplest query is to specify an _identifier_ that selects a key in the JSON object.</span></span>

<span data-ttu-id="0a877-129">たとえば、次のようなオブジェクトについて考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0a877-129">For example, given the object:</span></span>

```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

<span data-ttu-id="0a877-130">クエリ `people`を使用して、`people` 配列の値の配列を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-130">We can use the query `people` to return the array of values for the `people` array.</span></span> <span data-ttu-id="0a877-131">people の _1 人_だけが必要な場合は、インデクサーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-131">If we just want _one_ of the people, we can use an indexer.</span></span> <span data-ttu-id="0a877-132">たとえば、`people[1]` と指定すると次のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-132">For example, `people[1]` would return:</span></span>

```json
{
    "name": "Barney",
    "age": 25
}
```

<span data-ttu-id="0a877-133">絞り込むための修飾子を追加することもでき、条件に基づいてオブジェクトのサブセットが返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-133">We can also add specific qualifiers that would return a subset of the objects based on some criteria.</span></span> <span data-ttu-id="0a877-134">たとえば、修飾子 `people[?age > '25']` を指定すると次のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-134">For example, adding the qualifier `people[?age > '25']` would return:</span></span>

```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

<span data-ttu-id="0a877-135">最後に、結果を簡潔にするために select: `people[?age > '25'].[name]` を追加します。これで、名前だけが返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-135">Finally, we can constrain the results by adding a select: `people[?age > '25'].[name]` that returns just the names:</span></span>

```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

<span data-ttu-id="0a877-136">JMESQuery には、興味深いクエリ機能が他にもいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="0a877-136">JMESQuery has several other interesting query features.</span></span> <span data-ttu-id="0a877-137">時間があるときに、[オンライン チュートリアル](http://jmespath.org/tutorial.html)を確認してください。これは [JMESPath.org](http://jmespath.org/) サイトから入手できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-137">When you have time, check out the [online tutorial](http://jmespath.org/tutorial.html) available on the [JMESPath.org](http://jmespath.org/) site.</span></span>

## <a name="filtering-our-azure-cli-queries"></a><span data-ttu-id="0a877-138">Azure CLI クエリをフィルター処理する</span><span class="sxs-lookup"><span data-stu-id="0a877-138">Filtering our Azure CLI queries</span></span>

<span data-ttu-id="0a877-139">JMES クエリの基礎知識があれば、クエリによって返されるデータに `vm show` コマンドのようなフィルターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-139">With a basic understanding of JMES queries, we can add filers to the data being returned by queries like the `vm show` command.</span></span> <span data-ttu-id="0a877-140">たとえば、管理者のユーザー名を取得できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-140">For example, we can retrieve the admin user name:</span></span>

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "osProfile.adminUsername"
```

<span data-ttu-id="0a877-141">VM に割り当てられているサイズを取得できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-141">We can get the size assigned to our VM:</span></span>

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query hardwareProfile.vmSize
```

<span data-ttu-id="0a877-142">または、次のクエリを使用して、ネットワーク インターフェイスのすべての ID を取得できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-142">Or to retrieve all the IDs for your network interfaces, you can use the query:</span></span>

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

<span data-ttu-id="0a877-143">このクエリ手法は、任意の Azure CLI コマンドで機能し、コマンドライン上のデータの特定の部分を取得するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="0a877-143">This query technique will work with any Azure CLI command and can be used to pull specific bits of data out on the command line.</span></span> <span data-ttu-id="0a877-144">これはスクリプトにも役立ちます。たとえば、Azure アカウントから値を取得し、それを環境内またはスクリプトの変数に格納することができます。</span><span class="sxs-lookup"><span data-stu-id="0a877-144">It is useful for scripting as well - for example, you can pull a value out of your Azure account and store it in an environment or script variable.</span></span> <span data-ttu-id="0a877-145">このクエリ手法を使用する場合に、追加すると便利なフラグが `--output tsv` パラメーター (`-o tsv` に短縮可能) です。</span><span class="sxs-lookup"><span data-stu-id="0a877-145">If you decide to use it this way, a useful flag to add is the `--output tsv` parameter (which can be shortened to `-o tsv`).</span></span> <span data-ttu-id="0a877-146">これを追加すると、実際のデータ値とタブ区切りのみを含むタブ区切り値で結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="0a877-146">This will return the results in tab-separated values that only include the actual data values with tab separators.</span></span>

<span data-ttu-id="0a877-147">たとえば、</span><span class="sxs-lookup"><span data-stu-id="0a877-147">As an example,</span></span>

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

<span data-ttu-id="0a877-148">次のテキストが返されます。`/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span><span class="sxs-lookup"><span data-stu-id="0a877-148">returns the text: `/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`</span></span>
