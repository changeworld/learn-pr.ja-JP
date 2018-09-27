コンポーネントの MEAN スタックには、サーバーが必要です。 これは、ご使用のサーバー ルームで実行している Linux マシンや仮想マシンでも、それをクラウドベースの仮想マシンに構成するのでもかまいません。 このモジュールでは、Azure で実行されている Ubuntu Linux 仮想マシンで実行するスタックを設定します。

このユニットでは、Azure にホストされる、新しい Ubuntu Linux 仮想マシンを作成します。 MEAN スタック コンポーネントは、既存の仮想マシンや物理ホスト マシンにインストールすることも可能です。 この演習では新しいものを作成することにより、すべてのコンポーネントを Azure リソース グループにまとめて、演習の完了後の管理とクリーンアップを簡単にすることができます。

## <a name="provision-an-ubuntu-linux-vm"></a>Ubuntu Linux VM をプロビジョニングする

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a>リソース グループを作成する

通常、新しい一連のリソースを作成するときにまず行うことは、_リソース グループ_を作成してそれらすべてを所有することです。 これは Azure サンドボックスでは不要な手順ですが、独自のサブスクリプションで作業している場合は、次のコマンドを使用して、ご自分の近くの場所にリソース グループを作成します。

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> Azure サンドボックスでは、リソース グループを作成する必要はありません。 代わりに、**<rgn>[サンドボックス リソース グループ名]</rgn>** という名前の事前に作成されたリソース グループを使用します。

1. 右側にある Cloud Shell で、次のコマンドを入力して、新しい Ubuntu Linux VM を作成します。 `<vm-admin-username>` および `<vm-admin-password>` をご希望の管理ユーザー名とパスワードに置き換えます。

    ```azurecli
    az vm create \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
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
        "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    VM に接続するには、新たに作成した VM のパブリック IP アドレスも保存したおいた方がよいでしょう。

1. 新しい VM への接続を試します。

    Cloud Shell で次のコマンドを実行します。 `<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご利用の VM のパブリック IP アドレスに置き換えます。

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    マシンに最初に接続したとき、そのリモート マシンを信頼するかたずねられます。 `yes` と回答すると、マシンの ECDSA キー フィンガープリントがローカルに保存されるので、それ以降の接続は信頼されるものとなります。 パスワードの入力が求められます。これは接続するたびに表示されます。

    すべてに問題がないようであれば、`exit` と入力し、SSH セッションを閉じます。

1. VM でポート 80 を開き、作成する新しい Web アプリケーションへの受信 HTTP トラフィックを許可します。

    ```azurecli
    az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name MeanDemo
    ```

    このコマンドにより、作成時に "MeanDemo" と命名された HTTP ポートがご利用の VM で開きます。

## <a name="summary"></a>まとめ

新しい Ubuntu Linux VM の準備が完了したら、それに接続して、MEAN スタックのさまざまなコンポーネントのインストールを開始できます。