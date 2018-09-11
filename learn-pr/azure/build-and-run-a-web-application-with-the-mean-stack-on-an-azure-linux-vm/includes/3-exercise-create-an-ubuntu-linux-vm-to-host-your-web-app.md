コンポーネントの MEAN スタックには、サーバーが必要です。 これは、ご使用のサーバー ルームで実行している Linux マシンや仮想マシンでも、それをクラウドベースの仮想マシンに構成するのでもかまいません。 このモジュールでは、Azure で実行されている Ubuntu Linux 仮想マシンで実行するスタックを設定します。

この演習では、Azure にホストされる、新しい Ubuntu Linux 仮想マシンを作成します。 MEAN スタック コンポーネントは、既存の仮想マシンや物理ホスト マシンにインストールすることも可能です。 この演習では新しいものを作成することにより、すべてのコンポーネントを Azure リソース グループにまとめて、演習の完了後の管理とクリーンアップを簡単にすることができます。

Linux VM は、Azure portal に統合されている Cloud Shell コマンドラインを使用して作成します。

## <a name="provision-an-ubuntu-linux-vm"></a>Ubuntu Linux VM をプロビジョニングする

1. [Azure portal](https://portal.azure.com?azure-portal=true) にアクセスします。
1. Azure portal のツールバーの山かっこ (>_) アイコンから Cloud Shell を開きます。
1. Cloud Shell でコマンドを実行し、使用する VM が含まれる Azure リソース グループを作成します。 `<resource-group-name>` をご自分のリソース グループ名に、また `<resource-group-location>` をご希望の Azure の場所 (`westus` など) に置き換えます。


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    ご自分のリソース グループ名は、その他のコマンドでも使用するので覚えておきます。

1. Cloud Shell で次のコマンドを実行して、新しい Ubuntu Linux VM を作成します。 `<resource-group-name>` をご自分のリソース グループ名に、また `<vm-admin-username>` と `<vm-admin-password>` をご希望の管理ユーザー名とパスワードに置き換えます。

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
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
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    VM に接続するには、新たに作成した VM のパブリック IP アドレスも保存したおいた方がよいでしょう。

1. ご利用の新しい VM への接続を試します。

    コマンド プロンプトまたはターミナル ウィンドウを開き、次のコマンドを実行します。 `<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご利用の VM のパブリック IP アドレスに置き換えます。

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    マシンに最初に接続したとき、そのリモート マシンを信頼するかたずねられます。 `yes` と回答すると、マシンの ECDSA キー フィンガープリントがローカルに保存されるので、それ以降の接続は信頼されるものとなります。

    すべてに問題がないようであれば、`exit` と入力し、SSH セッションを閉じます。

1. ポート 80 を開き、作成する新しい Web アプリケーションへの受信 HTTP トラフィックを許可します。

    Azure portal 上で、Cloud Shell に戻ります。 `<resource-group-name>` の元のリソース グループ名を使用して、次のコマンドを発行します。

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    このコマンドにより、作成時に "MeanDemo" と命名された HTTP ポートがご利用の VM で開きます。

## <a name="summary"></a>まとめ

新しい Ubuntu Linux VM の準備が完了したら、それに接続して、MEAN スタックのさまざまなコンポーネントのインストールを開始できます。