このユニットでは、今後のサンプル Web アプリケーションのデータ ストアとして動作する MongoDB を Ubuntu Linux 仮想マシンにインストールします。

## <a name="install-mongodb"></a>MongoDB をインストールする

1. Cloud Shell から SSH で VM に接続します。

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. MongoDB リポジトリの暗号化キーをインポートします。 これで、インストールする mongodb パッケージが MongoDB Inc. からリリースされたものであることをパッケージ マネージャーで確認できるようになります。

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    **sudo** コマンドは、管理特権を使用して、指定されたコマンドを実行することを意味します。

1. パッケージ マネージャーで mongodb パッケージを検出できるように、MongoDB Ubuntu リポジトリを登録します。

    > [!NOTE]
    > このコマンドは Ubuntu のバージョンによって異なります。 使用している Ubuntu のバージョンを確認するには、`uname -v` を実行します。
    > このコマンドの出力は、`#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018` のようになります。
    >
    > この出力は、Ubuntu バージョン 16.04.1 を実行していることを示しています。
    > 使用しているバージョンの正確なコマンドを確認するには、[MongoDB Community Edition を Ubuntu にインストールする方法](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)に関するドキュメントを参照してください。

    Ubuntu 16.04 上で次のコマンドを実行します。

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. 最新のパッケージ情報が得られるように、パッケージ データベースを更新します。

    ```bash
    sudo apt-get update
    ```

1. MongoDB パッケージを VM にインストールします。

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. MongoDB サービスを開始して、後で接続できるようにします。

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a>まとめ

Ubuntu Linux VM に MongoDB をインストールしました。 MongoDB は、Web アプリケーションで保存および取得した情報のバッキング データ ストアとして機能します。