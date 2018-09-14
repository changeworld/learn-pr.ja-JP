Web サーバーが稼働しが実行されている、ユーザーの優れたエクスペリエンスを実現する複数の処理能力が必要になったらします。 高速に実行して VM を行う方法

データ センター、パフォーマンスの問題を解決するためより強力なハードウェアを web サーバーを移動する可能性があります。 問題は、購入、ラック取り付け、および、新しいシステムの電源をする必要があります。 Azure では、その答えははるかに簡単です。

ここでより強力なサイズに VM をスケール アップします。 VM のサイズを変更する前に、シャット ダウン、または割り当てを解除する必要があります。

最初に、どのようなスケールを意味し、VM の割り当てを解除するときの動作を定義してみましょう。

## <a name="what-is-scale"></a>スケールとは何ですか。

_スケール_追加のネットワーク帯域幅、メモリ、ストレージ、またはパフォーマンスの向上を実現するためにコンピューティング能力を参照します。  

条件を聞いている_スケール アップ_と_スケール アウト_します。

スケール アップ、または垂直方向のスケーリングは、メモリ、記憶域を増やすか、既存の仮想マシンの電源を入れますのコンピューティングを意味します。 たとえば、高速に実行されるように web データベースまたはデータベース サーバーに追加のメモリを追加できます。

> [!TIP]
> できます_スケール ダウン_のみ一時的にスケール アップする必要がある場合、システム。

スケール アウト、または水平方向のスケーリングは、電源、アプリケーションを追加の仮想マシンを意味します。 たとえばとまったく同じ方法で構成されている多数の仮想マシンを作成し、それらの間で作業を分散するロード バランサーを使用する可能性があります。

## <a name="what-is-deallocation"></a>割り当て解除とは何ですか。

割り当て解除は、VM をシャット ダウンし、そのコンピューティング リソースを解放するプロセスです。

職場や家庭で PC をシャット ダウンするときに、オペレーティング システムは、プログラムが終了し、し、電源をオフにする電源管理のハードウェアを通知します。

仮想マシンの割り当てを解除することは似ています。 VM がシャット ダウンした後、Azure は電源を使用しているハードウェアを解放します。 データ ディスクとストレージは、そのまま残ります。 バックアップする VM を開始するときに中断から再開、お使いの PC と同様です。

この割り当てを解除すると、仮想マシンが使用するコンピューティングとネットワークのリソースの課金はされません。 記憶域にとどまるように、関連付けられているディスクに対しても課金するが、全体的なコストは、VM が実行されているかどうかはそれよりもはるかに劣ります。

ここを VM の割り当て解除、簡単にサイズを変更できるようにします。 ただし、長い期間のコストを節約する Vm の割り当てを解除することもできます。 作業時間をテストするために使用する Vm の銀行があるとします。 夜間と週末で自動的に割り当てを解除する Vm をスケジュールすることができます。 常に遅延する場合は、それらを手動で再起動できます。

## <a name="scale-up-your-vm"></a>VM をスケール アップします。

サイズが指定されていることを思い出してください**Standard_DS2_v2** VM の作成時にします。 VM は、現在、2 つの仮想 Cpu と 7 GB のメモリが。

[次へ] のサイズを増やしてみましょう**Standard_DS3_v2**します。 VM には、次の 4 つの仮想 Cpu と 14 GB のメモリがなります。

::: zone pivot="windows-cloud"

1. Cloud Shell から実行`az vm deallocate`割り当てを解除すると、または VM を停止します。

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    プロセスを完了するまでに数分かかります。
1. 実行`az vm resize`に VM のサイズを増やす**Standard_DS3_v2**します。

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --size Standard_DS3_v2
    ```
    更新プロセスは、約 1 分かかります。
1. 実行`az vm start`VM を再起動します。

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    この処理は、約 1 分かかります。
1. 実行`az vm show`を VM で新しいサイズが実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    表示、新しい VM サイズ**Standard_DS3_v2**します。
    ```console
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Cloud Shell から実行`az vm deallocate`割り当てを解除すると、または VM を停止します。

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    プロセスを完了するまでに数分かかります。
1. 実行`az vm resize`に VM のサイズを増やす**Standard_DS3_v2**します。

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --size Standard_DS3_v2
    ```
    更新プロセスは、約 1 分かかります。
1. 実行`az vm start`VM を再起動します。

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    この処理は、約 1 分かかります。
1. 実行`az vm show`を VM で新しいサイズが実行されていることを確認します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    表示、新しい VM サイズ**Standard_DS3_v2**します。
    ```console
    Standard_DS3_v2
    ```

::: zone-end

> [!NOTE]
> 既定では、Azure では停止して再起動したときに、新しいパブリック IP アドレスを VM に割り当てます。 学習目的の場合は問題ありません。 実際には、VM が再起動される場合でも VM で保持されるパブリック IP アドレスを予約できます。

## <a name="summary"></a>まとめ

すばらしいです。 いくつかのコマンドを使用して、VM はようになりました 2 回に強力です。

スケール アップとスケール アウトは、パフォーマンスを向上させる 2 つの方法です。 ここで、vm のコンピューティング能力を増やすに拡大縮小します。

サイズを変更する前に、VM の割り当て解除します。 コストを節約する使用中でないときに、Vm の割り当て解除できますも。