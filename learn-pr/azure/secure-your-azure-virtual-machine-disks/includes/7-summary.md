Azure では、Azure VM のディスクをセキュリティで保護するために、Storage Service Encryption (SSE) と Azure Disk Encryption (ADE) が用意されています。 これらのテクノロジが連携して、Azure VM ディスクを保護するための多層防御アプローチの一部として、強力な 256 ビット暗号化を提供します。 ディスクの暗号化を有効にするには、Azure Disk Encryption の前提条件が満たされている必要があります。 Azure Disk Encryption の前提条件構成スクリプトを使用すると、このプロセスを自動化できます。 新しい VM で暗号化を有効にするとき、Azure Resource Manager テンプレートを使用できます。 これによりデプロイの時点でデータが確実に暗号化され、脆弱性を与えません。

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>その他のリソース

- [Azure VM Disk encryption のトラブルシューティング](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-tsg)
- [Azure で Linux 仮想マシンを暗号化する方法](https://docs.microsoft.com/azure/virtual-machines/linux/encrypt-disks)
- [Azure Disk Encryption の概要](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)
- [Azure Disk Encryption は、どの Linux ディストリビューションをサポートしていますか。](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-faq#bkmk_LinuxOSSupport)
- [GitHub の Resource Manager テンプレート](https://github.com/Azure/azure-quickstart-templates)