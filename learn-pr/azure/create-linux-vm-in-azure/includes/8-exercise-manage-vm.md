ネットワーク セキュリティ グループをネットワークに適用し、サーバーに対して HTTP トラフィックだけを許可してみましょう。

## <a name="create-a-network-security-group"></a>ネットワーク セキュリティ グループの作成

SSH アクセスが必要であることを示したため、Azure ではセキュリティ グループが作成されているはずです。 しかし、ここではプロセス全体を確認するために、新しいセキュリティ グループを作成します。 VM の _前_ に仮想ネットワークを作成することにした場合、これは特に重要です。 前述のとおり、セキュリティ グループは "_省略可能_" であり、必ずしもネットワークと共に作成されるわけではありません。

1. [Azure portal](https://portal.azure.com?azure-portal=true) で、左隅のサイドバーにある **[リソースの作成]** ボタンをクリックして、新しいリソースの作成を開始します。

1. フィルター ボックスに「ネットワーク セキュリティ グループ」と入力し、一覧で一致する項目を選択します。

1. **[リソース マネージャー]** デプロイメント モデルが選択されていることを確認し、**[作成]** をクリックします。

1. セキュリティ グループの **[名前]** を指定します。 ここでも、名前付け規則を使用することをお勧めします。"Test Web Network Security Group #1 in East US" の場合は "test-web-eus-nsg1" を使用します。 セキュリティ グループをどこに配置するかに応じて、場所の部分を変更してください。

1. 適切な **[サブスクリプション]** を選択し、既存の **[リソース グループ]** を使用します。

1. 最後に、VM / 仮想ネットワークと同じ**場所**に配置します。 これは重要なことです。場所が異なる場合、このリソースを適用できなくなります。

1. **[作成]** をクリックしてグループを作成します。

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a>ネットワーク セキュリティ グループに新しい受信規則を追加する

デプロイはすぐに完了するはずです。完了したら、新しい規則をセキュリティ グループに追加できます。

1. Azure portal で新しいセキュリティ グループのリソースを見つけて選択します。

1. 概要ページには、ネットワークをロックダウンするために作成された既定の規則がいくつか表示されます。

    受信側:

    - VNet 間の受信トラフィックはすべて許可されます。 これにより、VNet 上のリソースは相互に通信することができます
    - Azure Load Balancer の "プローブ" によって、VM が有効であることを確認するように求められます
    - 送信側の他の受信トラフィックはすべて拒否されます。
    - VNet 上のすべてのネットワーク内のトラフィックが許可されます
    - インターネットへの送信トラフィックはすべて許可されます
    - 他の送信トラフィックはすべて拒否されます

> [!NOTE]
> これらの既定の規則には優先度の高い値が設定されます。これは、"_最後_" に評価されることを意味します。 これらを変更したり、削除したりすることはできませんが、優先度の低い値のトラフィックと一致するようにより具体的な規則を作成することで、_上書きする_ことが できます。

1. セキュリティ グループの **[設定]** パネルで **[受信セキュリティ規則]** セクションをクリックします。

1. **[+ 追加]** をクリックして新しいセキュリティ規則を追加します。

    ![セキュリティ規則を追加する](../media-drafts/8-add-rule.png)

    セキュリティ規則に必要な情報を入力する方法は 2 つ (基本と高度) あります。 [追加] パネルの上部にあるボタンをクリックして、これらを切り替えることができます。

    ![基本と高度な規則の入力](../media-drafts/8-advanced-create-rule.png)

    高度なモードでは規則を完全にカスタマイズできますが、既知のプロトコルを構成するだけの場合は、基本モードのほうが操作は少し簡単です。

1. 基本モードに切り替えます。

1. HTTP 規則に関する情報を追加します。

    - **[サービス]** を HTTP に設定します。 これでポートの範囲が設定されます。
    - **[優先度]** を "1000" に設定します。 既定値の **[拒否]** 規則よりも低い数値にする必要があります。 任意の値で範囲を開始することができますが、後で例外を作成する必要がある場合は、余裕を持たせることをお勧めします。
    - 規則に名前を付けます。ここでは、"allow-http-traffic" を使用します。
    - 規則の説明を入力します。

1. **高度な**モードに戻ります。 設定がまだ存在することに注目してください。 このパネルを使用して、より詳細な設定を作成することができます。 たとえば、**[ソース]** を特定の IP アドレスまたはカメラに固有の IP アドレスの範囲に制限できます。 ローカル コンピューターの現在の IP アドレスがわかっている場合は、それを試すことができます。 それ以外の場合は、設定を "任意" のままにして、規則をテストできるようにします。

1. **[追加]** をクリックして、規則を作成します。 これで受信規則の一覧が更新されます。規則が優先順になっていることに注目してください。これは規則を確認する順序です。
    
## <a name="apply-the-security-group"></a>セキュリティ グループを適用する

セキュリティ グループを単一の VM を保護するためにネットワーク インターフェイスに適用するか、サブネットに適用し、そのサブネット上のリソースに適用できることを思い出してください。 後者のアプローチが最も一般的であるため、これを使用します。 Azure でこのリソースにアクセスすることができます。その場合、仮想ネットワーク リソースを経由するか、仮想ネットワークを使用している VM 経由で間接的にアクセスできます。

1. 仮想マシンの **[概要]** パネルに切り替えます。 **[すべてのリソース]** の下に VM があります。

1. **[設定]** セクションで **[ネットワーク]** という項目を選択します。

    ![VM 設定のネットワーク項目](../media-drafts/8-network-settings.png)

1. ネットワーク プロパティには、**[仮想ネットワーク/サブネット]** を含む、VM に適用されるネットワークに関する情報があります。 これは、リソースにアクセスするためのクリック可能なリンクです。 これをクリックして仮想ネットワークを開きます。 このリンクは、VM の **[概要]** パネル "_でも_" 使用できます。 これらのいずれかをクリックすると、仮想ネットワークの **[概要]** が開きます。

1. **[設定]** セクションで、**[サブネット]** 項目を選択します。

1. 以前に VM とネットワークを作成したときの定義された (既定) 単一のサブネットがあるはずです。 一覧の項目をクリックして詳細を表示します。

1. **[ネットワーク セキュリティ グループ]** エントリをクリックします。

1. **test-web-eus-nsg1** という新しいセキュリティ グループを選択します。 ここには VM で作成された別のグループもあるはずです。

1. **[保存]** をクリックして変更内容を保存します。 ネットワークに適用されるまで少し時間がかかります。

## <a name="verify-the-rules"></a>規則を確認する

変更内容を検証しましょう。

1. 仮想マシンの **[概要]** パネルに戻ります。 **[すべてのリソース]** の下に VM があります。

1. **[設定]** セクションで **[ネットワーク]** という項目を選択します。

1. ネットワークの **[概要]** パネルには、**[有効なセキュリティ規則]** のリンクがあります。これをクリックすると、規則がどのように評価されるかがすぐにわかります。 リンクをクリックして分析を開き、新しい規則が表示されることを確認します。

    ![ネットワークの有効なセキュリティ規則](../media-drafts/8-effective-rules.png)

1. もちろん、規則がすべて動作しているのを検証する最善の方法は、サーバーに HTTP 要求を送信することです。 まだ動作しているはずです。 **HTTP** 規則を削除して、セキュリティ グループ規則が適用されていることを確認することもできます。

## <a name="one-more-thing"></a>もう 1 つ注意すべき点

セキュリティ規則を理解するのは大変です。 この新しいセキュリティ グループを適用したときに間違えてしまいました。つまり、SSH が失われてしまったのです。 これを解決するために、SSH アクセスがサポートされるようにセキュリティ グループに別の規則を追加することができます。 必ず、規則に対する受信 TCP/IP アドレスを所有しているものに限定してください。

> [!WARNING]
> 常に、管理アクセスに使用するポートをロックダウンするようにしてください。 さらに優れたアプローチは、プライベート ネットワークに仮想ネットワークをリンクするための VPN を作成し、そのアドレス範囲からの RDP または SSH 要求のみを許可することです。 SSH で使用されるポートを、既定以外のものに変更することもできます。 ポートの変更は攻撃を阻止するには不十分であり、検出が少し困難になることに注意してください。