あなたが技術の専門家なら、おそらく特定の分野についての専門知識をお持ちでしょう。 ストレージの管理者か仮想化のエキスパート、それとも最新のセキュリティ プラクティスの専門家でしょうか。 あなたが学生なら、最も興味を引かれるものをまだ探しているところかもしれません。

::: zone pivot="windows-cloud"

役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。 ここでは、Windows Server 2016 を実行する仮想マシンを稼働させます。

::: zone-end

::: zone pivot="linux-cloud"

役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。 ここでは、Ubuntu 16.04 を実行する仮想マシンを稼働させます。

::: zone-end

Azure で仮想マシンを作成するには多くの方法があります。 ここでは、Cloud Shell と呼ばれる対話型ターミナルを使用して、Windows または Linux の仮想マシンを作成します。 日常的にターミナルから作業していれば、多くの場合はこれが作業を行う最も速い方法であることをご存知でしょう。

::: zone pivot="windows-cloud"

> [!TIP]
> Linux でよろしいですか、それとも何か新しいものを試したいですか。 Linux 仮想マシンを実行するには、このページの上部で **Linux** を選択します。

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Windows でよろしいですか、それとも何か新しいものを試したいですか。 Windows Server 仮想マシンを実行するには、このページの上部で **Windows** を選択します。

::: zone-end

いくつかの基本的な用語を確認し、初めての仮想マシンを稼働させてみましょう。

## <a name="what-is-a-virtual-machine"></a>仮想マシンとは

仮想マシン (VM) とは、物理コンピューターのソフトウェア エミュレーションです。 VM はソフトウェアとして存在するので、数十、数百、あるいは数千もの Azure VM を分単位で生成し、不要になったら削除することができます。 低コストで分単位の課金なので、使用している間だけ、使用したコンピューティング リソースに対してのみ料金を支払います。 さらに、ニーズに合わせて VM を構成する多くの方法があります。

::: zone pivot="windows-cloud"

実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。 Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。 独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。 ここでは、Microsoft によって提供される Windows Server 2016 VM を稼働させます。

::: zone-end

::: zone pivot="linux-cloud"

実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。 Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。 独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。 ここでは、Canonical によって提供される Ubuntu 16.04 VM を稼働させます。

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Azure 上の仮想マシンを定義するもの

仮想マシンは、サイズや場所など、さまざまな要素によって定義されます。 VM を稼働させる前に、関係するものを簡単に説明しておきます。

:::row:::
    :::column:::
        **サイズ**
    :::column-end:::
    :::column span="3"::: VM の "_サイズ_" によって、プロセッサの速度、メモリの量、ストレージの初期量、および予想されるネットワーク帯域幅が定義されます。 一部のサイズには、大量のグラフィックス レンダリングやビデオ編集用の GPU などの特殊なハードウェアも含まれます。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **リージョン**
    :::column-end:::
    :::column span="3"::: Azure は、世界中に分散するデータ センターによって構成されます。 "_リージョン_" とは、名前付きの地理的な場所における Azure データ センターのセットです。 仮想マシンを含むすべての Azure リソースには、リージョンが割り当てられます。 米国東部や北ヨーロッパなどがリージョンの例です。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **ネットワーク**
    :::column-end:::
    :::column span="3"::: "_仮想ネットワーク_" とは、Azure 上で論理的に分離されたネットワークのことです。 Azure 上の各仮想マシンには、仮想ネットワークが関連付けられます。 Azure では、"_ネットワーク セキュリティ グループ_" と呼ばれる仮想ネットワークに対して、クラウド レベルのファイアウォールが提供されます。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **リソース グループ**
    :::column-end:::
    :::column span="3"::: 仮想マシンと他のクラウド リソースは、"_リソース グループ_" と呼ばれる論理的なコンテナーにグループ化されます。 グループは、通常、アプリケーションまたはサービスの一部としてまとめてデプロイされるリソースの整理に使用されます。 リソース グループを参照するにはその名前を使用します。
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Azure Cloud Shell とは

Azure Cloud Shell とは、Azure リソースを管理および開発するための、ブラウザー ベースのコマンド ライン エクスペリエンスです。 Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。

Cloud Shell では、2 つのエクスペリエンス Bash と PowerShell のどちらかを選択できます。 どちらからも、Azure 用のコマンド ライン インターフェイスである Azure CLI にアクセスできます。

Azure portal、Azure CLI、Azure PowerShell などの Azure 管理インターフェイスを使用して、すべての種類の VM を管理できます。 学習が目的なので、ここでは Azure CLI を使用して Windows または Linux の VM の作成と管理を行います。

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Windows VM を作成する

Windows VM を稼働させてみましょう。

1. このページの右側にある Cloud Shell から `az group create` コマンドを実行し、**myResourceGroup** という名前のリソース グループを米国東部リージョンに作成します。

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```

1. `az vm create` コマンドを実行して、VM を作成します。 次に示すパスワードを、後で覚えやすいものに変更することをお勧めします。

      > [!NOTE]
    > 大文字と小文字の英字、数字、記号を組み合わせて 8 文字以上のパスワードを選択します。

    ```azurecli
    az vm create \
      --name myWindowsVM \
      --resource-group myResourceGroup \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser \
      --admin-password "Password1234&"
    ```

    > [!TIP]
    > **[コピー]** ボタンを使用して、各コマンドをコピーできます。 コマンドを貼り付けるには、Cloud Shell ウィンドウで新しい行を右クリックし、**[貼り付け]** を選択します。

    VM の作成には 4 から 5 分かかります。 それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。 まったく違います。

待っている間に、実行したコマンドを確認してみましょう。

* VM の名前は **myWindowsVM** です。 この名前によって Azure 内で VM が識別されます。 これはまた、VM の内部ホスト名またはコンピューター名にもなります。
* リソース グループつまり VM の論理的なコンテナーの名前は **myResourceGroup** です。
* **Win2016Datacenter** は、Windows Server 2016 VM イメージを指定します。
* **Standard_DS2_v2** は、VM のサイズを示します。 このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。
* VM が存在する場所つまりリージョンは、**米国東部**です。
* 後でユーザー名とパスワードを使用して、VM に接続できます。 たとえば、リモート デスクトップまたは WinRM 経由で接続し、システムを使用したり構成したりできます。

既定では、VM にはパブリック IP アドレスが割り当てられます。 インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。

この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

VM の準備ができたら、それについての情報を表示します。 次に例を示します。

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

## <a name="create-a-linux-vm"></a>Linux VM を作成する

Linux VM を稼働させてみましょう。

1. このページの右側にある Cloud Shell から `az group create` コマンドを実行し、**myResourceGroup** という名前のリソース グループを米国東部リージョンに作成します。

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```

1. `az vm create` コマンドを実行して、VM を作成します。

    ```azurecli
    az vm create \
      --name myLinuxVM \
      --resource-group myResourceGroup \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --location eastus \
      --generate-ssh-keys
    ```

    > [!TIP]
    > **[コピー]** ボタンを使用して、各コマンドをコピーできます。 コマンドを貼り付けるには、Cloud Shell ウィンドウで新しい行を右クリックし、**[貼り付け]** を選択します。

    VM の作成には約 2 分かかります。 それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。 まったく違います。

待っている間に、実行したコマンドを確認してみましょう。

* VM の名前は **myLinuxVM** です。 この名前によって Azure 内で VM が識別されます。 これはまた、VM の内部ホスト名またはコンピューター名にもなります。
* リソース グループつまり VM の論理的なコンテナーの名前は **myResourceGroup** です。
* **UbuntuLTS** は、Ubuntu 16.04 LTS VM イメージを指定します。
* **Standard_DS2_v2** は、VM のサイズを示します。 このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。
* VM が存在する場所つまりリージョンは、**米国東部**です。
* `--generate-ssh-keys` オプションでは、VM にログインできる SSH キー ペアが作成されます。

既定では、VM にはパブリック IP アドレスが割り当てられます。 インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。

この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

VM の準備ができたら、それについての情報を表示します。 次に例を示します。

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

いくつかの概念を理解するだけで、わずか数分で Azure 上で VM を稼働させることができます。 VM のサイズやファイアウォール規則などのこれらの概念の多くは、既に見慣れているはずです。

次に、VM に Web サーバーをインストールし、基本的な Web サイトを提供するように Web サーバーを構成します。