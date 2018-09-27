よく使用される値を格納して返すために、Azure Redis Cache インスタンスを作成しましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a>Azure で Redis Cache を作成する

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

1. **[リソースの作成]**、**[データベース]**、**[Redis Cache]** の順にクリックします。

    次のスクリーンショットは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。

    ![[Create a resource]\(リソースの作成\)、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal データベースのオプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a>キャッシュを構成する

キャッシュに次の設定を適用します。

1. **DNS 名:** **ContosoSportsApp[nnn]** のように、グローバルに一意の名前を作成します。`[nnn]` はランダムな数字に置き換えられます。

1. **サブスクリプション:** コンシェルジェ サブスクリプションを選択します。

1. **リソース グループ:** リソース グループの <rgn>[サンドボックス リソース グループ名]</rgn> を選択します。

1. **場所:** 通常は、顧客に近い場所 (この例では東海岸) を選択します。

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. **価格レベル:** **[Basic C0]** を選択します。 これは使用できる最下位レベルです。 運用環境のアプリでは、より多くのデータを格納し、高いレベルを選択する必要があるクラスタリングなどの Premium 機能を利用する可能性があります。

1. **[作成]** をクリックします。 Azure によって、自分の Redis Cache インスタンスが作成されデプロイされます。

    > [!IMPORTANT]
    > 通常、Redis Cache リソースは Azure portal で非常に迅速に作成され表示されますが、キャッシュ自体は数分間使用できなくなります。 次の手順では、キャッシュの状態を確認する方法を示します。

## <a name="verify-your-data"></a>データの検証

Azure portal の**コンソール**機能を使用して、Redis Cache が作成された後に、このキャッシュに対するコマンドを発行します。

1. デプロイの完了時の **[通知]** ポップアップ経由、または左側のサイドバーで **[すべてのリソース]** を選択して Redis Cache を見つけ、左のフィルター ボックスを使用して Redis Cache インスタンスを選択します。 または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。

1. Redis Cache インスタンスを選択します。

1. "Status" フィールドの値を確認します。 状態が "Running" になるまで、キャッシュの準備ができていません。 続行する前に、数分間待機しなければならない場合があります。

1. キャッシュが実行されたら、Redis Cache の **[概要]** ブレードのツールバーにある [`>_ Console`] ボタンをクリックします。 これにより、詳細な Redis コマンドを入力できる Redis コンソールが開きます。 次のいくつかの操作を試します。

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

上部の階層リンク バーを使用するか、またはスクロールバーを使用してビューを左へスライドし、**[概要]** パネルに戻ります。

## <a name="retrieve-the-access-keys-and-host-name"></a>アクセス キーおよびホスト名を取得する

1. **[設定]** > **[アクセス キー]** の順に選択します。

1. **プライマリ接続文字列 (StackExchange.Redis)** を安全な場所にコピーします。次の演習で必要になります。

    このキーには、使用する **StackExchange.Redis** パッケージのアプリケーション設定内で使用する完全な接続文字列のプライマリ キーとホスト名が含まれます。

次に、キャッシュの問い合わせに使用できるコマンドについて学習しましょう。