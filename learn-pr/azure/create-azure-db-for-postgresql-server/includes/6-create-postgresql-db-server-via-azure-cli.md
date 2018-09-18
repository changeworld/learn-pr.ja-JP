ランナーのフィットネス デバイスからキャプチャされたルートを格納するための Azure Database for PostgreSQL を作成します。 取得されたデータ ボリュームの履歴から、サーバー ストレージの要件を 20 GB に設定する必要があることがわかっています。 この処理要件をサポートするには、仮想コアを 1 個備えた Compute Gen 5 のサポートが必要です。 また、データのバックアップ用に 15 日間の保有期間が必要であることもわかっています。

> [!TIP]
> Microsoft Learn の演習はすべて無料ですが、自分で始めるときは、Azure サブスクリプションが必要になります。 サブスクリプションをお持ちでない場合、[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。これは数分で作成できます。

早速始めましょう。

[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。

Azure Cloud Shell セッションを開始する必要があることを思い出してください。 画面の上部にある Cloud Shell アイコンを選択してセッションを開始します。

![Cloud Shell ボタン](../media-draft/cloud-shell-button.png)

Cloud Shell で使用するストレージ アカウントがまだない場合は、初めてアクセスするときに作成する必要があります。 ポータル インターフェイスの手順に従って、ストレージ アカウントを作成します。

このラボでは、コマンド ライン環境として `bash` を使用します。

1. サーバーの作成に使用するサブスクリプションを選択します。

    複数のサブスクリプションがある場合は、次のコマンドを使用して適切なサブスクリプションをアクティブにします。0 はお使いのサブスクリプション ID に置き換えます。

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    `az account list --output table` コマンドを使用して、すべてのサブスクリプションを一覧表示できることを思い出してください。 使用するサブスクリプション ID をこの一覧から選択します。

1. 前のユニットでまだ行っていない場合は、リソース グループを作成します。 次のコマンドを実行します。

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > `az account list-locations` コマンドを使用して、すべての場所の一覧を取得できます。 値 `displayName` または `name` を選択し、`<location>` パラメーターに使用します。

1. これで、`az postgres server create` コマンドを実行する準備ができました。

    ストレージ サイズ 20 GB、1 仮想コアの Compute Gen 5 サポート、データ バックアップの保有期間 15 日に、サーバーを設定することに留意してください。

    指定するパラメーターがいくつかあります。

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    下のソリューションを見なくてもコマンドを記述してパラメーターを完了できるかどうかを確認してください。 `<>` の値は実際の値に置き換えます。

    > [!NOTE]
    > `az account list-locations` コマンドを使用して、すべての場所の一覧を取得できます。 値 `displayName` または `name` を選択し、`<location>` パラメーターに使用します。

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

実行時にはシステムによる情報の処理にしばらく時間がかかります。 サーバーが作成されると、サーバーを記述する JavaScript Object Notation (JSON) 文字列が返されます。 サーバーが作成されない場合は、エラー メッセージが表示されます。 このエラー情報を使用し、コマンド パラメーターを確認して修正します。

Azure CLI を使用して PostgreSQL サーバーを正常に作成しました。 次のユニットでは、サーバーのセキュリティ設定を構成する方法を説明します。
