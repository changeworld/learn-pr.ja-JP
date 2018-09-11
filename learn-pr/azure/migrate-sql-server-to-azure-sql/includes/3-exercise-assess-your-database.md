このユニットでは、Data Migration Assistant を使用して既存のデータベースを評価し、現在、Azure SQL Database でサポートされていないローカルの SQL Server インスタンスで使用される機能を確認します。

## <a name="setup"></a>セットアップ

1. まだインストールしていない場合は、[**Data Migration Assistant** をインストール](https://www.microsoft.com/download/details.aspx?id=53595)します。

1. SQL Server インスタンスを実行している必要であり、接続の詳細を利用できることを確認します。

<!-- 1. [**** likely replace with an LOD VM *****] TODO: -->

1. インターネット ブラウザーを起動して、 https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017 に移動します。

1. 「**OLTP downloads**」(OLTP のダウンロード) で **AdventureWorks2008R2.bak** をクリックして、お使いローカル コンピューターに保存します。

1. Management Studio で、*AdventureWorks 2008R2* を既定のインスタンスに復元します。

## <a name="create-an-assessment"></a>評価を作成する

1. **Microsoft Data Migration Assistant** を起動します。

1. アプリの左側にあるナビゲーションで、__[+]__ をクリックして新しい DMA プロジェクトを作成します。

1. 次のオプションを指定します。

    - **[プロジェクトの種類]** - *[評価]* を選択します
    - **[プロジェクト名]** - プロジェクトの名前を入力します (例: "Bicycle DB Assessment")
    - **[Source server type]\(ソース サーバーの種類\)** - *[SQL Server]* を選択します
    - **[Target server type]\(対象サーバーの種類\)** - *[Azure SQL Database]* を選択します

1. **[作成]** をクリックします。
    ![Data Migration Assistant でご利用の AdventureWorks SQL Server データに対して記述された構成を示すスクリーンショット。](../media-draft/3-create-assessment.png)

1. 評価レポートの種類を選択します。次の両方をオンにします。
    - データベースの互換性を確認します
    - 機能パリティを確認します

1. **[次へ]** をクリックします。

## <a name="add-databases-to-assess"></a>評価するデータベースを追加する

1. **[ソースの追加]** をクリックして接続メニューを開きます。

1. 以下の手順を実行します。

    - 既存の SQL Server インスタンスの名前を入力します
    - **[認証]** の種類を選択します
    - サーバーの接続プロパティを指定します

1. **[接続]** をクリックします。

1. **[ソースの追加]** で、評価するデータベースを選択します。 この演習では、**AdventureWorks2008R2** を選択します。

1. **[追加]** をクリックします。
    > [!NOTE]
    > 複数の SQL Server インスタンスからデータベースを追加するには、**[ソースの追加]** ボタンを使用します。 複数のデータベースを削除するには、Shift + Ctrl キーを押しながら削除するデータベースを選択して、**[ソースの削除]** をクリックします。

1. **[評価の開始]** をクリックします。

## <a name="view-results"></a>結果を表示する

複数のデータベースがある場合、各データベースの結果は利用できるようになるとすぐに表示されます。 すべてのデータベースの評価が完了するまで待機する必要はありません。

1. **AdventureWorks** の評価が完了したら、**[互換性の問題]** と **[機能に関する推奨事項]** をクリックして結果を表示します。

    - SQL Server の機能パリティ カテゴリには、完全にサポートされない可能性がある機能とこれらの問題を解決するための手順が一覧表示されます。 機能パリティの問題では、移行は停止されません。
    - 互換性の問題カテゴリには、移行を妨げる機能とこれらの問題を解決するための手順が一覧されます。

## <a name="summary"></a>まとめ

このユニットでは、ローカルにインストールされた SQL Server データベースを評価して、データベースを Azure SQL Database に移行したときに、利用できない機能があるかどうかを検証しました。