お客様によって管理されているデータセンターからクラウド データセンターへのコンピューティング環境の移行時に、セキュリティの責任も移行されます。 現在、セキュリティはクラウド プロバイダーとお客様の両方に共通の懸念事項です。 すべてのアプリケーションとソリューションにおいて、お客様が負う責任と Azure 側が負う責任を理解しておくことが重要です。

#### <a name="understand-security-threats"></a>セキュリティの脅威について

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWkotg]

#### <a name="azure-security-you-versus-the-cloud"></a>Azure のセキュリティ: お客様とクラウド

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a>セキュリティの責任を Azure と共有する

最初に行う移行は、オンプレミスのデータ センターからサービスとしてのインフラストラクチャ (IaaS) への移行です。 IaaS の場合は、最下位レベルのサービスを利用します。そして Azure で仮想マシン (VM) と仮想ネットワークを作成します。 このレベルでは、オペレーティング システムおよびソフトウェアに対してパッチを適用すること、これらをセキュリティで保護すること、さらにセキュリティで保護されるようにネットワークを構成することはお客様側の責任です。 Contoso Shipping では、自社のオンプレミスの物理サーバーに代えて Azure VM の使用を開始するときから、IaaS を活用しています。 運用上の利点に加えて、ネットワークの物理的な部分の保護に対する懸念を外部委託できるというセキュリティ上の利点があります。

サービスとしてのプラットフォーム (PaaS) への移行によって、セキュリティ上の懸念事項の多くが外部に委託されます。 このレベルでは、オペレーティング システムおよび最も基本的なソフトウェア (データベース管理システムなど) に対応します。 すべてが最新のセキュリティ パッチで更新されます。また、すべてを Azure Active Directory に統合してアクセス制御を行うことができます。 PaaS には、数多くの運用上の利点があります。 ご利用の環境に合わせてインフラストラクチャおよびサブネット全体を手動で構築するのではなく、Azure portal 内で "ポイントしてクリック" するか、または自動化されたスクリプトを実行することで、セキュリティで保護された複雑なシステムをアップまたはダウン状態にしたり、必要に応じてそれらのシステムをスケーリングしたりすることができます。 Contoso Shipping では、ドローンおよびトラック &mdash; からの製品利用統計情報データを取り込むために Azure Event Hubs を使用します。また、モバイル アプリ &mdash; で Azure Cosmos DB バック エンドを使用した Web アプリも使用します。これらはすべて PaaS の例です。

サービスとしてのソフトウェア (SaaS) の場合は、ほぼすべてを外部委託できます。 SaaS は、インターネット インフラストラクチャで実行されるソフトウェアです。 そのコードはベンダーによって管理されますが、お客様が使用できるように構成されます。 多くの企業と同様に Contoso Shipping でも、SaaS の良い例である Office 365 を使用しています。

![shared_responsibility.png](../media/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>セキュリティへの階層型アプローチ

"*多層防御*" は、情報への不正なアクセスを目的とする攻撃の進行を遅らせる一連のメカニズムを採用する戦略です。 各層で保護が提供されるため、1 つの層が破られた場合、後続の層は既に備えができており、さらなる露出を防ぐことができます。 Microsoft では、自社の物理データセンター内と Azure サービス間の両方でのセキュリティに階層型アプローチを適用しています。 多層防御の目的は、情報を保護し、情報へのアクセスが許可されていない個人から盗まれないようにすることです。

多層防御は、同心円状の輪のセットとして視覚化でき、データは中央でセキュリティ保護されます。 各輪では、データに関するセキュリティ層が追加されます。 このアプローチでは、単一の保護層に依存する必要がなくなり、攻撃速度を落とすことができます。また、自動または手動によるアラート テレメトリが提供されます。 それでは各層を見ていきましょう。

![多層防御](../media/defense_in_depth_layers_small.PNG)

:::row:::
  :::column:::
    ![データを表す画像](../media/2-data.png)
  :::column-end:::
    :::column span="3"::: **データ**

ほとんどすべての場合、攻撃者は次のようなデータを狙っています。

- データベースに格納されたデータ
- 仮想マシン内のディスク上に格納されたデータ
- Office 365 などの SaaS アプリケーションに格納されたデータ
- クラウド ストレージに格納されたデータ

データが確実に適切なセキュリティで保護されるように格納し、データへのアクセスを制御する必要があります。 多くの場合、データの機密性、整合性、可用性を確保するために適用する必要がある制御とプロセスを示す規制上の要件があります。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![ネットワーク上のファイルの画像](../media/2-application.png)
  :::column-end:::
    :::column span="3"::: **アプリケーション**

- アプリケーションが確実にセキュリティで保護された脆弱性のないものにします。
- 機密性の高いアプリケーション シークレットをセキュリティで保護されたストレージ メディアに格納します。
- すべてのアプリケーション開発に関する設計要件を安全なものにします。

アプリケーション開発のライフ サイクルにセキュリティを統合することは、コードにおける脆弱性の数を減らすのに役立ちます。 すべての開発チームが確実に既定でアプリケーションをセキュリティで保護するようにし、セキュリティ要件を交渉の余地のないものにします。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![コンピューティングを表すターミナル](../media/2-compute.png)
  :::column-end:::
    :::column span="3"::: **コンピューティング**

- 仮想マシンへのアクセスをセキュリティで保護します。
- エンドポイント保護を実装し、システムにパッチを適用して最新の状態に保ちます。

マルウェア、パッチが適用されていないシステム、適切にセキュリティ保護されていないシステムのために、環境が攻撃を受けやすくなってしまいます。 この層では、コンピューティング リソースをセキュリティで保護すること、セキュリティ問題を最小限に抑えるべく、適切な管理体制を敷くことに重点的に取り組みます。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![ネットワークを表す 3 つの接続されたシステム](../media/2-networking.png)
  :::column-end:::
    :::column span="3"::: **ネットワーク**

- リソース間の通信を制限します。
- 既定で拒否します。
- 必要に応じて、インバウンド インターネット アクセスを限定し、アウトバウンドを制限します。
- オンプレミス ネットワークへのセキュリティで保護された接続を実装します。

この層では、リソース全体でのネットワーク接続を制限し、必要なもののみを許可することに重点を置きます。 この通信を制限することで、ネットワーク全体の侵入拡大のリスクを軽減することができます。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![ネットワーク境界を表す物理バリア](../media/2-perimeter.png)
  :::column-end:::
    :::column span="3"::: **境界**

- 分散型サービス拒否 (DDoS) 保護を使用して、エンド ユーザーに対するサービス拒否が発生する前に大規模な攻撃をフィルター処理します。
- 境界ファイアウォールを使用して、ネットワークに対する悪意のある攻撃を識別し、警告します。

ネットワーク境界で、リソースに対するネットワークベースの攻撃から保護する目的があります。 これらの攻撃を識別し、影響を排除し、これらの発生時に警告することは、ネットワークを安全に保つために重要なこと方法です。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![セキュリティで保護されたアクセスを表すバッジ](../media/2-policies-and-access.png)
  :::column-end:::
    :::column span="3"::: **ポリシーとアクセス**

- インフラストラクチャへのアクセスを制御し、変更を制御します。
- シングル サインオンと多要素認証を使用します。
- イベントと変更を監査します。

ポリシーとアクセス層では、ID がセキュリティで保護されていることと、付与されたアクセス権のみが必要であることと、変更がログに記録されていることを確認することに集中します。
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![物理的なセキュリティを表すセキュリティ カメラ](../media/2-physical-security.png)
  :::column-end:::
    :::column span="3"::: **物理的なセキュリティ**

- 物理的なセキュリティを構築し、データ センター内のコンピューティング ハードウェアへのアクセスを制御することが、防御の最前線となります。

物理的なセキュリティの目的は、資産へのアクセスに対する物理的な保護措置を講じることです。 これにより、他の層をバイパスできなくなり、損失や盗難の適切な処理が確保されます。
  :::column-end:::
:::row-end:::

## <a name="summary"></a>まとめ

ここでは、セキュリティに関するお客様の懸念事項の解消に Azure が大いに役立っていることを見てきました。 しかし、セキュリティは引き続き、**共有する責任**となります。 背負う必要がある責任の程度は、Azure で使用するモデルの種類によって異なります。

データや環境に適切な保護を検討するためのガイドラインとして、*多層防御*リングを使用します。