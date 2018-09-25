最新のアプリケーションに存在するコードのかなりの部分は、開発者が選択したライブラリと依存関係です。 これは、時間と費用を節約するための一般的な方法です。 ただし、欠点として、そのようなコードは、他の開発者が作成したものであっても、選択した開発者のプロジェクトで使用されるので、使用する開発者が責任を負うことになります。 研究者が (より悪いケースではハッカーが) このようなサード パーティ製ライブラリのいずれかで脆弱性を発見した場合、それを使用しているアプリにも同じ問題が存在する可能性があります。

既知の脆弱性を含むコンポーネントを使用することは、この業界において大きな問題です。 これは非常に大きな問題なので、Web アプリケーションの最悪の脆弱性に関する [OWASP のトップ 10 リスト](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)で、数年にわたって 9 位を続けています。

## <a name="track-known-security-vulnerabilities"></a>セキュリティの既知の脆弱性を追跡する

問題となるのは、脆弱性が発見されたときにそれを知ることです。 ライブラリと依存関係を更新しておくこと (リストの 4 番目) ももちろん役に立ちますが、アプリケーションに影響を与える可能性がある識別された脆弱性を追跡することをお勧めします。

> [!IMPORTANT]
> システムに既知の脆弱性がある場合は、悪用できるコードを利用してシステムが攻撃される可能性が高くなります。 弱点が公になった場合、影響を受けるシステムが即座に更新されることが非常に重要です。

**Mitre** は、[一般的な脆弱性と脅威の一覧](https://cve.mitre.org)を管理している非営利組織です。 この一覧は、アプリ、ライブラリ、フレームワークにおけるサイバーセキュリティに関する既知の脆弱性のパブリックに検索可能なセットです。 **CVE データベースでライブラリまたはコンポーネントが見つかった場合、それには既知の脆弱性があります**。

製品またはコンポーネントにおいてセキュリティ上の欠陥が見つかると、セキュリティ コミュニティによって問題が送信されます。 公開された各問題には、ID が割り当てられており、発見された日付、脆弱性の説明、問題に関する公開された回避策やベンダーのステートメントへの参照が含まれます。

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>使用しているサード パーティ製コンポーネントに既知の脆弱性があるかどうかを確認する方法

毎日のタスクを携帯電話に入力してこの一覧を確認することもできますが、依存関係が脆弱かどうかを確認できる多くのツールが存在します。 コードベースに対してこれらのツールを実行できます。さらにいいのは、CI/CD パイプラインにツールを追加し、開発プロセスの一環として問題を自動的に確認することです。

- [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check): [Jenkins プラグイン](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)があります
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io): GitHub のオープン ソース リポジトリの場合は無料です
- [Black Duck](https://www.blackducksoftware.com): 多くの企業によって使用されています
- [RubySec](https://rubysec.com): Ruby 用のみのアドバイザリ データベースです
- [Retire.js](https://github.com/retirejs/retire.js/): JavaScript ライブラリが古くなっているかどうかを確認するためのツールであり、[Burp Suite](https://www.portswigger.net) などのさまざまなツールのプラグインとして使用できます

静的コード分析専用に作られている一部のツールもこれに使用できます。

- [Roslyn Security Guard](https://dotnet-security-guard.github.io)
- [Puma Scan](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](https://maven.apache.org/plugins/maven-dependency-plugin/)
- [Source Clear](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Node Security Platform](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [その他...](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

脆弱性のあるコンポーネントを使用したときのリスクの詳細については、このトピックについて特に説明されている [OWASP のページ](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities)をご覧ください。

## <a name="summary"></a>まとめ

アプリケーションの一部としてライブラリまたは他のサード パーティ製コンポーネントを使用するときは、それらに存在する可能性のあるすべてのリスクをアプリケーションも被ることになります。 このリスクを軽減する最善の方法は、既知の脆弱性がないコンポーネントのみを使用することです。
