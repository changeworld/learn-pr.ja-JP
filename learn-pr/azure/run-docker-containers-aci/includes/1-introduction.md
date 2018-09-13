コンテナーは、クラウド アプリケーションのパッケージ化、デプロイ、管理を行う優れた方法としてしだいに普及しつつあります。 Azure Container Instances には、仮想マシンを管理したり、より高度なサービスを採用したりせずに、Azure で最も高速かつシンプルにコンテナーを実行する方法が用意されています。

Azure Container Instances は、シンプルなアプリケーション、タスク自動化、ビルド ジョブなど、分離されたコンテナーで操作できるあらゆるシナリオ向けの、優れたソリューションです。 複数のコンテナー間でのサービスの検出、自動スケーリング、調整されたアプリケーション アップグレードなど、完全なコンテナー オーケストレーションが必要なシナリオには、Azure Kubernetes Service (AKS) をお勧めします。

Azure Container Instances には次の利点があります。

- **高速起動**: Azure Container Instances では秒単位で Azure 内のコンテナーを開始することができます。
- **秒単位の課金**: コンテナーが実行されていた期間に対してのみ課金されます。
- **ハイパーバイザーレベルのセキュリティ**: Azure Container Instances を使用すると、アプリケーションは、VM 内であるかのように、コンテナー内で確実に分離されます。
- **カスタム サイズ**: Azure Container Instances で、CPU のコア数とメモリを厳密に指定することによって最適な稼働率を確保できます。
- **永続的ストレージ**: Azure Container Instances を使用して状態を取得および保持できるように、Microsoft では Azure Files 共有を直接マウントしています。
- **Linux と Windows**: Azure Container Instances では、同じ API で、Windows と Linux の両方のコンテナーをスケジュールできます。

## <a name="learning-objectives"></a>学習の目的  

このモジュールでは、次のことを行います。

- Azure Container Instances でコンテナーを実行します。
- コンテナーの再起動の動作を制御します。
- コンテナーで環境変数を使用します。
- Azure Container Instances にデータ ボリュームをアタッチします。
- Azure Container Instances のトラブルシューティングについて学習します。
