Woodgrove Bank で働いていて、オンライン銀行サービスを立ち上げようとしていたことを思い出してください。 このセクターは競争が激しいので、99.99% 以上のサービス可用性を保証する必要があります。 あなたは、3 つの仮想マシンを含むプールを使用する Azure Load Balancer でこの目標が達成されると判断しました。

この演習では、Azure portal を使用してロード バランサーと仮想ネットワークを作成します。 必要なのは 1 つだけなので、ポータルで作成するのが簡単です。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a>パブリック ロード バランサーを作成する

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

1. サイドバーで **[リソースの作成]** をクリックします。

1. **[ネットワーキング]** セクションを選択し、**[Load Balancer]** をクリックします。 選択肢が表示されない場合は、検索ボックスを使用できます。

    ![[ネットワーキング] セクションが選択されて [Load Balancer] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace.png)

1. **[ロード バランサーの作成]** ブレードで次の情報を入力または選択します。
    - **名前:** _woodgrove-LB_
    - **種類:** _パブリック_
    - **SKU:** _Basic_
    - **パブリック IP アドレス**: **[新規作成]** を選択します。 テキスト ボックスに「_woodgrove-LB-ip_」と入力します。 [割り当て] は _[動的]_ のままにします。
    - **サブスクリプション:** "_コンシェルジェ サブスクリプション_" が既に選択されているはずです。
    - **リソース グループ**: **[既存のものを使用]** を選択し、_<rgn>[サンドボックス リソース グループ名]</rgn>_ を選択します。
    - **場所**: 次の一覧から、近くのリージョンを選択します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![新しいロード バランサーの作成画面のスクリーンショット](../media/3-create-load-balancer.png)

1. **[作成]** をクリックします。

ロード バランサー リソースが作成されてデプロイされている間に、バックエンド サブネットに使用する Azure 仮想ネットワークを作成しましょう。

## <a name="create-a-virtual-network"></a>仮想ネットワークを作成する

1. 左側のメニューで、**[リソースの作成]** をクリックします。 **[新規]** ブレードで **[ネットワーキング]** をクリックして、**[仮想ネットワーク]** をクリックします。

    ![[ネットワーキング] セクションが選択されて [Load Balancer] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace-2.png)

1. **[仮想ネットワークの作成]** ブレードで次の情報を入力または選択します。
    - **名前:** _woodgrove-VNET_
    - **アドレス空間:** _172.20.0.0/16_
    - **サブスクリプション:** "_コンシェルジェ サブスクリプション_" が既に選択されているはずです。
    - **リソース グループ**: 一覧から既存の _<rgn>[サンドボックス リソース グループ名]</rgn>_ リソース グループを選択します。
    - **サブネット名:** _backendSubnet_
    - **サブネットのアドレス範囲:** _172.20.0.0/24_
    - **DDoS 保護:** _Basic_
    - **サービス エンドポイント:** _無効_

    ![新しいロード バランサーの作成画面のスクリーンショット](../media/3-create-vnet.png)

1. **[作成]** をクリックします。

仮想ネットワークがデプロイされている間に、ネットワーク セキュリティ グループを作成しましょう。

## <a name="create-and-configure-a-network-security-group"></a>ネットワーク セキュリティ グループを作成して構成する

1. **[リソースの作成]** をクリックします

1. **[ネットワーキング]** グループを選択して、**[ネットワーク セキュリティ グループ]** 項目をクリックします。

    ![[ネットワーキング] セクションが選択されて [ネットワーク セキュリティ グループ] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace-3.png)


1. **woodgrove-NSG** という名前を付けて、Azure サンドボックス リソース グループに入れます。

1. Azure Load Balancer と同じ場所にあることを確認します。

    ![ネットワーク セキュリティ グループ作成画面のスクリーンショット](../media/3-create-nsg.png)

1. **[作成]** をクリックします。

ロード バランサー、仮想ネットワーク、NSG がデプロイされるのを待ちます。 その後、ネットワーク セキュリティを構成できます。

## <a name="configure-the-network-security-group"></a>ネットワーク セキュリティ グループを構成する

1. デプロイの通知から、または Azure portal の上部にある検索バーを使用して、新しいネットワーク セキュリティ グループを選択します。

    ![[通知] パネルのデプロイ成功を示すスクリーンショット](../media/3-deployment-success.png)

1. **[設定]** > **[受信セキュリティ規則]** セクションを選択します。 常に適用される 3 つの定義済み規則があることに注意してください。 次のような規則です。
    - **AllowVnetInbound** - 仮想ネットワーク上を流れるすべての内部トラフィックを許可します。 この規則により、ネットワークを共有する VM が相互に通信できます。
    - **AllowAzureLoadBalancerInBound** - サービスが動作しているかどうかを確認するため、ロード バランサーがネットワーク上のサービスに対して "ping" を行うのを許可します。
    - **DenyAllInbound** - 他のすべてのトラフィックを拒否します。

    **DenyAllInbound** 規則は特に重要です。これにより、優先度の高い規則を持たないすべての受信トラフィックが確実にブロックされます。 これが理由で、Web サーバーに対する HTTP (80) トラフィックを許可する新しい規則を追加する必要があります。

    > [!NOTE]
    > NSG の規則の優先順位は 0 から 65500 で、規則は順番に評価されます。 最初に一致した規則によって動作が決まります。 独自の規則は常にある程度低くしておきたいので、定義済みの規則より優先されるように 1000 あたりから始めます。

1. **[追加]** をクリックして新しい規則を追加します。

    ![[追加] ボタンが強調表示されている受信ネットワーク セキュリティ規則を示すスクリーンショット。](../media/3-inbound-security-rules.png)

1. 上部にある **[基本]** をクリックして、"基本" ビューに切り替えます。

1. 新しい規則の詳細を入力します。
    - **[サービス]** では _[HTTP]_ を選択します。
    - **[優先度]** を _1000_ に設定します。
    - 規則の名前を指定します (または、既定のままにします)。

    ![HTTP の詳細を入力した受信セキュリティ規則追加ダイアログ ボックスのスクリーンショット](../media/3-add-inbound-rule.png)

1. **[追加]** をクリックして規則を追加します。

    ![NSG 規則一覧で新しく追加した HTTP 規則を示すスクリーンショット](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a>VNet にネットワーク セキュリティ グループを適用する

次に、この NSG を仮想ネットワークに適用してみましょう。

1. 自分の仮想ネットワークを探します。上部にある検索ボックスを使用できます。 名前は **woodgrove-VNET** です。

1. **[設定]** > **[サブネット]** を選択して、定義したサブネットに移動します。

1. **backendSubnet** エントリをクリックして、プロパティを開きます。

    ![サブネットの [サブネット] 領域の backendSubnet エントリを示すスクリーンショット](../media/3-subnets.png)

1. **[ネットワーク セキュリティ グループ]** の [なし] エントリをクリックします

    ![backendSubnet の空のネットワーク セキュリティを示すスクリーンショット](../media/3-add-network-security-group.png)

1. **woodgrove-NSG** を選択してこの VNET に追加します。

1. **[保存]** をクリックして変更を適用します。

ネットワークの準備ができたので、このネットワークに配置する仮想マシンをいくつか作成しましょう。