この演習では、Microsoft Azure で仮想ネットワークを作成します。 2 つの仮想マシンを作成し、仮想ネットワークを使用して、仮想マシンを相互に、またインターネットに接続します。

このユニットを開始する前に、試用版サブスクリプション資格情報を使用して [Azure Cloud Shell](https://shell.azure.com) にログインする必要があります。 Azure Cloud Shell を使用して、リソース グループ、仮想ネットワーク、仮想マシンを作成します。

## <a name="create-a-resource-group"></a>リソース グループの作成

1. **[Azure Cloud Shell へようこそ]** ウィンドウで **[PowerShell (Linux)]** をクリックします。

1. **[ストレージがマウントされていません]** ウィンドウで、**[ストレージの作成]** をクリックします。

1. Azure PowerShell コマンド ラインのプロンプトで、次のコードを入力し、Enter キーを押します。

    ```PowerShell
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-virtual-network"></a>仮想ネットワークを作成する

1. 仮想ネットワークを作成するには、次のコマンドを入力し、Enter キーを押します。

    ```PowerShell
    az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a>2 つの仮想マシンの作成

1. 最初の仮想マシンを作成するには、次のコマンドを実行して、ポート 3389 (リモート デスクトップ) 経由でアクセスできるパブリック IP アドレスを持つ Windows VM を作成します。

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. プロンプトで、パスワードの値を指定します。

1. パブリック IP アドレスがない Windows VM を作成するには次のコマンドを実行します。

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. 完了すると、2 番目のコマンドからの出力で publicIpAddress の値が返されます。

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a>リモート デスクトップを使用して dataProcessingStage1 に接続する

1. クライアント コンピューターで、Windows キーを押し、「RDP」と入力します。

1. **リモート デスクトップ接続**アプリが選択されていることを確認し、Enter キーを押します。

1. **[リモート デスクトップ接続]** ダイアログ ボックスの **[コンピューター]** フィールドで、dataProcessingStage1PublicIPAddress の値を入力してから、**[接続]** をクリックします。

1. **[Do you trust this remote connection?]\(このリモート接続を信頼しますか?\)** ダイアログ ボックスで、**[接続]** をクリックします。

1. **[Windows セキュリティ]** ダイアログ ボックスで、dataProcessingStage1 の作成時に使用したユーザー名とパスワードを入力します。

1. **[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。

1. Azure 内でリモート コンピューターにサインインします。

1. "**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。

1. サーバー マネージャーを閉じます。

1. リモート セッションで Windows キーを右クリックし、**[コマンド プロンプト]** をクリックします。

1. コマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. リモート コンピューターからの応答はありません。 これは、既定では Windows ファイアウォールが ICMP 応答を阻止するからです。

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a>リモート デスクトップを使用して dataProcessingStage2 に接続する

1. クライアント コンピューターで、Windows キーを押し、「**RDP**」と入力します。 **[リモート デスクトップ接続]** アプリを選択し、Enter キーを押します。

1. **[コンピューター]** フィールドで、dataProcessingStage2PublicIPAddress の値を入力してから、**[接続]** をクリックします。

1. **[Do you trust this remote connection?]\(このリモート接続を信頼しますか?\)** ダイアログ ボックスで、**[接続]** をクリックします。

1. **[Windows セキュリティ]** ダイアログ ボックスで、dataProcessingStage2 の作成時に使用したユーザー名とパスワードを入力します。

1. **[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。 ここで、Azure 内でリモート コンピューターにサインインします。

1. "**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。

1. サーバー マネージャーを閉じます。

1. dataProcessingStage2 で、Windows キーを押して「**ファイアウォール**」と入力し、Enter キーを押します。 「**セキュリティが強化された Windows ファイアウォール**」コンソールが表示されます。

1. 左側のウィンドウで、**[受信規則]** をクリックします。

1. 右側のウィンドウで下にスクロールし、**[ファイルとプリンターの共有 (エコー要求 - ICMPv4 受信)]** を右クリックし、**[規則の有効化]** をクリックします。

1. dataProcessingStage1 コンソールに戻り、コマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. dataProcessingStage2 は、4 つの返信で応答し、2 つの VM 間の接続を示します。

## <a name="summary"></a>まとめ

仮想ネットワークを正常に作成し、その仮想ネットワークに接続される 2 つの VM を作成し、一方の VM に接続して、同じ仮想ネットワーク内の他の VM へのネットワーク接続を表示しました。 Azure Virtual Network を使用して、Azure ネットワーク内のリソースを接続できます。 ただし、これらのリソースは、同じリソース グループとサブスクリプションに含まれる必要があります。 次に、VPN ゲートウェイを参照します。これにより、別のリソース グループ、サブスクリプション、地理的リージョン内の仮想ネットワークを接続できます。
