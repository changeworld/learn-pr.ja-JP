仮想マシンのサイズは、予定される作業に適したものであることが必要です。 VM のメモリの大きさや CPU が適切でない場合は、負荷を処理できなくなることや、動作が遅すぎて実用的ではなくなることがあります。 

仮想マシンを作成するときに、_VM サイズ_値を指定することができます。この値によって、その VM に割り当てられるコンピューティング リソースの量が決まります。 これには、Azure から仮想マシンに使用できるようにする CPU、GPU、およびメモリが含まれます。

Azure では、予想される使用量に基づいて選択するための Linux と Windows 用の[定義済みの VM サイズ](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)のセットが定義されています。 

| 種類 | サイズ | 説明 |
|------|-------|-------------|
| 汎用   | Dsv3、Dv3、DSv2、Dv2、DS、D、Av2、A0 - 7 | CPU とメモリのバランスがとれています。 開発/テスト環境や、小中規模のアプリケーションとデータ ソリューションに最適です。 |
| コンピューティング最適化 | Fs、F | メモリに対する CPU の比が大きくなっています。 トラフィックが中程度のアプリケーション、ネットワーク アプライアンス、バッチ処理に適しています。 |
| メモリ最適化  | Esv3、Ev3、M、GS、G、DSv2、DS、Dv2、D   | コアに対するメモリの比が大きくなっています。 リレーショナル データベース、中から大規模のキャッシュ、およびインメモリ分析に適しています。 |
| ストレージ最適化 | Ls | 高いディスク スループットと IO。 ビッグ データ、SQL、および NoSQL のデータベースに最適です。 |
| GPU 最適化 | NV、NC | 負荷の大きいグラフィックスのレンダリングやビデオ編集に特化した VM です。 |
| 高性能 | H、A8 ～ 11 | オプションで高スループットのネットワーク インターフェイス (RDMA) を備えた、最も強力な CPU VM です。 | 

使用可能なサイズは、VM を作成するリージョンに基づいて変わります。 `vm list-sizes` コマンドを使用して、使用可能なサイズのリストを取得することができます。 これを Azure Cloud Shell に入力してみてください。

```azurecli
az vm list-sizes --location eastus --output table
```

`eastus` の省略形の応答は次のとおりです。

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

VM を作成したときにサイズを指定しなかったので、Azure によって `Standard_DS1_v2`の既定の汎用サイズが選択されました。 ただし、サイズは `vm create` コマンドの一部として `--size` パラメーターを使用して指定できます。 たとえば、次のコマンドを使用して 16 コアの仮想マシンを作成します。

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM \
  --image Debian --admin-username aldis --generate-ssh-keys --verbose \
  --size "Standard_DS5_v2"
```

> [!WARNING]
> ご利用のサブスクリプション レベルに応じて、作成できるリソース数に[制限が課され](https://docs.microsoft.com/azure/azure-subscription-service-limits)、さらにそのリソースの合計サイズも制限されます。 たとえば、従量課金制サブスクリプションでの仮想 CPU 数の制限は **20** で、無料レベルの場合は **4** となっています。 Azure CLI には、これを超えたときに**クォータ超過**エラーで通知する機能があります。 アーキテクチャを作成するときにこのエラーが発生した場合は、有料サブスクリプションに関連付けられている制限の引き上げ (仮想 CPU の上限は 10,000 個) を申請することができます。申請は[オンラインで行います (無料)](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors)。 

## <a name="resizing-an-existing-vm"></a>既存の VM のサイズを変更する
ワークロードが変化した場合や、作成時に正しくサイズが設定されなかった場合にも、既存の VM のサイズを変更することができます。 サイズ変更を要求する前に、VM が属するクラスター内で目的のサイズが使用できるかどうかを確認する必要があります。 これを行うには、`vm list-vm-resize-options` コマンドを使用します。

```azurecli
az vm list-vm-resize-options --resource-group ExerciseResources --name SampleVM --output table
```

これにより、リソース グループ内で使用可能なすべてのサイズ構成のリストが返されます。 目的のサイズがクラスターでは使用できないものの、リージョンでは使用_できる_場合は、[VM を割り当て解除](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate)することができます。 このコマンドは、実行中の VM を停止し、リソースを失うことなく、現在のクラスターからその VM を削除します。 その後、VM のサイズを変更できます。つまり、目的のサイズ構成が使用できる新しいクラスター内に VM を再作成します。

VM のサイズを変更するには、`vm resize` コマンドを使用します。 たとえば、現在の VM リソースを、必要最小限の 2 G のメモリ、1 CPU コア、4 G のディスク領域に削減してみましょう。 Cloud Shell に次のコマンドを入力します。

```azurecli
az vm resize --resource-group ExerciseResources --name SampleVM --size "Standard_B1ms"
```

このコマンドで VM のリソースを削減するには数分かかります。完了すると、新しい JSON 構成が返されます。