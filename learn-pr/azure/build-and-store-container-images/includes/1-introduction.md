Azure Container Registry は、オープンソースの Docker Registry 2.0 に基づくマネージド Docker レジストリ サービスです。 Container Registry は Azure でプライベートにホストされており、あらゆる種類のコンテナー展開用にイメージをビルド、格納、管理できます。

Container Registry では、Docker CLI または Azure CLI を使用して、コンテナー イメージをプッシュおよびプルできます。 Azure portal の統合を使用して、コンテナー レジストリ内のコンテナー イメージを視覚的に検査できます。 分散環境では、Container Registry の geo レプリケーション機能を使用して、ローカライズされた配布用に複数の Azure データ センターにコンテナー イメージを配布できます。

コンテナー イメージを格納するだけでなく、Azure Container Registry Build では Azure にコンテナー イメージを構築できます。 Build では、標準的な Dockerfile を使用してコンテナー イメージが作成され、Azure Container Registry に格納されます。ローカルな Docker ツールは必要ありません。 Azure Container Registry Build では、DevOps のプロセスとツールを使用して、必要に応じてコンテナー イメージをビルドしたり、コンテナー イメージのビルドを完全に自動化したりできます。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。

- Azure コンテナー レジストリを展開する
- Azure Container Registry Build を使用してコンテナー イメージをビルドする
- このコンテナーを Azure コンテナー インスタンスに展開する
- コンテナー イメージを複数の Azure データ センターにレプリケートする