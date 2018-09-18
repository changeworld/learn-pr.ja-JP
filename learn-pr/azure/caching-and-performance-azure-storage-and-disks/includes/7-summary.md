このモジュールでは、Azure ディスク キャッシュと、それによりパフォーマンスを向上させる可能性について学習しました。 VM のディスク キャッシュの管理に、Azure portal と Azure PowerShell を使用しました。 

Azure VM ディスク キャッシュの方法を決めると、スクリプトとテンプレートを使用して、最適なディスク キャッシュ設定で新しい VM とディスクを迅速かつ簡単にデプロイできます。

## <a name="further-reading"></a>参考資料

- [Azure Premium Storage: 高パフォーマンスのための設計](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [Azure PowerShell の概要](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [Azure Computer コマンドレット リファレンス](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## <a name="cleanup"></a>クリーンアップ
<!---TODO: Update for sandbox?--->

Azure VM を実行すると、サブスクリプションに対してコストが発生します。 不必要な課金を避けるために不要なリソースを削除します。 Azure サブスクリプションをクリーンアップする最も簡単な方法は、リソース グループを削除することです。 リソース グループを削除すると、そのグループ内にあるすべてのリソースが削除されます。 このモジュールが終了したら、次の Azure PowerShell コマンドレットを実行します。

    ```powershell
    Remove-AzureRmResourceGroup -Name "fotoshare-rg"
    ```

削除の確認を求められたら、**[はい]** と答えます。 リソースが削除済みとなってコマンドが完了するまで、数分かかる場合があります。
