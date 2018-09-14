
次の表は、どのようなディスクは、マネージ コードとアンマネージ ディスクとして使用できます。

|ディスクの種類  |管理対象外  | 管理者常駐型  | ストレージ アカウントの管理対象外|
|---------|---------|---------|---------|
|[標準的な HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   はい      |  はい       |  一般的なストレージ アカウント       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   ×      |   はい      |  適用不可       |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    はい     |   はい      |     Premium storage アカウント    |

次に、Vm のディスクの種類を選択するときにすることのトレードオフの概要を示します。

|ディスクの種類  |相対 IOPS *  | 相対的なコスト  | シナリオ|
|---------|---------|---------|---------|
|[標準的な HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   最小      |  最小       |  一貫したパフォーマンスとコスト効果の高い開発とテストを必要とする低 IOPs ワークロードの使用       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   [中]|   [中]      |  一貫したパフォーマンスとコスト効果の高い開発とテストを必要とする低 IOPs ワークロードの使用 |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    最高     |   最高      |     運用環境と低待機時間と高いスループットが重要なパフォーマンスが重視されるワークロードの使用    |

* IOPS の 1 秒あたりの入力/出力操作です。