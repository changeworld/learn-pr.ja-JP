この演習では、Microsoft Azure で仮想ネットワークを作成します。 2 つの仮想マシンを作成し、仮想ネットワークを使用して、仮想マシンを相互に、またインターネットに接続します。

このユニットを開始する前にログインする必要があります。 [Azure Cloud Shell](https://shell.azure.com)試用版サブスクリプション資格情報を使用します。 リソース グループ、仮想ネットワーク、および仮想マシンを作成するのに Azure Cloud Shell を使用して Azure CLI を使用します。

## <a name="create-a-resource-group"></a>リソース グループの作成

1. **Azure Cloud Shell へようこそ**ウィンドウで、をクリックして**Bash (Linux)** します。

1. **[ストレージがマウントされていません]** ウィンドウで、**[ストレージの作成]** をクリックします。

1. Azure PowerShell のコマンド ライン プロンプトで次のコードを入力し、Enter キーを押します。 置換、`<myResourceGroup>`を後で作成されたすべてのリソースをクリーンアップするときに覚えておくが簡単にわかりやすい名前を持つ値。 このラボのリセットでは、この名前を使用します。

    ```azurecli
    az group create --name <myResourceGroup> --location eastus
    ```

## <a name="create-a-virtual-network"></a>仮想ネットワークの作成

1. 仮想ネットワークを作成するには、次のコマンドを入力し、Enter キーを押します。 置換、`<myVirtualNetwork>`覚えやすいようにわかりやすい名前を持つ値

    ```azurecli
    az network vnet create --name <myVirtualNetwork> --resource-group <myResourceGroup> --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a>2 つの仮想マシンの作成

1. 最初の仮想マシンを作成するには、ポート 3389 (リモート デスクトップ) 経由でアクセスできるパブリック IP アドレスを持つ Windows VM を作成する次のコマンドを実行します。 VM の名前`dataProcStage1`:

    ```azurecli
    az vm create --name dataProcStage1 --resource-group <myResourceGroup> --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. プロンプトで、パスワードの値を指定します。 サーバーにアクセスするには、後で必要になるとは、このパスワードをメモしてください。

1. 2 つ目の VM を作成します。 この VM のパブリック IP アドレスではありません。 Windows VM を作成する次のコマンドを実行**せず**空の文字列を使用してパブリック IP アドレス。 VM の名前`dataProcStage2`:

    ```azurecli
    az vm create -n dataProcStage2 -g <myResourceGroup> --public-ip-address "" --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. 完了すると、2 番目のコマンドからの出力で publicIpAddress の値が返されます。  

## <a name="connect-to-dataprocstage1-using-remote-desktop"></a>DataProcStage1 に接続するリモート デスクトップの使用

1. クライアント コンピューターで、Windows キーを押し、「RDP」と入力します。

1. いることを確認**リモート デスクトップ接続**アプリが選択されているし、し、Enter キーを押します。

1. **リモート デスクトップ接続** ダイアログ ボックスで、**コンピューター**フィールドの値を入力`dataProcStage1`クリックして、PublicIPAddress の**Connect**します。
    
    手順については、に記述されていない場合は、リモート ip を取得するには

1. **[このリモート接続を信頼しますか?]** ダイアログ ボックスで、**[接続]** をクリックします。

1. **Windows セキュリティ** ダイアログ ボックスで、ユーザー名とパスワードを作成したときに使用した入力`dataProcStage1`します。 

    ここでのプロファイルを変更する必要があります。

1. **[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。

1. Azure 内でリモート コンピューターにサインインします。

1. "**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。

1. サーバー マネージャーを閉じます。

1. リモート セッションで Windows キーを右クリックし、をクリックして**コマンド プロンプト**します。

1. コマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。

    ```cmd
    ping dataProcStage2 -4
    ```

1. 応答ことはありません`dataProcStage2`します。 これは、既定では、Windows ファイアウォールがの ICMP 応答を防止するため`dataProcStage2`します。

## <a name="connect-to-dataprocstage2-using-remote-desktop"></a>DataProcStage2 に接続するリモート デスクトップの使用

Windows ファイアウォールを構成します`dataProcStage2`新しいリモート デスクトップ seesion を使用します。 ただし、アクセスできないことを学習`dataProcStage2`デスクトップから。 リコール、`dataProcStage2`はパブリック IP アドレスがありません。 リモート デスクトップを使用する`dataProcStage1`への接続に`dataProcStage2`します。

1. `dataProcStage1`、Windows キーと型のキーを押して**RDP**します。 **[リモート デスクトップ接続]** アプリを選択し、Enter キーを押します。

1. **コンピューター**フィールドに、入力`dataProcStage2`、 をクリックし、 **Connect**します。 既定のネットワーク構成に基づいて`dataProcStage1`のアドレスを解決できる`dataProcStage2`コンピューター名を使用します。

1. **[このリモート接続を信頼しますか?]** ダイアログ ボックスで、**[接続]** をクリックします。

1. **Windows セキュリティ** ダイアログ ボックスで、ユーザー名とパスワードを作成したときに使用した入力`dataProcStage2`します。

1. **[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。 ここで、Azure 内でリモート コンピューターにサインインします。

1. "**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。

1. サーバー マネージャーを閉じます。

1. `dataProcStage2`、Windows キーの型のキーを押して**ファイアウォール**、Enter キーを押します。 「**セキュリティが強化された Windows ファイアウォール**」コンソールが表示されます。

1. 左側のウィンドウで、**[受信規則]** をクリックします。

1. 右側のウィンドウで下にスクロールし、右クリックして**ファイルとプリンターの共有 (エコー要求 - icmpv4 受信)**、 をクリックし、**規則の有効化**します。

1. 戻り、`dataProcStage1`コンソールしコマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。

    ```cmd
    ping dataProcStage2 -4
    ```

1. `dataProcStage2` 2 つの Vm 間の接続を示す 4 つの応答で応答します。

## <a name="summary"></a>まとめ

仮想ネットワークは、その仮想ネットワークに接続されている、Vm のいずれかに接続および同じ仮想ネットワーク内の他の VM にネットワーク接続を表示する 2 つの Vm の作成が正常に作成しました。 Azure Virtual Network を使用すると、Azure のネットワーク内のリソースを接続します。 ただし、これらのリソースは、同じリソース グループとサブスクリプションに含まれる必要があります。 次に、VPN ゲートウェイは、別のリソース グループ、サブスクリプション、および地理的地域内の仮想ネットワークを接続するためには注目します。
