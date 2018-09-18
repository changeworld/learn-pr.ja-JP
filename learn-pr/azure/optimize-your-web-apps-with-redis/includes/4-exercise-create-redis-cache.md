よく使用される値を格納して返すために、Azure Redis Cache インスタンスを作成しましょう。

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a>Azure で Redis Cache を作成する

1. [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。

1. **[リソースの作成]**、**[データベース]**、**[Redis Cache]** の順にクリックします。

    次のスクリーンショットは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。

    ![[リソースの作成]、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal のデータベース オプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a>キャッシュの場所を特定する

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a>キャッシュを構成する

キャッシュに次の設定を適用します。

1. **DNS 名:** **ContosoSportsApp1028** など、グローバルに一意の名前を作成します。

1. **サブスクリプション:** Azure サンドボックス サブスクリプションを選択します。

1. **リソース グループ:** リソース グループの<rgn>サンドボックス リソース グループ名</rgn>を選択します。

1. **場所:** 通常は、顧客に近い場所 (この例では東海岸) を選択します。 ただし、Azure サンドボックスでは、上記のようにリソースには特定のリージョンのみを選択できます。 それらの場所のいずれかを選択してください。

1. **価格レベル:** **[Basic C0]** を選択します。 これは使用できる最下位レベルです。 運用環境のアプリでは、より多くのデータを格納し、高いレベルを選択する必要があるクラスタリングなどの Premium 機能を利用する可能性があります。

1. **[作成]** をクリックします。

    次のスクリーンショットは、新しい Redis Cache リソースの作成に使用された、代表的な構成を示しています。 構成は、Azure サンドボックスによって若干異なります。

    ![DNS 名、サブスクリプション、新しいリソース グループ、場所、価格レベルの構成例が入力された、新しい Redis Cache リソースを作成するときの Azure portal のブレードを示すスクリーンショット。](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> 続行する前に、キャッシュがデプロイされるまで待つ必要があります。 このプロセスにはしばらく時間がかかることがあります。

## <a name="retrieve-the-access-keys-and-host-name"></a>アクセス キーおよびホスト名を取得する

1. Azure portal で新しいキャッシュ インスタンスに移動し、**[設定]** > **[アクセス キー]** を選択します。 

1. **プライマリ接続文字列 (StackExchange.Redis)** を安全な場所にコピーします。次の演習で必要になります。

    このキーには、使用する **StackExchange.Redis** パッケージのアプリケーション設定内で使用する完全な接続文字列のプライマリ キーとホスト名が含まれます。

次に、キャッシュの問い合わせに使用できるコマンドについて学習しましょう。