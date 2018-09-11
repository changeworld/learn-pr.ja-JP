仮想マシンを作成するためのイメージに `Debian` を使用しました。 Azure には、仮想マシンの作成に使用できる標準的な VM イメージがいくつか用意されています。 

`az vm image list --output table` コマンドを使用して、使用可能なイメージのリストを取得することができます。 このコマンドを使用すると、Azure CLI に組み込まれているオフラインのリストの一部である最も一般的なイメージが出力されます。 ただし、Azure Marketplace には、_数百_の使用可能なイメージ オプションがあります。 

> [!TIP]
> 完全なリストを取得するには、コマンドに `--all` フラグを追加します。 Marketplace のイメージのリストは非常に大きいので、`--publisher` または `–-offer` のオプション使用して、リストをフィルター処理すると便利です。

一部のイメージは、特定の場所でのみ使用可能です。 コマンドに `--location [location]` フラグを追加して、仮想マシンを作成するリージョンで使用可能なものに結果を絞り込んでみましょう。 たとえば、Azure Cloud Shell に次を入力して、`eastus` リージョンで使用できるイメージのリストを取得します。

```azurecli
az vm image list --location eastus --output table
```

> [!NOTE]
> これらは、Azure で提供されている標準のイメージです。 固有の構成またはオペレーティング システムのあまり一般的ではないバージョンまたはディストリビューションに基づいて VM を作成するために、[独自のカスタム イメージを作成してアップロード](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images)することもできます。