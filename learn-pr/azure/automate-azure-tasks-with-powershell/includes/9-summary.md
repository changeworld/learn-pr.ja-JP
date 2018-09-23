このモジュールでは、複数の VM の作成を自動化するスクリプトを書きました。 スクリプトは比較的短いものでしたが、PowerShell のループ、変数、関数を Azure PowerShell のコマンドレットと組み合わせることで、潜在的な機能の高さを確認できます。

Azure PowerShell は、PowerShell の経験を持つ管理者にとって、適切な自動化の選択肢です。 簡潔な構文と強力なスクリプト言語の組み合わせは、PowerShell を初めて使う場合であっても、検討に値します。 エラーが発生しやすく時間がかかるタスクを対象にしたこのレベルの自動化は、管理時間の削減と品質の向上につながります。

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

自分のサブスクリプションで実行している場合、次の PowerShell コマンドレットを使用し、リソース グループ (とすべての関連リソース) を削除できます。

```powershell
Remove-AzureRmResourceGroup -Name MyResourceGroupName
```

削除の確定を求められたら、**[はい]** と答えてください。あるいは `-Force` パラメーターを追加すれば、このプロンプトを省略できます。 コマンドは、完了までに数分かかる場合があります。