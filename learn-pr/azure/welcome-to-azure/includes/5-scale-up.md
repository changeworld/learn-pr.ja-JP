Web サーバーは稼働するようになりましたが、エクスペリエンスをユーザーの役に立つものにするにはさらにコンピューティング能力が必要です。 どうすれば VM をもっと速く実行させることができますか。

自社のデータ センターであれば、Web サーバーをいっそう強力なハードウェアに移動することで、パフォーマンスの問題を解決できるかもしれません。 問題は、新しいシステムを購入し、ラックに格納し、電源を供給する必要があることです。 Azure なら答えははるかに簡単です。

そのような場合は、より強力なサイズに VM をスケールアップします。 VM のサイズを変更する前に、VM をシャットダウンするか、または割り当て解除する必要があります。

最初に、スケーリングの意味を定義し、VM の割り当てを解除するとどうなるかを説明します。

## <a name="what-is-scale"></a>スケーリングとは

"_スケーリング_" とは、よりよいパフォーマンスを実現するために、ネットワーク帯域幅、メモリ、ストレージ、またはコンピューティング能力を追加することです。  

"_スケールアップ_" や "_スケールアウト_" という用語を聞いたことがあるかもしれません。

スケールアップまたは垂直スケーリングとは、既存の仮想マシンのメモリ、ストレージ、またはコンピューティング能力を増やすことです。 たとえば、Web サーバーやデータベース サーバーにメモリを追加して、さらに高速で実行させることができます。

> [!TIP]
> また、スケールアップが一時的にのみ必要であった場合は、システムを "_スケールダウン_" することもできます。

スケールアウトまたは水平スケーリングとは、仮想マシンを追加してアプリケーションの能力を高めることです。 たとえば、まったく同じ構成方法で多数の仮想マシンを作成し、ロード バランサーを使用してそれらの間に処理を分散させることができます。

## <a name="what-is-deallocation"></a>割り当て解除とは

割り当て解除とは、VM をシャットダウンし、そのコンピューティング リソースを解放するプロセスです。

職場や家庭の PC をシャットダウンすると、オペレーティング システムによってプログラムが終了され、電源をオフにするよう電源管理ハードウェアに通知されます。

仮想マシンの割り当て解除もそれと似ています。 VM がシャットダウンすると、Azure はその VM が使用していたハードウェアを再利用します。 データ ディスクとストレージはそのまま残ります。 VM を再び起動すると、PC と同様に中断したところから再開します。

割り当てを解除すると、仮想マシンが使用するコンピューティング リソースとネットワーク リソースは課金されなくなります。 ストレージに関連するディスクに対してはやはり課金されますが、全体的なコストは VM が実行されている場合よりはるかに低くなります。

ここでは、サイズを変更できるように、VM の割り当てを短時間解除します。 ただし、コストを節約するため VM の割り当てを長期間解除することもできます。 営業時間中にテストのために使用する VM のバンクがあるものとします。 夜間と週末には自動的に割り当てが解除されるように、VM をスケジュールすることができます。 遅くまで使用する必要がある場合は、手動で再起動できます。

## <a name="scale-up-your-vm"></a>VM をスケールアップする

VM を作成するときに、サイズ **Standard_DS2_v2** を指定したことを思い出してください。 現在、VM は 2 つの仮想 CPU と 7 GB のメモリを備えています。

1 つ上のサイズ **Standard_DS3_v2** に増強してみましょう。 VM は 4 つの仮想 CPU と 14 GB のメモリを備えるようになります。

::: zone pivot="windows-cloud"

1. Cloud Shell から `az vm deallocate` を実行し、VM の割り当てを解除するか、または VM を停止します。

    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myWindowsVM
    ```
    プロセスが完了するまでに数分かかります。
1. `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。

    ```azurecli
    az vm resize \
      --resource-group myResourceGroup \
      --name myWindowsVM \
      --size Standard_DS3_v2
    ```
    更新プロセスには約 1 分かかります。
1. `az vm start` を実行して VM を再起動します。

    ```azurecli
    az vm start \
      --resource-group myResourceGroup \
      --name myWindowsVM
    ```
    プロセスには約 1 分かかります。
1. `az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group myResourceGroup \
      --name myWindowsVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    新しい VM サイズ **Standard_DS3_v2** が表示されます。
    ```console
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Cloud Shell から `az vm deallocate` を実行し、VM の割り当てを解除するか、または VM を停止します。

    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myLinuxVM
    ```
    プロセスが完了するまでに数分かかります。
1. `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。

    ```azurecli
    az vm resize \
      --resource-group myResourceGroup \
      --name myLinuxVM \
      --size Standard_DS3_v2
    ```
    更新プロセスには約 1 分かかります。
1. `az vm start` を実行して VM を再起動します。

    ```azurecli
    az vm start \
      --resource-group myResourceGroup \
      --name myLinuxVM
    ```
    プロセスには約 1 分かかります。
1. `az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group myResourceGroup \
      --name myLinuxVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    新しい VM サイズ **Standard_DS3_v2** が表示されます。
    ```console
    Standard_DS3_v2
    ```

::: zone-end

> [!NOTE]
> 既定では、VM を停止して再起動すると、Azure によって VM に新しいパブリック IP アドレスが割り当てられます。 学習目的の場合はこれで問題ありません。 実際には、VM を再起動しても VM のパブリック IP アドレスが変わらないように予約しておくことができます。

## <a name="summary"></a>まとめ

ごくろうさま。 ほんの数コマンドで、VM の能力が 2 倍になりました。

スケールアップとスケールアウトは、パフォーマンスを向上させる 2 つの方法です。 ここでは、VM をスケールアップしてそのコンピューティング能力を増強しました。

サイズを変更する前に、VM の割り当てを解除します。 コストを節約するために使用しなくなった VM の割り当てを解除することもできます。