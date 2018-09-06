Azure SQL Database が稼働しているので、お気に入りの SQL Server 管理ツールに接続して実際のデータを入力することができます。

最初に、データベースをオンプレミスとクラウドのどちらで実行するかを検討しました。 Azure SQL Database を使用していくつかの基本的なオプションを構成し、アプリに接続できる完全な機能を備えた SQL データベースがあります。

保守が必要なインフラストラクチャやソフトウェア修正プログラムはありません。 輸送物流アプリのプロトタイプを稼働させることに労力を割き、データベース管理にかかる作業を減らすことができるようになりました。 また、このプロトタイプは無駄になるデモではありません。 Azure SQL Database には、運用レベルのセキュリティとパフォーマンスの機能があります。

各 Azure SQL 論理サーバーには、1 つ以上のデータベースが含まれています。 Azure SQL Database には DTU と仮想コアという 2 つの価格モデルがあり、すべてのデータベースでコストとパフォーマンスのバランスをとることができます。

初めて使用する場合や、シンプルで事前に構成された購入オプションが必要な場合は、DTU を選択します。 作成して支払うコンピューティング リソースとストレージ リソースをきめ細かく制御するには、仮想コアを選択します。

Cloud Shell を使用すると、データベースの操作を簡単に開始できます。 Cloud Shell から Azure CLI にアクセスできます。そのため、Azure リソースに関する情報を取得できます。 また、Cloud Shell には `sqlcmd` などの他の一般的なユーティリティも多く用意されているので、新しいデータベースをすぐに使用できます。

## <a name="cleanup"></a>クリーンアップ

Azure SQL Database のインストールを使用して、他にも自由に実験してみてください。 終わった後にデータベースを削除する最も簡単な方法は、親リソース グループを削除することです。

1. ポータルで **[リソース グループ]** をクリックします。
1. **logistics-db-rg** を選択します。
1. **[リソース グループの削除]** をクリックします。

    ![リソース グループを削除する](../media-draft/delete-rg.png)
1. プロンプトで「logistics-db-rg」と入力し、**[削除]** をクリックします。

## <a name="additional-resources"></a>その他のリソース

ドキュメントには、チュートリアルやサンプルなど、多くの情報が提供されています。 ここで取り上げた内容に関するリンクを次にいくつか示します。

* [Azure SQL Database のドキュメント](https://docs.microsoft.com/en-us/azure/sql-database/)
* [Azure SQL Database の購入モデルとリソース](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-service-tiers)
* [Azure SQL Database 論理サーバーとその管理](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-logical-servers)
* [Azure SQL Database と SQL Data Warehouse のファイアウォール規則](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure)

Cloud Shell の詳細については、[Azure Cloud Shell の概要](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)に関するページを参照してください。

`sqlcmd` ユーティリティの詳細について関心をお持ちの場合は、[sqlcmd ユーティリティ](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-2017)に関するページを参照してください。
