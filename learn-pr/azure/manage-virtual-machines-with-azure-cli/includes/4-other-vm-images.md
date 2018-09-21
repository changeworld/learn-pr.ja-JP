仮想マシンを作成するためのイメージに `Debian` を使用しました。 Azure には、仮想マシンの作成に使用できる標準的な VM イメージがいくつか用意されています。 

## <a name="listing-images"></a>イメージの一覧表示

次のコマンドを使用して、使用可能な VM イメージのリストを取得できます。 

```azurecli
az vm image list --output table
```

このコマンドを使用すると、Azure CLI に組み込まれているオフライン リストの一部である最も一般的なイメージが出力されます。 ただし、Azure Marketplace には、_数百_の使用可能なイメージ オプションがあります。 

## <a name="getting-all-images"></a>すべてのイメージの取得

完全なリストを取得するには、コマンドに `--all` フラグを追加します。 Marketplace のイメージのリストは非常に大きいので、`--publisher`、`--sku`、または `–-offer` のオプション使用して、リストをフィルター処理すると便利です。

たとえば、次のコマンドを試行して、使用可能な_すべて_の Wordpress イメージを確認します。

```azurecli
az vm image list --sku Wordpress --output table --all
```

または、次のコマンドで Microsoft が提供するすべてのイメージを確認します。

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a>場所に固有のイメージ

一部のイメージは、特定の場所でのみ使用可能です。 コマンドに `--location [location]` フラグを追加して、仮想マシンを作成するリージョンで使用可能なものに結果を絞り込んでみましょう。 たとえば、Azure Cloud Shell に次のように入力して、`eastus` リージョンで使用できるイメージのリストを取得します。

```azurecli
az vm image list --location eastus --output table
```

その他の使用可能な Azure Sandbox の場所にあるイメージの一部をチェックするには、次を試行します。

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> これらは、Azure で提供されている標準のイメージです。 固有の構成やオペレーティング システムのあまり一般的ではないバージョンまたはディストリビューションに基づいて VM を作成するために、[独自のカスタム イメージを作成してアップロード](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images)することもできます。

おそらく、コマンド ウィンドウの表示はこの時点で限界になります。必要に応じて `clear` を入力して画面をクリアできます。