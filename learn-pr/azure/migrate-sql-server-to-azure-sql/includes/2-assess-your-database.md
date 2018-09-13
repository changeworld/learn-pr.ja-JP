Azure の拡張性と可用性を利用するため、Azure SQL Database に SQL Server データベースを移動することにしました。 ただし、オンプレミスの SQL Server で現在使用しているすべての機能が Azure SQL Server で提供されることを確認する必要があります。

既存データベースの Azure SQL Server との互換性を評価するには、Data Migration Assistant (DMA) と呼ばれるツールを使用します。

## <a name="what-is-the-data-migration-assistant-dma"></a>Data Migration Assistant (DMA) とは

Data Migration Assistant (DMA) ツールは、オンプレミスの SQL Server インスタンスを分析することを目的として設計されています。 このツールでは、Azure SQL Server への SQL データベースの移行を妨げる可能性のある一般的な問題が検出されて報告されます。

移行をブロックする可能性がある互換性の問題が検出され、オンプレミスのサーバーで使用されている機能のうち部分的にサポートされているもの、または全くサポートされていないものが認識されます。

また、移行の前に、オンプレミスのサーバーで実行すべき包括的なレコメンデーションも提供されます。

### <a name="why-do-you-need-data-migration-assistant"></a>Data Migration Assistant が必要な理由

Azure SQL Database は開発が続けられており、現時点では SQL Server のすべての機能をサポートしてはいません。

最新の一覧については、[Azure SQL Database の機能一覧](https://docs.microsoft.com/azure/sql-database/sql-database-features)をご覧ください。

## <a name="how-to-assess-your-database-using-data-migration-assistant"></a>Data Migration Assistant を使用してデータベースを評価する方法

通常、Data Migration Assistant を使用したデータベースの評価は以下の手順で行います。

- **Data Migration Assistant をインストールする** – DMA を https://www.microsoft.com/download/details.aspx?id=53595 からダウンロードしてインストールします。 Azure SQL Database は定期的に更新されます。DMA も、新しい機能を反映するために更新されます。 最新バージョンが確実にインストールされるように、インストーラーを実行することをお勧めします。
- **評価を作成する** – オンプレミスのデータベースとターゲット データベースを定義する新しい評価を作成します。 Azure Virtual Machines 上の別の SQL Server ターゲットを評価して、別の移行と比較することもできます。
- **評価とデータベースソースのオプションを選択**– 例えば、2 つのデータベース間での互換性を確認するか、または、機能パリティーを確認するかなど、オプションを選択します。 そのあと、ソース データベースを選択します。 複数のソースを選択できます。
- **結果を確認する** - 詳細な結果を使用して、エラーを確認し、是正措置を実行できます。 結果には、クロスデータベース参照と SQL Server エージェントでサポートされていない機能の一覧が表示されます。 また、フル テキスト検索と監査による、部分的にサポートされている機能の一覧も表示されます。 結果では、可能性のあるエラーと、それらのエラーを修正する方法についてのアドバイスが提供されます。 Data Migration Assistant の結果は、.json ファイルとしてエクスポートできます。
