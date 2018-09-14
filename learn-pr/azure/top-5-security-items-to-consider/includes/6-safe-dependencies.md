最新のアプリケーションに存在するコードの大部分は、ライブラリと選択した依存関係: 開発者。 これは、時間と費用を節約するための一般的な方法です。 ただし、欠点は、しているようになりましたこのコードは、責任を負う場合でも、他のユーザーが書いた、プロジェクトで使用するために。 研究者 (または、さらに悪いことで、ハッカー) 同じ問題が可能性がありますが、これらのサード パーティ ライブラリのいずれかの脆弱性を検出する場合も、アプリで発生します。

業界に大きな問題は、既知の脆弱性でコンポーネントを使用します。 それほど問題でが行われて、 [OWASP トップ 10 リスト](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)最悪 web アプリケーションの脆弱性、9 で数年間保持します。

## <a name="track-known-security-vulnerabilities"></a>既知のセキュリティの脆弱性を追跡します。

ある問題を認識するには、問題が検出されたとき。 ライブラリと依存関係を維持する方法、更新 (4 一覧に!) が役立つが、アプリケーションに影響を与えかねない、識別された脆弱性を追跡することをお勧めです。

> [!IMPORTANT]
> システムに既知の脆弱性がある場合は、ユーザーがこれらのシステムの攻撃に使用できる、使用可能な悪用コードがある場合にも可能性が高くなります。 悪用が行われたパブリック場合は、任意の影響を受けるシステムがすぐに更新される非常に重要です。

**Mitre**が管理している非営利組織、[一般的な脆弱性と脅威の一覧](https://cve.mitre.org)します。 この一覧は、アプリ、ライブラリ、およびフレームワークの既知のサイバー セキュリティの脆弱性の検索可能なパブリックに設定します。 **CVE データベースで、ライブラリまたはコンポーネントを検索することと脆弱性がわかっている**します。

問題は、製品またはコンポーネントのセキュリティ上の欠陥が見つかったときに、セキュリティ コミュニティによって送信されます。 パブリッシュされた各問題は、ID が割り当てられている検出されると、日付、脆弱性の説明が含まれ、パブリッシュされた回避策や、問題に関するベンダー ステートメントへの参照します。

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>かどうか、サード パーティ製コンポーネントの脆弱性を正常があることを確認する方法

この一覧を確認するには、携帯電話に、毎日のタスクを配置する可能性がありますが幸運にも、多くのツールが存在かどうか、依存関係は脆弱性のあることを確認できるようにするため。 コードベースに対してこれらのツールを実行したり、さらに、開発プロセスの一環として、問題を自動的に確認、CI/CD パイプラインに追加できます。

- [OWASP 依存関係の確認](https://www.owasp.org/index.php/OWASP_Dependency_Check)を持つ、 [Jenkins プラグイン](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io)GitHub でオープン ソース リポジトリの無料
- [黒アヒル](https://www.blackducksoftware.com)多くの企業が使用します。
- [RubySec](https://rubysec.com) Ruby 用だけアドバイザリのデータベース
- [Retire.js](https://github.com/retirejs/retire.js/)を検証するためのツール、JavaScript ライブラリが期限切れ場合として使用できます、プラグインなど、さまざまなツールの[Burp スイート](https://www.portswigger.net)

静的コード分析の専用に行われたいくつかのツールは、これも使用できます。

- [Roslyn のセキュリティ保護](https://dotnet-security-guard.github.io)
- [Puma スキャン](https://pumascan.com)
- [PT Application インスペクター](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven の依存関係のプラグイン](http://maven.apache.org/plugins/maven-dependency-plugin/)
- [ソースのクリア](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [ノード セキュリティ プラットフォーム](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [その他.](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

脆弱性のあるコンポーネントを使用して関連するリスクの詳細については、次を参照してください。、 [OWASP ページ](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities)このトピックを特集します。

## <a name="summary"></a>まとめ

すべてのリスクで行ったも、アプリケーションの一部としてライブラリまたは他のサード パーティ製コンポーネントを使用する場合があります。 このリスクを軽減する最善の方法では、それらに関連付けられている既知の脆弱性がないコンポーネントを使用してのみことを確認します。
