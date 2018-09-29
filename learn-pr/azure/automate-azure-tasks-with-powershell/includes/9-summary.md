<span data-ttu-id="26e11-101">このモジュールでは、複数の VM の作成を自動化するスクリプトを書きました。</span><span class="sxs-lookup"><span data-stu-id="26e11-101">In this module, we wrote a script to automate the creation of multiple VMs.</span></span> <span data-ttu-id="26e11-102">スクリプトは比較的短いものでしたが、PowerShell のループ、変数、関数を Azure PowerShell のコマンドレットと組み合わせることで、潜在的な機能の高さを確認できます。</span><span class="sxs-lookup"><span data-stu-id="26e11-102">Even though the script was relatively short, you can see the potential power when you combine loops, variables, and functions from PowerShell with cmdlets from Azure PowerShell.</span></span>

<span data-ttu-id="26e11-103">Azure PowerShell は、PowerShell の経験を持つ管理者にとって、適切な自動化の選択肢です。</span><span class="sxs-lookup"><span data-stu-id="26e11-103">Azure PowerShell is a good automation choice for admins with PowerShell experience.</span></span> <span data-ttu-id="26e11-104">簡潔な構文と強力なスクリプト言語の組み合わせは、PowerShell を初めて使う場合であっても、検討に値します。</span><span class="sxs-lookup"><span data-stu-id="26e11-104">The combination of clean syntax and a powerful scripting language also makes it worth considering even if you are new to PowerShell.</span></span> <span data-ttu-id="26e11-105">エラーが発生しやすく時間がかかるタスクを対象にしたこのレベルの自動化は、管理時間の削減と品質の向上につながります。</span><span class="sxs-lookup"><span data-stu-id="26e11-105">This level of automation for time-consuming and error-prone tasks should help you reduce administrative time and increase quality.</span></span>

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="26e11-106">自分のサブスクリプションで実行している場合、次の PowerShell コマンドレットを使用し、リソース グループ (とすべての関連リソース) を削除できます。</span><span class="sxs-lookup"><span data-stu-id="26e11-106">When you are running in your own subscription, you can use the following PowerShell cmdlet to delete the resource group (and all related resources).</span></span>

```powershell
Remove-AzureRmResourceGroup -Name MyResourceGroupName
```

<span data-ttu-id="26e11-107">削除の確定を求められたら、**[はい]** と答えてください。あるいは `-Force` パラメーターを追加すれば、このプロンプトを省略できます。</span><span class="sxs-lookup"><span data-stu-id="26e11-107">When you are asked to confirm the delete, answer **Yes**, or you can add the `-Force` parameter to skip the prompt.</span></span> <span data-ttu-id="26e11-108">コマンドは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="26e11-108">The command may take several minutes to complete.</span></span>