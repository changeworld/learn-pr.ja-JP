コンポーネントの MEAN スタックには、サーバーが必要です。 これは、ご使用のサーバー ルームで実行している Linux マシンや仮想マシンでも、それをクラウドベースの仮想マシンに構成するのでもかまいません。 このモジュールでは、Azure で実行されている Ubuntu Linux 仮想マシンで実行するスタックを設定します。

このユニットでは、Azure でホストされている、新しい Ubuntu Linux 仮想マシンが作成されます。 MEAN スタック コンポーネントは、既存の仮想マシンや物理ホスト マシンにインストールすることも可能です。 この演習では新しいものを作成することにより、すべてのコンポーネントを Azure リソース グループにまとめて、演習の完了後の管理とクリーンアップを簡単にすることができます。

## <a name="provision-an-ubuntu-linux-vm"></a>Ubuntu Linux VM をプロビジョニングする

[!include[](../../../includes/azure-sandbox-activate.md)]

<!--
TODO: Omitting for sandbox. Keeping here for possible later inclusion.

1. In Cloud Shell, execute the command to create an Azure resource group, which will include our VM. Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).


    ```azurecli
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Remember your resource group name, as we will use it in other commands.
-->

1. Cloud Shell で次のコマンドを実行して、新しい Ubuntu Linux VM を作成します。 優先管理ユーザー名とパスワードに置き換えてください`<vm-admin-username>`と`<vm-admin-password>`します。

    ```azurecli
    az vm create \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    この VM に後で接続できるよう、管理者名とパスワードをメモしておきます。

    このコマンドの完了には約 2 分かかります。 コマンドの完了の結果の出力は次のようになります。

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[Sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    VM に接続するには、新たに作成した VM のパブリック IP アドレスも保存したおいた方がよいでしょう。

1. ご利用の新しい VM への接続を試します。

    Cloud Shell から次のコマンドを実行します。 `<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご自分の VM のパブリック IP アドレスに置き換えます。

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    マシンに最初に接続したとき、そのリモート マシンを信頼するかたずねられます。 `yes` と回答すると、マシンの ECDSA キー フィンガープリントがローカルに保存されるので、それ以降の接続は信頼されるものとなります。

    すべてに問題がないようであれば、`exit` と入力し、SSH セッションを閉じます。

1. 作成する新しい web アプリケーションへの受信 HTTP トラフィックを許可するように VM 上のポート 80 を開きます。

    ``` bash
    az vm open-port --port 80 --resource-group <rgn>[Sandbox resource group name]</rgn> --name MeanDemo
    ```

    このコマンドにより、作成時に "MeanDemo" と命名された HTTP ポートがご利用の VM で開きます。

## <a name="summary"></a>まとめ

新しい Ubuntu Linux VM の準備が完了したら、それに接続して、MEAN スタックのさまざまなコンポーネントのインストールを開始できます。