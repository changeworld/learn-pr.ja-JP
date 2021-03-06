開発者の多くが、ソフトウェアの構築に使用するフレームワークとライブラリは、主に機能または個人的な好みによって決まると考えています。 しかし、フレームワークの選択は、設計や機能性の視点だけでなく、"_セキュリティ_" の視点からも重要な決定となります。 最新のセキュリティ機能を備えたフレームワークを選択し、そのフレームワークを常に最新の状態にしておくことは、アプリのセキュリティを確保するための最善の方法の 1 つです。

## <a name="choose-your-framework-carefully"></a>フレームワークを慎重に選択する

フレームワークを選択するときにセキュリティに関して最も重要な要素は、そのフレームワークが十分にサポートされているかどうかです。 優れたフレームワークではセキュリティに関する取り決めが明示されており、フレームワークの改善やテストに取り組む大規模なコミュニティによってサポートされています。 バグがまったくない、あるいは 100% 安全なソフトウェアはありませんが、脆弱性が特定されたときに、確実に穴が塞がれるか、迅速に回避策が提供されることを望むものです。

"十分にサポートされている" という言葉は、多くの場合、"最新" と同じ意味になります。 古いフレームワークは他に取って代わられる、あるいは最終的には人気が衰えていく傾向にあります。 古いフレームワークでの経験が豊富にある、あるいはそのフレームワークで多くのアプリを作成していたとしても、必要な機能を備えた最新のライブラリを選択することをお勧めします。 多くの最新フレームワークは、新しいアプリを作成する際に最新フレームワークを選択することが、脅威にさらされる面を減らすための 1 つの形式となっているという過去の繰り返しから得られた教訓に基づいて構築されています。 レガシー アプリケーションを作成した古いフレームワークで脆弱性が検出された場合、心配する必要があるアプリが 1 つ減ることになります。

セキュリティで保護された設計と脅威にさらされる面の削減の詳細については、「[Azure でのセキュリティのための設計](../../design-for-security-in-azure/index.yml)」を参照してください。

## <a name="keep-your-framework-updated"></a>お使いのフレームワークを最新の状態に保つ

Java Spring、.NET Core などのソフトウェア開発フレームワークでは、更新プログラムおよび新しいバージョンが定期的にリリースされます。 これらの更新プログラムには、新機能が追加されたり古い機能が削除されたりするほか、多くの場合、セキュリティの修正プログラムや機能強化が含まれています。 フレームワークを古いまま放置すると、"技術的負債" が発生します。 古くなればなるほど、コードを最新バージョンに更新するのが大変になり、リスクが高くなります。 また、初期フレームワークを選択した場合と同様に、古いバージョンのフレームワークを使い続けると、新しいリリースのフレームワークでは対処済みのセキュリティ脅威にさらされることが増えます。

たとえば、2016 から 2017 年の間に、[30 を超えるの脆弱性](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45)が Apache Struts フレームワークで見つかりました。 これらは開発チームによってすぐに対処されましたが、一部の企業では修正プログラムを適用しなかったため、[データ侵害という形で代償を支払うことになりました](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/)。 **お使いのフレームワークとライブラリは必ず最新の状態に保ってください**。

### <a name="how-do-i-update-my-framework"></a>使用しているフレームワークの更新方法

Java、.NET など、一部のフレームワークにはインストールが必要であり、既知の周期でリリースされる傾向があります。 新しいリリースがないか注意して、リリースされている場合は、お使いのコードの分岐を作成して試してみることをお勧めします。 たとえば、.NET Core には[リリース ノート ページ](https://github.com/dotnet/core/tree/master/release-notes)があり、このページでは使用可能な最新バージョンがないか確認できます。

JavaScript フレームワークなどのより特殊なライブラリや、.NET コンポーネントは、パッケージ マネージャーを使用して更新できます。 **NPM** および **Bower** は Web プロジェクトでは人気のある選択肢で、ほとんどの IDE またはビルド ツールでサポートされています。 .NET では、**NuGet** を使用して、コンポーネントの依存関係を管理します。 コア フレームワークの更新、コードの分岐、コンポーネントの更新、およびテストは、新しいバージョンの依存関係を検証する手法として優れています。

> [!NOTE]
> `dotnet` コマンドライン ツールには、NuGet パッケージを追加または削除するための `add package` および `remove package` オプションが含まれていますが、対応する `update package` コマンドはありません。 ただし、プロジェクトで `dotnet add package <package-name>` を実行できるため、パッケージが自動的に最新バージョンに "_アップグレード_" されます。 これにより、IDE を開かずに簡単に依存関係を更新できます。

## <a name="take-advantage-of-built-in-security"></a>組み込みのセキュリティを活用する

お使いのフレームワークが提供しているセキュリティ機能を常に確認するようにしてください。 標準的な手法や機能が組み込まれている場合は、独自のセキュリティは**決して展開しないでください**。 また、実績のあるアルゴリズムとワークフローを使用してください。こうしたアルゴリズムやワークフローは、ほとんどの場合、多くの専門家によって精査されており、さらに、批評および強化されているため、その信頼性と安全性が保証されているからです。

.NET Core フレームワークは数え切れないほど多くのセキュリティ機能を備えています。このドキュメントでは、まず基本的な機能をいくつか紹介します。
* [認証 - ID 管理](https://docs.microsoft.com/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [承認](https://docs.microsoft.com/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [データ保護](https://docs.microsoft.com/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [安全な構成](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [セキュリティ拡張 API](https://docs.microsoft.com/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

これらの機能はそれぞれその分野の専門家によって記述され、意図したとおりに確実に動作するように、また意図したとおりにしか動作しないように徹底的にテストされています。 同様の機能を提供するフレームワークは他にもあります。フレームワークを提供しているベンダーに問いあわせて、カテゴリごとに機能を確認してください。

> [!WARNING]
> フレームワークによって提供されている機能を使用せずに、ご自身でセキュリティ コントロールを記述することは、時間の無駄であるだけでなく、セキュリティの低下にもつながります。


## <a name="azure-security-center"></a>Azure Security Center

Azure を使用して Web アプリケーションをホストしているとき、お使いのフレームワークが古くなっている場合、Security Center によって推奨タブに警告が表示されます。お使いのアプリに関する警告がタブに表示されていないかどうかを、折を見て確認するのを忘れないでください。

![フレームワークのアップグレードを推奨している Azure Security Center。](../media/5-ASCFramework.png)


## <a name="summary"></a>まとめ

可能な限りご自身のアプリを構築する際は最新のフレームワークを選択し、必ず組み込みのセキュリティ機能を使用して、フレームワークを確実に最新の状態を保つようにしてください。 これらの簡単なルールによって、確実に強固な基盤の上にアプリケーションを開始できるようになります。
