Web サーバーは稼働するようになりましたが、エクスペリエンスをユーザーの役に立つものにするにはさらにコンピューティング能力が必要です。 どうすれば VM をもっと速く実行させることができますか。

自社のデータ センターであれば、Web サーバーをいっそう強力なハードウェアに移動することで、パフォーマンスの問題を解決できるかもしれません。 問題は、新しいシステムを購入し、ラックに格納し、電源を供給する必要があることです。 Azure なら答えははるかに簡単です。

より強力なサイズに VM をスケールアップする前に、まずはスケーリングの意味を定義してみましょう。

## <a name="what-is-scale"></a>スケーリングとは

"_スケーリング_" とは、よりよいパフォーマンスを実現するために、ネットワーク帯域幅、メモリ、ストレージ、またはコンピューティング能力を追加することです。  

"_スケールアップ_" や "_スケールアウト_" という用語を聞いたことがあるかもしれません。

スケールアップまたは垂直スケーリングとは、既存の仮想マシンのメモリ、ストレージ、またはコンピューティング能力を増やすことです。 たとえば、Web サーバーやデータベース サーバーにメモリを追加して、さらに高速で実行させることができます。

スケールアウトまたは水平スケーリングとは、仮想マシンを追加してアプリケーションの能力を高めることです。 たとえば、まったく同じ構成方法で多数の仮想マシンを作成し、ロード バランサーを使用してそれらの間に処理を分散させることができます。

> [!TIP]
> クラウドには柔軟性があります。 スケールアップまたはスケールアウトが一時的にのみ必要であった場合は、展開を "_スケールダウン_" または "_スケールイン_" できます。 スケールダウンまたはスケールインはコストの削減に役立ちます。<br><br>**Azure Advisor** と **Azure Cost Management** の 2 つのサービスを使用して、クラウド支出を最適化できます。 これらのサービスを利用して必要以上に使用している部分を識別し、実際に使用している量にスケールを戻すことができます。

## <a name="scale-up-your-vm"></a>VM をスケールアップする

VM を作成するときに、サイズ **Standard_DS2_v2** を指定したことを思い出してください。 現在、VM は 2 つの仮想 CPU と 7 GB のメモリを備えています。

1 つ上のサイズ **Standard_DS3_v2** に増強してみましょう。 VM は 4 つの仮想 CPU と 14 GB のメモリを備えるようになります。

::: zone pivot="windows-cloud"

1. Cloud Shell から `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    更新プロセスには約 1 分かかります。 この処理中に VM が再起動します。

1. `az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    新しい VM サイズ **Standard_DS3_v2** が表示されます。
    ```output
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Cloud Shell から `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    更新プロセスには約 1 分かかります。 この処理中に VM が再起動します。

1. `az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    新しい VM サイズ **Standard_DS3_v2** が表示されます。
    ```output
    Standard_DS3_v2
    ```

::: zone-end

## <a name="summary"></a>まとめ

ごくろうさま。 ほんの 1 コマンドで、VM の能力が 2 倍になりました。

スケールアップとスケールアウトは、パフォーマンスを向上させる 2 つの方法です。 ここでは、VM をスケールアップしてそのコンピューティング能力を増強しました。