Lamna Healthcare では、医師が患者の医療データを管理するための従来の内部アプリケーションと Web ポータルをホストしています。 組織では、多くの場合、オンサイトで患者を介護するため、ネットワーク外にいる介護士がこのアプリケーションを利用できるようにしてほしいという多くの要望を受けています。 

悪意のあるエージェントによる最近のデータ リークにより、企業はそのパスワード ポリシーを強化することを余儀なくされています。 ユーザーは現在、自分のパスワードをより頻繁に変更し、また、より長い複雑なパスワードを使用する必要があります。 ユーザーはさまざまな管理者ロール用に作成された複数の資格情報セットを覚えておくのに苦労するため、複雑なパスワードを安全でない方法で記録するという望ましくない副作用につながっています。 

ここでは、内外のアプリケーション用のセキュリティ層としての ID について説明します。また、ID をセキュリティで保護するためのシングル サインオン (SSO) と多要素認証 (MFA) の利点、Azure Active Directory にオンプレミス ID をレプリケートすることを検討する理由を説明します。

## <a name="identity-as-a-layer-of-security"></a>セキュリティ層としての ID

デジタル ID は、現在のオンプレミスとオンラインでのビジネスおよびソーシャル インタラクションの不可欠な部分です。 これまでは、ID およびアクセス サービスは、専用の Kerberos や LDAP などのプロトコルを使用する、会社の内部ネットワーク内での運用に限定されていました。 ここ最近では、モバイル デバイスが、ユーザーがデジタル サービスとやりとりする主な手段となっています。 顧客と従業員は同様にいつでもどこからでもサービスにアクセスできることを期待しています。このことが、多くのさまざまなデバイスとオペレーティング システムの間でインターネット規模で動作できる ID プロトコルの開発を促しています。

アーキテクチャの ID に関する機能が評価されるため、Lamna Healthcare ではアプリケーションに次の機能を導入できる方法を模索しています。

- アプリケーション ユーザーにシングル サインオンを提供する
- レガシ アプリケーションを強化して最小限の労力で先進認証を使用できるようにする
- 会社のネットワーク外でのすべてのログインに多要素認証を適用する
- 患者が自分のアカウント データを登録し、安全に管理できるようにアプリケーションを開発する

## <a name="single-sign-on"></a>シングル サインオン

ユーザーが管理する必要がある ID が多くなればなるほど、資格情報に関するセキュリティ インシデントのリスクが高まります。 ID が多くなると、覚えておく必要があり、また変更する必要があるパスワードが増えることになります。 パスワード ポリシーはアプリケーションによって異なる場合があり、複雑さの要件が増えるほど、ユーザーはパスワードを覚えておくことがより困難になります。

その一方で、それらすべての ID を管理する必要があります。 ヘルプ デスクにはさらに負担がかかります。アカウントのロックアウトやパスワードのリセット要求に対応する必要があるためです。 ユーザーが退職した場合、その ID をすべて追跡し、無効になっていることを確認することが困難である可能性があります。 ID を見落とした場合、除外しておくべきアクセスが許可される可能性があります。

シングル サインオンの場合、ユーザーは 1 つの ID と 1 つのパスワードを覚えておくだけで済みます。 アプリケーション間でのアクセスはユーザーに関連付けられている単一の ID に許可され、セキュリティ モデルが簡素化されます。 ユーザーがロールを変更したか、退職したときに、アクセス変更が単一の ID に関連付けられ、アカウントを変更したり、無効にするために必要な労力が大幅に減少します。 アカウントにシングル サインオンを利用することで、ユーザーは自分の ID を管理しやすくなり、ご利用の環境内でのセキュリティ機能が強化されます。

### <a name="sso-with-azure-active-directory"></a>Azure Active Directory での SSO

Azure Active Directory (AD) はクラウド ベースの ID サービスです。 これは、既存のオンプレミス Active Directory との同期をサポートしています。スタンドアロンで使用することもできます。 つまり、アプリケーションがオンプレミスであるか、クラウド (Office 365 を含む) にあるか、さらにはモバイルであるかに関係なく、すべてのアプリケーションで同じ資格情報を共有することができます。 管理者と開発者は、Azure AD で構成され、一元管理されたルールとポリシーを使用して、データやアプリケーションへのアクセスを制御できます。

SSO に対して Azure AD を活用することで、複数のデータ ソースを 1 つのインテリジェントなセキュリティ グラフに統合することもできます。 このセキュリティ グラフによって、Azure AD 内のすべてのアカウントに対する脅威の分析とリアルタイムの ID 保護が可能になります。対象のアカウントには、オンプレミスの AD から同期されているものも含まれます。 一元化された ID プロバイダーを使用することで、ID インフラストラクチャのセキュリティ コントロール、レポート、アラート、および管理が一元化されます。

### <a name="synchronize-directories-with-ad-connect"></a>ディレクトリと AD Connect を同期させる

Azure AD Connect では、オンプレミスのディレクトリと Azure Active Directory を統合します。 Azure AD Connect では最新機能が提供されます。Azure AD Connect は、DirSync や Azure AD Sync などの古いバージョンの ID 統合ツールに代わるものです。

同期とサインインのための容易なデプロイを実現できる単一のツールです。

![Azure AD Connect を使用して、オンプレミス ディレクトリと Active Directory と Azure クラウド内のアプリを同期していることを示す図。 これにより、ユーザーは Azure クラウド内のすべてのアプリケーションに対して共通のサインオンを行うことができます。](../media/AADCONNECTxprs_960.png)

Lamna Healthcare では、主にオンプレミスの DC に対する認証が必要ですが、ディザスター リカバリー シナリオでのクラウド認証も必要です。 Azure AD でまだサポートされていない要件はありません。

Lamna Healthcare は、次の構成で進めることにしました。

- Azure AD Connect を使用して、グループ、オンプレミスの Active Directory に格納されているユーザー アカウントおよびパスワード ハッシュを Azure AD に同期します
  - パススルー認証を利用できない場合、これをバックアップとして使用できます
- オンプレミスの Windows Server 上にインストールされているオンプレミスの認証エージェントを使用するパススルー認証を構成します
- Azure AD のシームレス シングル サインオン機能を使用して、オンプレミスのドメインに参加している PC から自動的にユーザーにサインインします
  - 複数の認証要求を抑制することでユーザーの摩擦が軽減します

## <a name="authentication--access"></a>認証とアクセス

Lamna Healthcare のセキュリティ ポリシーでは、会社の境界ネットワーク外で行われるすべてのログインを追加の認証要素で認証することが求められます。 この要件により、Azure AD サービスの 2 つの側面である、多要素認証と条件付きアクセス ポリシーが組み合わされます。

### <a name="multi-factor-authentication"></a>多要素認証

多要素認証 (MFA) では、完全な認証のために 2 つ以上の要素を必須とすることで、ID のセキュリティが強化されます。 これらの要素は、次の 3 つのカテゴリに分類されます。

- *ユーザーが知っているもの*
- *ユーザーが持っているもの*
- *ユーザー自身*

**ユーザーが知っているもの**は、パスワード、あるいはセキュリティの質問に対する答えです。 **ユーザーが持っているもの**は、通知を受け取るモバイル アプリまたはトークンを生成するデバイスです。 **ユーザー自身**は通常、多くのモバイル デバイスで使用される指紋や顔スキャンなどの、ある種の生体認証です。

多要素認証を使用して、資格情報の露出の影響を限定することで、ID のセキュリティが強化されます。 ユーザーのパスワードを持つ攻撃者も、完全に認証するためには自分の電話や顔が必要になります。 確認された単一要素のみの認証では十分ではなく、攻撃者は資格情報を使用して認証できなくなります。 セキュリティにもたらされるこの利点は大きく、可能な限り、MFA を有効にするために十分に繰り返すことはできません。

Azure AD には MFA 機能が組み込まれており、他のサード パーティの MFA プロバイダーと統合されます。 これらは機密性の高いアカウントであるため、Azure AD でグローバル管理者のロールを持つすべてのユーザーに無料で提供されます。 その他のすべてのアカウントでは MFA を有効にすることができます。その場合、この機能を持つライセンスを購入し、ライセンスをアカウントに割り当てます。

### <a name="conditional-access-policies"></a>条件付きアクセス ポリシー

MFA と共に、アクセスを許可する前に追加の要件が満たされていることを確認することで、別の保護層を追加することができます。 不審な IP アドレスからのログインをブロックしたり、マルウェア保護なしのデバイスからのアクセスを拒否したりすることで、危険なサインインからのアクセスを制限できます。

Azure Active Directory では、グループ、場所、またはデバイスの状態に基づくアクセス ポリシーに関するサポートを含む、条件付きアクセス ポリシー (CAP) 機能が提供されます。 位置情報機能を利用することで、Lamna では、Lamna のネットワークに属していない IP アドレスを区別することができ、そのようなすべての場所からの多要素認証を求めるセキュリティ ポリシーを満たすことができます。

Lamna Healthcare では、ユーザーが会社のネットワーク外の IP アドレスからアクセスする際に MFA を求める[条件付きアクセス ポリシー](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)が作成されています。

![条件付きアクセス ポリシーと多要素認証の実装例を示す図。 オンプレミスおよびクラウド アプリケーションへのユーザーのアクセス要求は、最初に条件の一覧に対してチェックされます。 要求は、アクセスを許可されるか、多要素認証を実行するように強制されるか、またはそれらが満たす条件に基づいてブロックされます。](../media/conditional-access.png)

## <a name="securing-legacy-applications"></a>レガシ アプリケーションのセキュリティ保護

Lamna Healthcar の従業員は、オンプレミスでホストされている管理アプリケーションに対するセキュリティで保護されたリモート アクセスが必要です。 ユーザーは現在、企業のファイアウォールの背後にある、ドメインに参加しているコンピューターから Windows 統合認証 (WIA) を使用してアプリケーションに対して認証を行っています。 アプリケーションに先進認証メカニズムを組み込むプロジェクトが予定されていますが、できるだけ早くリモート アクセス機能を有効にするという大きなビジネス プレッシャーがあります。 Azure アプリケーション プロキシでは、コード変更なしで、すばやく、簡単かつ安全にアプリケーションにリモート アクセスできます。

Azure AD アプリケーション プロキシの特徴:

- シンプル
  - アプリケーション プロキシを使用するためにアプリケーションを変更または更新する必要はありません。
  - ユーザーに一貫性のある認証エクスペリエンスが提供されます。 MyApps ポータルを使用して、クラウド内の SaaS アプリとオンプレミスのアプリの両方でシングル サインオンを取得できます。
- セキュリティ保護
  - Azure AD アプリケーション プロキシを使用してアプリを発行するときは、Azure の豊富な承認制御とセキュリティ分析を活用できます。 クラウド スケールのセキュリティと、条件付きアクセスや 2 段階認証などの Azure セキュリティ機能が提供されます。
  - ユーザーにリモート アクセスを許可するためにファイアウォールを介した着信接続を開く必要はありません。
- 高いコスト効率
  - アプリケーション プロキシはクラウドで動作するため、時間とコストを節約できます。 オンプレミスのソリューションでは、通常、DMZ、エッジ サーバー、またはその他の複雑なインフラストラクチャを設定して維持する必要があります。

Azure AD アプリケーション プロキシは、企業ネットワーク内の Windows Server 上にあるコネクタ エージェントと、外部エンドポイント (MyApps ポータルまたは外部 URL) の 2 つのコンポーネントで構成されています。 ユーザーはエンドポイントに移動したときに、Azure AD で認証され、コネクタ エージェント経由でオンプレミス アプリケーションにルーティングされます。

## <a name="working-with-consumer-identities"></a>コンシューマー ID の操作

先進認証と既存のアプリケーションとの統合により、Lamna Healthcare では、Azure AD などのマネージド ID システムが組織にもたらす力をすぐに認識できました。 リーダーシップ チームは現在、Microsoft の ID サービスによってビジネス価値を高められる他の方法を探求することに関心があります。 現在は、外部の顧客と既存の顧客との対話を最新化して、Google、Facebook、LinkedIn などのサード パーティの ID プロバイダーと緊密に統合する方法に着目しています。

Azure AD B2C は、アプリケーション使用時の顧客のサインアップ、サインイン、プロファイル管理の方法をカスタマイズして制御できる Azure Active Directory の強固な基盤に築かれた ID 管理サービスです。 これには特に iOS、Android、.NET 用に開発されたアプリケーションが含まれます。 Azure AD B2C ではソーシャル ID ログイン エクスペリエンスが提供され、同時に顧客の ID プロファイル情報が保護されます。 Azure AD B2C ディレクトリは標準の Azure AD ディレクトリとは区別されており、Azure Portal で作成することができます。

## <a name="identity-management-at-lamna-healthcare"></a>Lamna Healthcare での ID 管理

ここでは、Lamna Healthcare で Azure の ID 管理ソリューションを使用して、環境のセキュリティを強化する方法について説明しました。 ユーザーが処理する必要があるアカウントを最小限に抑え、過度なアカウントによる運用の複雑さを減らすため、ユーザーにシングル サインオン エクスペリエンスを提供することから始めました。 アプリケーションへのアクセスに MFA を適用し、最小限の労力で先進認証を使用できるようにレガシ アプリケーションを更新しました。 また、顧客 ID を操作しやくして、患者がアプリケーションをより使いやすくする方法を学習しました。

## <a name="summary"></a>まとめ

このユニットでは、多くの Azure Active Directory 機能を組み合わせて、場所に関係なく、アプリケーションへのアクセスをセキュリティで保護するために強固な ID ソリューションを提供する方法を確認しました。 ID はセキュリティの重要な層です。 適切に設計し、アーキテクチャに組み込むことで、安全な環境を確保できます。