迅速に実行、CI/CD パイプラインの設定は常に困難されています。 ここで、完全なエンド ツー エンドの Azure DevOps プロジェクトをまったく何もから移動するのには驚くほど簡単です。 Azure DevOps プロジェクトで、次の取得します。

1. インフラストラクチャを Azure でプロビジョニングします。

2. VSTS のインスタンス内のチーム プロジェクト。

3. VSTS インスタンスのリポジトリで選択した言語でサンプル アプリのソース コードです。

4. ビルドとリリース パイプラインの意味のあるテクノロジの選択。

そのが終了したら、Azure DevOps プロジェクトのサンプル アプリは、ビルド、および、パイプラインを介してプロビジョニングするので、Azure インフラストラクチャにそれを解放します。 またこの数回のクリックだけですべてを実現できます。

## <a name="create-an-azure-devops-project"></a>Azure DevOps プロジェクトを作成する

Azure DevOps プロジェクトを作成するには、Azure portal から。

1. ヘッド、 [Azure Portal](https://portal.azure.com)  をクリック `Create a resource`  
![AzurePortal](../media-drafts/1-azureportal.png)

2. [`DevOps Project`] をクリックします。  
![DevOps プロジェクトを選択します。](../media-drafts/1-pickdevopsproject.png)

3. 次の画面は、使用する言語を選択できます。 .NET (当然) を選択する方法、java、ノード、php、python、ruby、および移動に注意してください。 Git リポジトリからも、独自のコードを表示できます。 このユニットの Node.js アプリを作成してみましょう。 Node.js をクリックし、[次へ] をクリックします。  
![Node.js の言語を選択します。](../media-drafts/1-picknodejsforlang.png)

4. 次に使用する node.js フレームワークを依頼する予定です。 このユニットのシンプルな Node.js アプリを選択し、[次へ] をクリックします。  
![単純なノードを選択します](../media-drafts/1-picksimplenode.png)

5. 次に、どのようなインフラストラクチャをプロビジョニングしするようにアプリの実行を依頼することはでしょうか。 このユニットをプロビジョニングして、次の Azure Kubernetes サービスを使用して、Kubernetes クラスターにデプロイしてみましょう。 Kubernetes サービスを選択し、[次へ] をクリックします。  
![Kubernetes を選択します。](../media-drafts/1-pickkubernetes.png)

6. 次に、VSTS の新しいインスタンスを作成か、既存のものを選択します。 取得することも実行する kubernetes クラスターが必要な場所と方法を設定します。 このユニットの新規の選択を選択して、VSTS の新しいインスタンスを作成して、VSTS のインスタンスに一意の名前します。 プロジェクト名の情報を入力します。、azure サブスクリプションを選択、名前をクラスター名 LearnCluster の場所、およびクリックして、米国東部を選択します  
![](../media-drafts/1-finalconfirmation2.png)

およびそれでは文字どおり! これは、時間がかかりますので、戻って、緩和され、そのことを Azure に任せるだけです。 ほとんどの場合は、プロビジョニングと、Azure インフラストラクチャの構成に費やされます。 このモジュールには、これに、Azure Kubernetes Service と Azure Container Registry になります。

## <a name="a-lap-around-the-finished-azure-devops-project"></a>簡単な完成した Azure DevOps プロジェクト

Azure を完了すると、アラートで通知されます。

1. [アラート] をクリックし、リソースに移動し、  
![アラートからリソースに移動します。](../media-drafts/1-gotoresourcefromalert.png)

2. プロビジョニングされたすべてのものを表示するポータル ブレードに移動します。 左側にある、CI/CD パイプラインです。 コード リポジトリ、ビルド定義、およびリリース定義があります。 すべてのリンクは、VSTS でのリソースに直接移動するディープ リンクです。 する、右側にあるすべてのインフラストラクチャを Azure でデプロイを参照してください。 既にデプロイされているサンプル アプリでの Kubernetes クラスターともアプリケーションの分析情報があります。 もう一度、これらすべてのリンクは、Azure 内のリソースへのディープ リンクです。  
![ポータルの DevOps プロジェクト](../media-drafts/1-portaldevopsproj.png)

3. ソース コードは、リンクをクリックします  
![ソースへのリンク](../media-drafts/1-linktosource.png)

4. Azure のコードでの git リポジトリに移動します。 これは、helm チャートを使ってサンプル node.js アプリを保持している通常の git リポジトリに注意してください。  
![Azure リポジトリ](../media-drafts/1-azurerepo.png)

5. ビルドに移動します。  
![ビルドへのリンクします。](../media-drafts/1-linktobuild.png)

6. ビルドをクリックして作成されたビルドを編集し、[編集] をクリックしてください  
![ビルドのリンクを編集します。](../media-drafts/1-editbuildlink2.png)

7. ピッキング テクノロジにとって合理的なビルド パイプラインが表示されます。 Kubernetes クラスターに node.js アプリを選択しましたので、Docker タスクを使用して Node.js アプリをビルド、イメージのコンテナー イメージを作成し、Helm Helm パッケージを作成するタスクをパイプラインのビルドを取得します。  
![パイプラインを作成します。](../media-drafts/1-buildpipeline2.png)

8. クリックしてリリース パイプラインに移動します。 `Releases`  
![リリースのリンク](../media-drafts/1-gotoreleaselink.png)

9. 作成されたリリース パイプラインが表示されます。 リリースをクリックし、選択の編集します。 `Edit pipeline`  
![リリースを編集します。](../media-drafts/1-editrelease2.png)

10. Azure では、選択テクノロジにとって合理的なリリース パイプラインを作成しました。 Kubernetes クラスターでホストされているコンテナーで実行されているノードのアプリ。
![リリース パイプライン](../media-drafts/1-releasepipeline2.png)  
![リリース パイプラインのステップ](../media-drafts/1-pipelinesteps.png)

11. Azure portal に戻り、kubernetes サービスの外部エンドポイントをクリックします  
![](../media-drafts/1-clickonendpoint.png)

12. サンプル アプリをビルドする必要がありますを参照してくださいし、AKS クラスターにデプロイします。  
![実行中のアプリ](../media-drafts/1-apprunning.png)

## <a name="summary"></a>まとめ

このユニット内で構成される Azure DevOps プロジェクトの作成。

1. Azure Kubernetes サービスと Application Insight - アプリのインフラストラクチャ

2. VSTS のインスタンス内のチーム プロジェクト。

3. VSTS インスタンスのリポジトリ内のコンテナーで実行される Node.js のサンプル アプリのソース コードです。

4. Azure Kubernetes サービス インスタンスで実行されている Node.js コンテナー アプリのビルドとリリース パイプライン。

次に、サンプル アプリを実際のアプリに置き換える方法について説明します。