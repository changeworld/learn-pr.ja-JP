プロフェッショナル、テクノロジとしては特定の領域に専門知識をある可能性があります。 おそらく、記憶域の管理者または仮想化エキスパート、またはおそらくに集中すること、最新のセキュリティ プラクティスをできました。 学生なら、する可能性がありますも利用する最も必要なもの。

::: zone pivot="windows-cloud"

自分の役割に関係なくほとんどの人を使ってみるクラウド仮想マシンを作成します。 ここで Windows Server 2016 を実行する仮想マシンを取り込みます。

::: zone-end

::: zone pivot="linux-cloud"

自分の役割に関係なくほとんどの人を使ってみるクラウド仮想マシンを作成します。 ここで Ubuntu 16.04 を実行する仮想マシンを取り込みます。

::: zone-end

Azure で仮想マシンを作成する多くの方法はあります。 ここでは、Cloud Shell と呼ばれる対話型ターミナルを使用して、Windows または Linux 仮想マシンを取り込みます。 日常的に、ターミナルから作業している場合、ジョブの実行を取得する最も簡単な方法は、多くの場合、これがわかります。

::: zone pivot="windows-cloud"

> [!TIP]
> Linux を好むか、何か新しいですか。 選択**Linux** Linux 仮想マシンを実行するには、このページの上部から。

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Windows またはを新しいものをご試用いただけますか。 選択**Windows** Windows Server 仮想マシンを実行するには、このページの上部から。

::: zone-end

いくつかの基本的な用語を確認し、稼働している最初の仮想マシンを取得してみましょう。

## <a name="what-is-a-virtual-machine"></a>仮想マシンとは何ですか。

仮想マシン、または VM、物理コンピューターのソフトウェア エミュレーションにです。 Vm は、ソフトウェア、数十、数百台、存在するか、千もの Azure Vm は分単位で生成できる、不要になったときに削除されます。 低コストで分単位の課金でそれらを使用しているだけの期間を使用するコンピューティング リソースに対してのみ支払います。 さらに、ニーズに合わせて Vm を構成する方法はたくさんあります。

::: zone pivot="windows-cloud"

実行中の VM のスナップショットと呼ばれる、_イメージ_します。 Azure では、Windows と Linux の一部のエディションのイメージを提供します。 展開の速度を上げるようにする、独自の事前構成済みイメージを作成することもできます。 ここで Microsoft によって提供される、Windows Server 2016 VM を取り込みます。

::: zone-end

::: zone pivot="linux-cloud"

実行中の VM のスナップショットと呼ばれる、_イメージ_します。 Azure では、Windows と Linux の一部のエディションのイメージを提供します。 展開の速度を上げるようにする、独自の事前構成済みイメージを作成することもできます。 ここで Canonical によって提供される、Ubuntu 16.04 VM を取り込みます。

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Azure で仮想マシンを定義しますか。

仮想マシンは、そのサイズや場所など、さまざまな要因の数によって定義されます。 VM を起動する前に簡単に紹介する手順します。

:::row:::
    :::column:::
        **サイズ**
    :::column-end:::
    列のスパン =「3」: VM の_サイズ_プロセッサ速度、メモリの量、ストレージ、および予期されるネットワーク帯域幅の初期量を定義します。 いくつかのサイズには、大量のグラフィックスのレンダリングやビデオ編集の Gpu などの特殊なハードウェアも含まれます。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **[リージョン]**
    :::column-end:::
    列のスパン =「3」: Azure は、世界中の分散データ センターの構成されます。 A_リージョン_は名前付きの地理的な場所での Azure データ センターのセットです。 仮想マシンを含む、すべての Azure リソースには、リージョンが割り当てられます。 米国東部と北ヨーロッパ リージョンの例に示します。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **ネットワーク**
    :::column-end:::
    列のスパン =「3」: A_仮想ネットワーク_は Azure で論理的に分離されたネットワークです。 Azure では、各仮想マシンは、仮想ネットワークに関連付けられます。 Azure と呼ばれる仮想ネットワークのクラウド レベルのファイアウォールを提供する_ネットワーク セキュリティ グループ_します。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **リソース グループ**
    :::column-end:::
    列のスパン =「3」: 仮想マシンとその他のクラウド リソースと呼ばれる論理コンテナーにグループ化されます_リソース グループ_します。 グループは通常、アプリケーションまたはサービスの一部として一緒にデプロイされるリソースの整理に使用されます。 リソース グループを参照するには、名前でします。
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Azure Cloud Shell とは

Azure Cloud Shell は、管理および Azure リソースを開発するためのブラウザー ベースのコマンド ライン エクスペリエンスです。 Cloud Shell は対話型コンソールが、クラウドで実行されると考えます。

Cloud Shell から選択する 2 つのエクスペリエンスを提供します。 Bash と PowerShell。 Azure CLI、azure コマンド ライン インターフェイスへのアクセス機能があります。

Azure CLI、および Azure PowerShell は、Azure ポータルを含む、任意の Azure 管理インターフェイスを使用すると、任意の種類の VM を管理します。 学習目的の場合、ここを CLI を使用する Azure 作成して、Windows または Linux VM を管理します。

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Windows VM を作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

稼働している Windows VM を取得します。

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.

1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. このページの右側にある Cloud Shell から実行、 `az vm create` VM を作成するコマンド。 次に示すいずれかの後でわかりやすいパスワードを変更することをお勧めします。

      > [!NOTE]
    > 大文字と小文字の英字、数字、記号と少なくとも 8 文字を含むパスワードを選択します。

    ```azurecli
    az vm create \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --admin-username azureuser \
      --admin-password "Password1234&"
    ```

    > [!TIP]
    > 使用することができます、**コピー**各コマンドをコピー ボタンをクリックします。 これを貼り付けるには、Cloud Shell ウィンドウに新しい行を右クリックし、選択**貼り付けます**します。

    VM を 4 ~ 5 分になります。 購入、ラック取り付け、およびデータ センター内のシステムの構成にかかる時間を比較します。 かなりの違いです。

待機しているときに実行したコマンドを確認してみましょう。

* VM の名前は**myWindowsVM**します。 この名前は、Azure で VM を識別します。 これはまた、VM の内部ホスト名、またはコンピューター名になります。
* リソース グループ、または、VM の論理的なコンテナーの名前が **<rgn>[サンド ボックス リソース グループ名]</rgn>** します。
* **Win2016Datacenter** Windows Server 2016 VM イメージを指定します。
* **Standard_DS2_v2** VM のサイズを示します。 このサイズは、2 つの仮想 Cpu と 7 GB のメモリがします。
* ユーザー名とパスワードを使用すると、後で、VM に接続できます。 たとえばを使用して、システムを構成するには、リモート デスクトップまたは WinRM 経由で接続できます。

既定では、Azure は、VM にパブリック IP アドレスを割り当てます。 インターネットまたは内部ネットワークからのみアクセスできるように VM を構成できます。

この短いビデオを作成して Vm を管理する必要があるオプションの一部についても確認できます。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

VM 準備ができたら、それに関する情報を参照してください。 次に例を示します。

```console
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myWindowsVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

::: zone-end

::: zone pivot="linux-cloud"

## <a name="create-a-linux-vm"></a>Linux VM の作成

[!include[](../../../includes/azure-sandbox-activate.md)]

稼働している Linux VM を取得します。

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.
 
1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. このページの右側にある Cloud Shell から実行、 `az vm create` VM を作成するコマンド。

    ```azurecli
    az vm create \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    > [!TIP]
    > 使用することができます、**コピー**各コマンドをコピー ボタンをクリックします。 これを貼り付けるには、Cloud Shell ウィンドウに新しい行を右クリックし、選択**貼り付けます**します。

    VM を約 2 分になります。 購入、ラック取り付け、およびデータ センター内のシステムの構成にかかる時間を比較します。 かなりの違いです。

待機しているときに実行したコマンドを確認してみましょう。

* VM の名前は**myLinuxVM**します。 この名前は、Azure で VM を識別します。 これはまた、VM の内部ホスト名、またはコンピューター名になります。
* リソース グループ、または、VM の論理的なコンテナーの名前が **<rgn>[サンド ボックス リソース グループ名]</rgn>** します。
* **UbuntuLTS** Ubuntu 16.04 LTS VM イメージを指定します。
* **Standard_DS2_v2** VM のサイズを示します。 このサイズは、2 つの仮想 Cpu と 7 GB のメモリがします。
* `--generate-ssh-keys`オプションは、VM にログインするための SSH キー ペアを作成します。

既定では、Azure は、VM にパブリック IP アドレスを割り当てます。 インターネットまたは内部ネットワークからのみアクセスできるように VM を構成できます。

この短いビデオを作成して Vm を管理する必要があるオプションの一部についても確認できます。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

VM 準備ができたら、それに関する情報を参照してください。 次に例を示します。

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myLinuxVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## <a name="summary"></a>まとめ

身に付ければ、いくつかの概念、Azure 上に VM をわずか数分で迅速に作成することができました。 VM のサイズとファイアウォール ルールなど、これらの概念の多くは見慣れたに、既にします。

次に、VM に web サーバーをインストールし、基本的な web サイトを処理するために、web サーバーを構成します。