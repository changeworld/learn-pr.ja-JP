仮想マシンが作成されたら、他のコマンドを使用してその仮想マシンに関する情報を取得できます。

`vm list` から始めましょう。

```azurecli
az vm list
```

このコマンドは、このサブスクリプションで定義されている_すべて_の仮想マシンを返します。 出力は、`--resource-group` パラメーターを使用して特定のリソース グループにフィルター処理することができます。 

## <a name="output-types"></a>出力の種類
これまでに行ったすべてのコマンドに対する既定の応答の種類は、JSON でした。 これはスクリプトには優れていますが、ほとんどの人が読みにくいと感じます。 `--output` フラグを使用して、任意の応答の出力スタイルを変更することができます。 たとえば、異なる出力スタイルを確認するため、Azure Cloud Shell で次のコマンドを試してください。

```azurecli
az vm list --output table
```

`table` と共に、`json` (既定)、`jsonc` (色付けされた JSON)、または `tsv` (テーブル区切り値) を指定できます。 違いを確認するため、上記のコマンドを使用していくつかのバリエーションを試してください。

## <a name="getting-the-ip-address"></a>IP アドレスを取得する

もう 1 つの便利なコマンドが `vm list-ip-addresses` です。これは、VM のパブリック IP アドレスとプライベート IP アドレスを一覧表示します。 IP アドレスは、変更になった場合、または作成時にキャプチャしなかった場合、いつでも取得することができます。

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

次が返されます。

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!NOTE]
> `--output` フラグの略式の構文として `-o` を使用していることに注目してください。 Azure CLI コマンドのほとんどのパラメーターは、単一のダッシュと文字に短縮できます。 たとえば、`--name` は `-n`に、`--resource-group` は `-g` に短縮できます。 これは入力するには便利ですが、スクリプトではわかりやすくするために完全なオプション名を使用することをお勧めします。 各コマンドの詳細については、ドキュメントで確認してください。

## <a name="getting-vm-details"></a>VM の詳細を取得する

`vm show` コマンドを使用して、名前または ID で特定の仮想マシンに関するより詳細な情報を取得することができます。

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM
```

これにより、VM に関するあらゆる情報 (接続されている記憶域デバイス、ネットワーク インターフェイス、および VM が接続されているリソースのすべてのオブジェクト ID など) を含むかなり大きな JSON ブロックが返されます。 テーブル形式に変更することはできますが、それにより興味深いデータのほとんどが省略されます。 代わりに、[JMESPath](http://jmespath.org/) と呼ばれる JSON 用の組み込みクエリ言語を使用することができます。

## <a name="adding-filters-to-queries-with-jmespath"></a>JMESPath を使用してクエリにフィルターを追加する

JMESPath は、JSON オブジェクトを中心に構築された業界標準クエリ言語です。 最もシンプルなクエリは、JSON オブジェクト内のキーを選択する_識別子_を指定することです。

たとえば、次のようなオブジェクトについて考えてみましょう。

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

クエリ `people`を使用して、`people` 配列の値の配列を返すことができます。 people の _1 人_だけが必要な場合は、インデクサーを使用できます。 たとえば、`people[1]` と指定すると次のオブジェクトが返されます。

```json
{
    "name": "Barney",
    "age": 25
}
```

絞り込むための修飾子を追加することもでき、条件に基づいてオブジェクトのサブセットが返されます。 たとえば、修飾子 `people[?age > '25']` を指定すると次のオブジェクトが返されます。

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

最後に、結果を簡潔にするために select: `people[?age > '25'].[name]` を追加します。これで、名前だけが返されます。

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

JMESQuery には、興味深いクエリ機能が他にもいくつかあります。 時間があるときに、[オンライン チュートリアル](http://jmespath.org/tutorial.html)を確認してください。これは [JMESPath.org](http://jmespath.org/) サイトから入手できます。

## <a name="filtering-our-azure-cli-queries"></a>Azure CLI クエリをフィルター処理する

JMES クエリの基礎知識があれば、クエリによって返されるデータに `vm show` コマンドのようなフィルターを追加できます。 たとえば、管理者のユーザー名を取得できます。

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "osProfile.adminUsername"
```

VM に割り当てられているサイズを取得できます。

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query hardwareProfile.vmSize
```

または、次のクエリを使用して、ネットワーク インターフェイスのすべての ID を取得できます。

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

このクエリ手法は、任意の Azure CLI コマンドで機能し、コマンドライン上のデータの特定の部分を取得するために使用できます。 これはスクリプトにも役立ちます。たとえば、Azure アカウントから値を取得し、それを環境内またはスクリプトの変数に格納することができます。 このクエリ手法を使用する場合に、追加すると便利なフラグが `--output tsv` パラメーター (`-o tsv` に短縮可能) です。 これを追加すると、実際のデータ値とタブ区切りのみを含むタブ区切り値で結果が返されます。

たとえば、

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

次のテキストが返されます。`/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`
