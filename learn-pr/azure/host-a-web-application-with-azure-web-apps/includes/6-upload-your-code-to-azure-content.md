ここでは、ほとんどまたはまったくダウンタイムを発生させずに、コンテンツをステージングしてデプロイする方法を見てみましょう。

## <a name="what-is-a-deployment-slot"></a>デプロイ スロットとは

デプロイ スロットは、独自のコンテンツ、構成、さらには一意のホスト名を持つ**独立した** Web アプリです。 そのため、他の Web アプリと同様に機能します。

> Azure では、デプロイ スロットの使用に対して追加料金は発生しません。

各デプロイ スロットには、その一意の URL からアクセスできます。 たとえば、`BESTBIKE` という名前で**ステージング** デプロイ スロットを追加したとします。 アプリケーションとデプロイ スロットの URL は次のとおりです。

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a>デプロイ スロットが便利な理由

FTP や Web 配置、Git、または別の方法であれ、従来の方法で Web アプリをデプロイすると、次のような弱点がいくつかあります。

- デプロイが完了すると、アプリケーションの**コールド スタート**を引き起こす Web アプリが再起動する場合があります。 アプリケーションへの最初の要求が低速になります。

- 場合によっては、Web アプリの*不良*バージョンをデプロイしている可能性があり、クライアントをリリースする前に、それを (運用環境) でテストする必要があります。

ここで、デプロイ スロットが作用します。 **運用**デプロイ スロットにアクセスしているユーザーに影響を与えずに、**ステージング** デプロイ スロットの Web アプリに変更を加えて、変更をテストすることができます。 新しい機能を運用環境に移動する準備ができたら、ステージング スロットと運用スロットを**スワップ**するだけです。**ダウンタイムは発生しません**。

デプロイ スロットを使用するもう 1 つのメリットは、運用スロットにスワップする前に、ステージング スロットでアプリケーションを**ウォーム アップ**できることです。 **コールド スタート**の遅延と時間のかかる初期化コードを回避できます。

最後に、アプリケーションの新しいバージョンが期待どおりに動作しないことが判明したら、前のデプロイ スロットに**戻す**ことができます。

## <a name="what-is-automated-deployment"></a>自動デプロイとは

自動デプロイ、または継続的インテグレーションは、エンド ユーザーへの影響を最小限に抑えて、新機能とバグ修正を高速かつ反復的なパターンでプッシュ アウトするために使用されるプロセスです。

Azure では、複数のソースから直接、自動デプロイをサポートします。 次のオプションを使用できます。

- **Visual Studio Team Services (VSTS)**: VSTS にコードをプッシュし、クラウドでコードをビルドし、テストを実行し、コードからリリースを生成し、最後にコードを Azure Web アプリにプッシュできます。

- **GitHub**: Azure では、GitHub から直接、自動デプロイをサポートします。 自動デプロイのために GitHub リポジトリを Azure に接続すると、GitHub 上の運用ブランチにプッシュするすべての変更が自動的にデプロイされるようになります。

- **Bitbucket**: GitHub との類似性により、Bitbucket を使用して自動デプロイを構成できます。

- **OneDrive**: 自分の OneDrive アカウント上の任意のファイルを変更する時に、Azure が自動的にそれを検出して、その Web アプリ ファイルのすべての変更を取り消せるように、自分の OneDrive アカウントと Web アプリを接続することができます。 これは、静的な Web サイトに最適なオプションです。 Azure でのすべての変更が円滑かつ瞬時に反映されるローカル ファイル システムを扱う感覚が得られます。

- **Dropbox**: OneDrive と同様に、自分の Web アプリ ファイルを Dropbox アカウントでホストして、それらを Azure Web アプリに自動的にプッシュしてデプロイすることができます。

- **外部リポジトリ**: 外部の Git リポジトリを使用して、自動デプロイを構成することができます。

### <a name="non-automated-deployment-to-azure"></a>Azure への非自動デプロイ

手動でコードを Azure にプッシュするために使用できるいくつかのオプションがあります。

- **FTP/S**: FTP または FTPS は、コードを任意のホスティング環境にプッシュする従来の方法です。 現在は推奨されていませんが、引き続き利用することができます。

- **Web 配置/IDE**: Visual Studio IDE を使用して Azure に Web アプリを発行することができます。 Visual Studio の発行メカニズムでは、Web 配置テクノロジを利用してコードを Azure にプッシュすることができます。

- **Git**: Azure では、アプリケーション用に**ローカル Git リポジトリ**が維持されます。 単純にコードをそこに直接**コミット**できます。

> このモジュールでは、Git を使用して非自動デプロイを実行します。

## <a name="summary"></a>まとめ

Azure では、現在のワークフローにより適合するように、コードをアップロードする複数の方法が提供されています。 デプロイ スロットを使用して、ユーザーのダウンタイムを防ぐこともできます。