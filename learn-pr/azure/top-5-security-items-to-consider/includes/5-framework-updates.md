多くの開発者は、フレームワークとライブラリ、機能または個人的な好みで主に決定するで、ソフトウェアの構築に使用を検討してください。 ただし、選択したフレームワークからも、デザインと機能の観点からだけでなく、重要な決定事項、_セキュリティ_パースペクティブ。 最新のセキュリティ機能とフレームワークを選択して、最新の状態を維持するは、アプリがセキュリティで保護されたことを確認する最善の方法の 1 つです。

## <a name="choose-your-framework-carefully"></a>フレームワークを慎重に選択します。

フレームワークの選択がどの程度サポートされている場合のセキュリティに関する最も重要な要因が。 最適なフレームワークでは、セキュリティの並べ替え方法が記載されているし、改善およびフレームワークのテストを行う大規模なコミュニティでサポートされています。 ソフトウェアには、バグをなくし、または完全にセキュリティで保護された 100% はありませんは閉じられますが、または迅速に提供されている回避策があることを確認する脆弱性が識別されます。

多くの場合、「もサポートされている」は同じ意味で「最新」。 古いフレームワークでは、置き換えられるか、最終的に人気をフェードインする傾向があります。 場合でも、重要な経験があるか、または古いフレームワークで記述された多くのアプリ、きっと必要がある機能を備えた最新のライブラリを選択することをお勧めします。 最新のフレームワークでは、新しいアプリの脅威の回避のフォームを選択したり、以前のイテレーションによって得られた教訓で構築する傾向があります。 少ない 1 つのアプリで、レガシ アプリケーションが記述されている古いフレームワークで脆弱性が検出された場合について心配する必要があります。

<!-- TODO: add link; Should we be pointing to other modules? -->
<!--
For more information on secure design and reducing threat surface, please see [Design For Security in Azure](../../design-for-security-in-azure/index.yml).
-->

## <a name="keep-your-framework-updated"></a>更新されたフレームワークを保持します。

Java Spring と .NET Core などのソフトウェア開発フレームワークは、更新プログラムと新しいバージョンを定期的にリリースします。 これらの更新プログラムには、古い機能は、多くの場合のセキュリティ修正プログラムや機能強化の削除の新機能が含まれます。 このフレームワークに期限切れになる、許可しています「技術的な負債」を作成します。 取得、さらに期限切れ、最新バージョンまで、コードを表示することが困難とリスクです。 さらに、初期フレームワークの選択を使い続ける以前のバージョンの framework オープンするまで、framework のそれ以降のリリースで修正された複数のセキュリティの脅威をほぼ同じようにします。

たとえば、2016-2017 年から[30 を超えるの脆弱性](https://www.cvedetails.com/product/6117/Apache-Struts.html?vendor_id=45)Apache Struts framework で見つかりました。 これらがすぐに、開発チームによってアドレス指定されたが、一部の企業が、修正プログラムを適用していないと[データ侵害の形式で価格を有料](https://www.zdnet.com/article/equifax-confirms-apache-struts-flaw-it-failed-to-patch-was-to-blame-for-data-breach/)します。 **フレームワークおよびライブラリを最新に保つために必ず**します。

### <a name="how-do-i-update-my-framework"></a>このフレームワークの更新方法

.NET、Java など、一部のフレームワークでは、インストールを必要とし、既知のリズムをリリースする傾向があります。 新しいリリースの視聴およびを試してみると、解放されるときに、コードの分岐を作成することをお勧めします。 例として、.NET Core の維持、[リリース ノート ページ](https://github.com/dotnet/core/tree/master/release-notes)使用可能な最新バージョンを検索するチェックインできます。

さらに特殊化などの JavaScript フレームワーク、ライブラリまたはパッケージ マネージャーでの .NET コンポーネントを更新できます。 **NPM**と**Bower** web プロジェクトは、人気のある選択肢やほとんどの Ide でサポートされているツールを構築します。 使用して .NET では、 **NuGet**コンポーネント依存関係を管理します。 はるかと同様に、core framework を更新するには、コードを分岐する、コンポーネントの更新、およびテストは、依存関係の新しいバージョンを検証する優れた手法です。

> [!NOTE]
> `dotnet`コマンド ライン ツールには、`add package`と`remove package`オプションを追加または削除の NuGet パッケージしますが、ない、対応する`update package`コマンド。 実は、行うことができます`dotnet add package <package-name>`、プロジェクトでは自動的に_アップグレード_パッケージを最新のバージョン。 これは、IDE を開くことがなく依存関係を更新する簡単な方法です。

## <a name="take-advantage-of-built-in-security"></a>組み込みのセキュリティを活用します。

常にどのようなセキュリティ機能のフレームワークのオファリングを確認します。 **決して**が標準的な技法や機能が組み込まれている場合は、独自のセキュリティをロールバックします。 さらに、実証されたアルゴリズムに依存しているとワークフローの専門家の多くで調査されている多くの場合、これらために critiqued し強化のため、信頼性が高く、セキュリティで保護されることを確認できます。

.NET Core framework が無数のセキュリティ機能は、いくつかのコア ドキュメントの場所を開始します。
* [認証-Id 管理](https://docs.microsoft.com/aspnet/core/security/authentication/index?view=aspnetcore-2.1)
* [承認](https://docs.microsoft.com/aspnet/core/security/authorization/index?view=aspnetcore-2.1)
* [データ保護](https://docs.microsoft.com/aspnet/core/security/data-protection/index?view=aspnetcore-2.1)
* [セキュリティで保護された構成](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/index?view=aspnetcore-2.1)
* [セキュリティの機能拡張 Api](https://docs.microsoft.com/aspnet/core/security/data-protection/extensibility/index?view=aspnetcore-2.1)

これらの機能のいずれかが書き込まれ、フィールドに、専門家によるは予定どおりに動作することを確認するためのテストと意図したとおりにのみ、使い込まれました。 その他のフレームワーク プランのような機能 - 各カテゴリがあることを確認するフレームワークを提供するベンダーに確認します。

> [!WARNING]
> 使用して、フレームワークによって提供されるものではなく、独自のセキュリティ コントロールを記述がないだけ時間を無駄、セキュリティが低下します。


## <a name="azure-security-center"></a>Azure Security Center

Azure を使用して、web アプリケーションをホストする Security Center は警告を表示する推奨事項 タブの一部として、フレームワークが、最新でないかどうか。忘れずにかどうかは、アプリに関連するすべての警告を表示する時間の経過に探します。

![Azure Security Center の推奨のフレームワークをアップグレードします。](../media-draft/ASCFramework.png)


## <a name="summary"></a>まとめ

可能であればは、アプリを構築、組み込みのセキュリティ機能を使用して、常に最新の状態を保つことを確認するために最新のフレームワークを選択します。 これらの単純なルールは、アプリケーションの強固な基盤を開始することを確認するのに役立ちます。
