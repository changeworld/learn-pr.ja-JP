この演習では、Azure でホストされている Ubuntu Linux 仮想マシンに Node.js (頭字語 MEAN の **N**) をインストールします。 Node.js は、HTTP トラフィックを処理し、Web アプリケーションをホストするためのランタイムとして機能します。

## <a name="connect-to-the-vm"></a>VM に接続する

Node.js をインストールするには、**ssh** を使用して VM に接続する必要があります。 まだ VM に接続していない場合は、次のコマンドを実行します。 `<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご自分の VM のパブリック IP アドレスに置き換えます。

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a>Node.js をインストールする

> [!Important]
> Ubuntu には **Node.js-legacy** という非公式のパッケージが用意されていますが、このパッケージは Node.js によって保守されておらず、古い内容です。

1. ご使用の仮想マシンにインストールできる適切なパッケージを **apt-get** で検出できるように、Node.js パッケージ リポジトリを登録します。

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. Linux システムに Node.js パッケージをインストールします。

    ```bash
    sudo apt-get install -y Node.js
    ```

1. 次の単純な Node.js コマンドを実行して、Node.js のインストールが成功したことを確認します。

    ```bash
    node -v
    ```

    出力は `v8.11.4` のようになります。パッケージのインストール時点で最新の Node.js バージョンが反映されたバージョンが使用できます。

## <a name="summary"></a>まとめ

仮想マシンに Node.js をインストールすると、実行を担当する Web アプリケーションを構築できるようになります。
