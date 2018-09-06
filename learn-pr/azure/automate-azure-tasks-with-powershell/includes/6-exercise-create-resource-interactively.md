Linux 管理ツールのスイートを作成する会社で働いているものとします。 仕事は、見込みのあるお客様が購入する前にソフトウェアを試すのを支援することです。 ソフトウェアでは OS に対してルート レベルの変更を加えるため、試用版のお客様ごとに Linux VM を作成することにしました。 必要に応じて VM を作成し、試用サブスクリプションの最後にそれらを削除します。 このようにすると、各お客様はクリーンなバージョンの OS で始めることができます。 

これらの VM を、内部テスト用に会社で使用されている VM から分離しておくため、それらを格納するための専用のリソース グループを作成します。 必要なリソース グループは 1 つだけなので、このタスクには対話モードの Azure PowerShell を使用するのが妥当です。

## <a name="steps-to-create-a-resource-group"></a>リソース グループの作成手順

1. PowerShell を起動します。

1. Azure コマンドレットにアクセスできるように、現在のセッションにモジュールをインポートします。

   ```powershell
   Import-Module AzureRM
   ```

1. 次に示すコマンドを使用して、Azure に接続します。 コマンドを入力した後、Azure 資格情報を指定して認証を行います。

   ```powershell
   Connect-AzureRmAccount
   ```

1. リソース グループを作成します。

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. リソース グループが正常に作成されたことを確認します。

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
リソース グループが正常に作成されたかどうかは、Azure portal を使用して確認することもできます。 そのためには、ポータルにログインし、**[リソース グループ]** セクション (下記参照) に移動します。 新しいリソース グループが一覧に表示されるはずです。

次のスクリーンショットでは、Azure portal でのリソース グループ カテゴリの場所を示します。

![Azure portal の [お気に入り] ブレードでリソース グループ カテゴリが強調表示されているスクリーンショット。](../media/6-listing-resource-groups.png)

## <a name="summary"></a>まとめ
この演習では、対話型 PowerShell セッションの一般的なパターンを示します。 標準的なコマンドレットを使用して AzureRM モジュールをインポートしてから、Azure PowerShell コマンドレットを使用して特定のタスクを実行しました。 サブスクリプションにリソース グループが作成されて、VM を作成する準備が整いました。