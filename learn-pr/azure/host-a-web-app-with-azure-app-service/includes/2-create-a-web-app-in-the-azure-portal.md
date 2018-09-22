ここでは、Azure portal を使用して Azure App Service で Web アプリを作成する方法について説明します。

## <a name="why-use-the-azure-portal"></a>Azure portal を使用する理由

Web アプリケーションをホストするには、まず、ご利用の Azure サブスクリプション内で Web アプリ (App Service アプリ) を作成します。

Web アプリを作成するには、いくつかの方法があります。 Azure portal、Azure CLI、スクリプト、または IDE を使用することができます。

ここでは、ポータルを使用します。ポータルはグラフィカルなエクスペリエンスであるため、最適な学習ツールになります。 このポータルは、使用可能な機能の検出、リソースの追加、既存のリソースのカスタマイズを行うのに役立ちます。

## <a name="what-is-azure-app-service"></a>Azure App Service とは

Azure App Service は、Azure 環境内で完全に管理されたコンピューティング プラットフォームで、この Azure 環境は Web アプリ、REST API、およびモバイルのバック エンドをホストするために最適化されています。

Microsoft Azure によって提供される、このサービスとしてのプラットフォーム (PaaS) を使用すると、ご利用のアプリケーションの実行とスケーリングのためのインフラストラクチャは Azure で処理されるため、お客様はビルド側の作業に集中できます。

## <a name="how-to-create-a-web-app"></a>Web アプリを作成する方法

独自のアプリをホストするときは、Azure portal にアクセスし、**Web アプリ**を作成します。 Azure portal で **Web アプリ**を作成すると、実際には App Service で一連のホスティング リソースを作成しています。これは、ASP.NET Core、Node.js、PHP などのいずれであっても、Azure によってサポートされる任意の Web ベースのアプリケーションをホストするために使用できます。次の図は、アプリによって使用されるフレームワーク/言語を簡単に構成する方法を示しています。

![Web アプリの設定](../media/2-web-app-settings.png)

Azure portal では、Web アプリを作成するためのテンプレートが提供されています。 このテンプレートには、次のフィールドが必要です。

- **名前**: Web アプリの名前。
- **サブスクリプション**: 有効でアクティブなサブスクリプション。
- **リソース グループ**: 有効なリソース グループ。 以下のセクションでは、リソース グループの内容を詳細に説明します。
- **OS**: オペレーティング システム。 オプションとして、Windows、Linux、および Docker コンテナーを使用できます。 Windows では、さまざまなテクノロジからあらゆる種類のアプリケーションをホストできます。 同じことが Linux ホスティングにも当てはまりますが、Linux では、ASP.NET アプリが .NET Core Framework 上の ASP.NET Core である必要があります。 最後のオプションは Docker コンテナーです。この場合、Azure によってホストおよび保守されるコンテナー経由で直接コンテナーをデプロイできます。 
- **App Service プラン/場所**: 有効な Azure App Service プラン。 以下のセクションでは、App Service プランの内容を詳細に説明します。
- **Applications Insights**: Azure Application Insights オプションを有効にすると、Azure portal が提供する監視とメトリック ツールを利用して、ご自分のアプリのパフォーマンスを監視できます。

Azure portal では、多くの使用可能なツールからご自分のアプリを優位に管理、監視、および制御できます。

### <a name="deployment-slots"></a>デプロイ スロット

Azure portal を使用すると、簡単に**デプロイ スロット**を App Service Web アプリに追加できます。 たとえば、ご自分のコードをプッシュできる**ステージング**のデプロイ スロットを作成して、Azure 上でテストできます。 適切なコードになったら、ステージングのデプロイ スロットを運用スロットに簡単に**切り替える**ことができます。 Azure portal で単純なマウス クリックを数回行うだけで、すべての操作を行うことができます。

![デプロイ スロット](../media/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a>継続的インテグレーション/デプロイのサポート

Azure portal では、Visual Studio Team Services、GitHub、Bitbucket、Dropbox、OneDrive、または開発マシン上のローカルの Git リポジトリで、そのままで使用できる継続的インテグレーションおよびデプロイが提供されます。 上のソースのいずれかを使ってご自分の Web アプリに接続すると、App Service でコードとそのコードでの今後の変更が Web アプリに自動同期され、自動的に残りの作業が行われます。 さらに、Visual Studio Team Services では、独自のビルドとリリース プロセスを定義できます。これにより、コードをコミットするたびに、ご自分のソース コードのコンパイル、テストの実行、リリースのビルドが行われ、最後に Web アプリにリリースがプッシュされます。 ユーザーが介入する必要はなく、すべて暗黙的に行われます。

![継続的インテグレーションを構成する](../media/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a>Visual Studio の発行と FTP の発行の統合

ご自分の Web アプリ用に継続的インテグレーション/デプロイを設定できるのに加え、常に、Visual Studio との緊密な統合からの恩恵を受け、Web 配置テクノロジによってご自分の Web アプリを Azure に発行することができます。 また、Azure では FTP もサポートされています。ただし、Web 配置で変更または追加されたそれらのファイルのみを選び、単純にすべてを Azure に公開しないようにするための機能がないため、公開に FTP を使用することはお勧めしません。

### <a name="built-in-auto-scale-support-automatic-scale-out-based-on-real-world-load"></a>組み込みの自動スケールのサポート (実際の負荷を基に自動的にスケール アウト)

組み込み済みの Web アプリの機能は、スケーリングまたはスケール アウトです。Web アプリの使用量に応じて、ご自分の Web アプリをホストしている基になるマシンのリソースの増加または減少により、ご自分のアプリをスケーリングできます。 リソースは、コアの数または利用可能な RAM の量になります。

一方、スケール アウトは、ご自分の Web アプリを実行しているマシン インスタンスの数を増やす機能です。

## <a name="what-is-a-resource-group"></a>リソース グループとは

リソース グループは、指定したアプリケーションと環境に対して、仮想マシン、Web アプリ、データベースなどの相互に依存するリソースとサービスをグループ化するメソッドです。 リソース グループを、ご自分のアプリの要素をグループ化する場所である**フォルダー**と考えます。

リソース グループを使用すると、簡単にリソースを管理および削除できます。 また、アプリケーションの実行に必要であったり、クライアントによって使用されたりするリソースのコレクションへの請求の監視、アクセスの制御、プロビジョニング、および管理を行う方法も提供します。

## <a name="what-is-an-app-service-plan"></a>App Service プランとは

App Service プランは、ご自分の App Service アプリをデプロイできる物理リソースと容量のセットです。

Azure portal では、新しい App Service プランを作成するためにテンプレートを提供します。 このテンプレートには、次の基本情報が必要です。

- リージョン (米国西部、米国中部、北ヨーロッパなど)
- スケール カウント (インスタンス数 1、2、3 など)
- インスタンス サイズ (S、M、L)
- SKU、または価格レベル (Free、Shared、Basic、Standard、Premium、PremiumV2、および Isolated)

Azure App Service でホストされている Web アプリ、モバイル アプリ、API アプリのほか、Azure Functions は、すべて App Service プラン内で実行されます。 アプリケーションを無制限に App Service プランにデプロイできますが、使用する数は、デプロイされているアプリケーションの種類や CPU 使用率で必要なリソースによって大幅に異なります。

スケーリングするか、アプリケーションを別の App Service プランに移動するかの必要性を判断できるように、常に、Azure portal でご自分の App Service プランを使用し、ご自分の CPU とメモリの使用率を視覚化することができます。