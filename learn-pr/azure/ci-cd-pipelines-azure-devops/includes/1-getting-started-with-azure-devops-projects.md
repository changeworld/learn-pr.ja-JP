これまで、CI/CD パイプラインの設定を迅速に行うことは常に簡単ではありませんでした。 今では、完全なエンド ツー エンドの Azure DevOps プロジェクトをまったく何もない状態から驚くほど簡単に設定できます。 Azure DevOps プロジェクトでは次のものを作成できます。

1. Azure でプロビジョニングされたインフラストラクチャ。

2. VSTS インスタンス内のチーム プロジェクト。

3. VSTS インスタンスのリポジトリで選択した言語でのサンプル アプリのソース コード。

4. 選択したテクノロジで意味のあるビルドとリリースのパイプライン。

また、完了すると、Azure DevOps プロジェクトはサンプル アプリを取得し、Azure でユーザー用にプロビジョニングされたインフラストラクチャに、パイプラインを介してビルドし、リリースします。 これらすべてをほんの数回クリックで実行できます。

## <a name="create-an-azure-devops-project"></a>Azure DevOps プロジェクトを作成する

Azure portal から Azure DevOps プロジェクトを作成します。

1. [Azure portal](https://portal.azure.com) に移動し、[`Create a resource`] をクリックします。  
![](/media-draft/1-azureportal.png)

2. [`DevOps Project`] をクリックします。  
![[DevOps プロジェクト] を選択する](/media-draft/1-pickdevopsproject.png)

3. 次の画面では、使用する言語を選択します。 .NET、Java、Node、PHP、Python、Ruby、Go を選択できます。 Git リポジトリから独自のコードを取り込むこともできます。 このユニットでは、Node.js アプリを作成します。 [Node.js] をクリックし、[次へ] をクリックします。  
![言語として Node.js を選択する](/media-draft/1-picknodejsforlang.png)

4. 次に、使用する Node.js フレームワークの選択を求められます。 このユニットでは [シンプルな Node.js アプリ] を選択し、[次へ] をクリックします。  
![シンプルな Node を選択する](/media-draft/1-picksimplenode.png)

5. 次に、プロビジョニングしてアプリを実行するインフラストラクチャの選択を求められます。 このユニットでは、Azure Kubernetes Service を使用して Kubernetes クラスターをプロビジョニングしてそこに展開します。 [Kubernetes サービス] を選択して [次へ] をクリックします  
![Kubernetes を選択する](/media-draft/1-pickkubernetes.png)

6. 次に、VSTS の新しいインスタンスを作成するか、既存のものを選択できます。 また、Kubernetes クラスターを実行する場所と方法を設定します。 このユニットでは、[Select new]\(新しい選択\) を選択して新しい VSTS インスタンスを作成し、VSTS インスタンスに一意の名前を付けます。 プロジェクト名に「Learn」と入力し、Azure サブスクリプションを選択し、クラスター名を LearnCluster にし、場所として米国東部を選択し、[完了] をクリックします  
![最終確認画面](/media-draft/1-finalconfirmation.png)

文字どおりの意味です。 Azure の処理が終わるまで待ちます。 ほとんどの時間は、Azure インフラストラクチャのプロビジョニングと構成に費やされます。 このモジュールでは、これは Azure Kubernetes Service と Azure Container Registry です。

## <a name="a-lap-around-the-finished-azure-devops-project"></a>完成した Azure DevOps プロジェクト

Azure の処理が終わると、アラートで通知されます

1. アラートをクリックし、[リソースに移動] をクリックします  
![アラートからリソースに移動する](/media-draft/1-gotoresourcefromalert.png)

2. プロビジョニングされたすべてのものが表示されているポータル ブレードに移動します。 左側は CI/CD パイプラインです。 コード リポジトリ、ビルド定義、リリース定義があります。 すべてのリンクは、VSTS のリソースに直接移動するディープ リンクです。 右側には、Azure に展開されたすべてのインフラストラクチャが表示されます。 サンプル アプリが既に展開された Kubernetes クラスターと Application Insight もあります。 これらすべてのリンクは、Azure 内のリソースへのディープ リンクです。  
![ポータル DevOps プロジェクト](/media-draft/1-pickdevopsproject.png)

3. ソース コードのリンクをクリックします  
![ソースへのリンク](/media-draft/1-linktosource.png)

4. VSTS プロジェクトの Git リポジトリに移動します。 これは、helm グラフを含むサンプル Node.js アプリを保持する通常の Git リポジトリであることに注意してください。  
![VSTS リポジトリ](/media-draft/1-vstsrepo.png)

5. ビルドに移動します  
![ビルドへのリンク](/media-draft/1-navtobuild.png)

6. ビルドをクリックしてから [編集] をクリックして作成されたビルドを編集します  
![](/media-draft/1-editbuildlink.png)

7. 選択したテクノロジで意味のあるビルド パイプラインが表示されます。 Kubernetes クラスターに Node.js アプリを作成したので、Docker タスクを使用して Node.js アプリを作成し、イメージ コンテナー イメージを作成し、Helm タスクを使用して Helm パッケージを作成するパイプラインが作成されます。  
![ビルド パイプライン](/media-draft/1-buildpipeline.png)

8. [`Releases`] をクリックしてリリース パイプラインに移動します。  
![リリースに移動する](/media-draft/1-gotoreleases.png)

9. 作成されたリリース パイプラインが表示されます。 リリースをクリックして [`Edit pipeline`] を選択してパイプラインを編集します。  
![リリースを編集する](/media-draft/1-editrelease.png)

10. Azure では選択したテクノロジで意味のあるリリース パイプラインが作成されています。 Kubernetes クラスターでホストされているコンテナーで実行されている Node アプリ。
![リリース パイプライン](/media-draft/1-releasepipeline.png)

11. Azure portal に戻り、Kubernetes サービスの外部エンドポイントをクリックします  
![](/media-draft/1-clickonendpoint.png)

12. ビルドされて AKS クラスターに展開されたサンプル アプリが表示されます  
![実行中のアプリ](/media-draft/1-apprunning.png)

## <a name="summary"></a>まとめ

このユニットでは、次のもので構成される Azure DevOps プロジェクトを作成しました。

1. アプリのインフラストラクチャ - Azure Kubernetes Service と Application Insight

2. VSTS インスタンス内のチーム プロジェクト。

3. VSTS インスタンスのリポジトリ内のコンテナーで実行される Node.js サンプル アプリのソース コード。

4. Azure Kubernetes Service インスタンスで実行される Node.js コンテナー アプリ用のビルドとリリース パイプライン。

次に、サンプル アプリを実際のアプリに置き換える方法について学習します。