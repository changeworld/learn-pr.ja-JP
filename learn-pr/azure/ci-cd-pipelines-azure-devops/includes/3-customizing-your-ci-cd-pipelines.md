エクスペリエンスでは、Azure DevOps プロジェクトの出力がビルドを作成し、リリース パイプラインをうえで適したテクノロジを選択します。 このモジュールには、Kubernetes クラスターでホストされているコンテナーで実行されている node.js アプリにとって合理的なビルドとリリース パイプラインを作成しました。 

プロジェクトの特定の作業を行うビルドとリリース パイプラインをカスタマイズする多くの場合に必要があります。 VSTS のビルドとリリース パイプラインは、カスタマイズ可能な 100% です。 必要なことを行うには、パイプラインを行うことができます。

このユニットでは、ビルドのカスタマイズやリリース パイプラインの方法を説明します。

## <a name="customize-the-build-pipeline"></a>パイプラインのビルドをカスタマイズします。

VSTS でビルド エンジンは、のみ、タスク ランナー、もう 1 つのタスクを実行します。 ビルドをカスタマイズするには、だけ追加またはタスクを削除し、タスクの正しいパラメーターを入力します。

すぐに使える VSTS に使用できる約 100 個のタスクが付属します。 すぐに存在しないことを行うため必要がある場合は、marketplace を確認してください。 700 を超えるがビルドおよびリリースのダウンロードおよび使用の準備完了のタスクがないです。 また、独自のカスタム タスクを記述する機能があります。

このユニットの marketplace タスク コードのセキュリティのスキャンを行う WhiteSource Bolt をインストールすることで、パイプラインのビルドをカスタマイズします。

1. Azure portal で Azure DevOps プロジェクトを参照し、ビルド定義のリンクをクリックします  
![リンクを構築します。](../media-drafts/3-buildlink.png)

2. ビルド パイプラインのページに移動します。 ビルド を選択します `Edit`  
![ビルドを編集します。](../media-drafts/3-editbuild2.png)

3. ビルド定義に移動します。 をクリックして、`+`エージェント ジョブの 1 にタスクを追加するには  
![タスクを追加します。](../media-drafts/3-addtask2.png)

4. テキスト フィールドに「 `bolt`  をクリック `Get it free`  
![白いソース ボルトを取得します。](../media-drafts/3-getwhitesourcebolt.png)

5. WhiteSource Bolt の Marketplace ページに移動します。 [`Get it free`] をクリックします。  
![WhiteSource Bolt の無料の取得します。](../media-drafts/3-getwhitesourceboltfree2.png)

6. Azure DevOps 組織を選択し、順にクリックします `Install`  
![インストール](../media-drafts/3-installwsb.png)

7. ここで説明された手順に従って、WhiteSource Bolt をアクティブ化します。 <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. DevOps プロジェクトが読み込まれ、ビルド パイプライン リンクをクリックして、Azure portal に戻る  
![パイプライン リンクを構築します。](../media-drafts/3-buildpipelinelink.png)

9. ビルドを選択し、をクリックしてください `Edit`  
![ビルドを編集します。](../media-drafts/3-editbuild2.png)

10. をクリックして、`+`エージェント ジョブにタスクを追加、入力`bolt`検索フィールドをクリックします `Add`  
![ボルトを追加します。](../media-drafts/3-addwsbolt.png)

11. WhiteSource Bolt タスク、タスク一覧の下部に追加されます、一番上にドラッグします  
![ボルトをページのトップへ移動します。](../media-drafts/3-moveboltototopoftasklist.png)

12. クリック`Save & queue`し選択します `Save & queue`  
![保存してキュー](../media-drafts/3-saveandqueue2.png)

13. [`Save & queue`] をクリックします。  
![保存して、ボタンのキュー](../media-drafts/3-saveandqueuebutton.png)

これにより、変更済みのビルド パイプラインを保存し、ビルドをキューします。 ビルドの終了後は、ビルドを見て`WhiteSource Bolt Build Report`セキュリティの脆弱性を探して WhiteSource Bolt によってスキャンされたソース コードを参照してください。

![白のソース レポート](../media-drafts/3-whitesourcereport.png)

## <a name="customize-the-release-pipeline"></a>リリース パイプラインをカスタマイズします。

リリース パイプラインは、ビルドのようにタスク ランナーは、しと同じ方法をカスタマイズできます。 このユニットでは、リリースの最後に、web パフォーマンス テストを追加します。 これにより、アプリが展開されていることを確認して、Kubernetes クラスターに正常に実行します。

1. Azure Portal で DevOps プロジェクトを参照し、リリース パイプラインは、リンクをクリックします  
![リリースのリンク](../media-drafts/3-releaselink.png)

2. リリース パイプラインのページに移動します。 リリース パイプラインをクリックし、をクリックしてください `Edit`  
![リリースを編集します。](../media-drafts/3-editreleasebutton.png)

3. リリース内のタスクでクリックして`Dev`ステージ  
![リリース ステージ](../media-drafts/3-releasestage2.png)

4. をクリックして、`+`タスクの種類、フェーズ 1 を追加`web test` をクリックし、検索フィールドで`Add`クラウド ベースの Web パフォーマンス テスト タスク  
![Web テストを追加します。](../media-drafts/3-addwebtest2.png)

5. クイック Web パフォーマンス テストのタスクをクリックして、(url の確認、ポータル、Azure DevOps プロジェクト ページにし、右側にある、サンプル アプリ外部エンドポイントとコピーのリンクを右クリックして) を web サイトの URL とアプリへの url を追加し、テスト ビルダーの編集します。tName に入力`Ping Test` をクリック `Save`  
![URL のコピー](../media-drafts/3-copyurl.png)  
![Web テストを保存します。](../media-drafts/3-savewebtest.png)

ここで、新しい helm パッケージを展開した後、リリースを実行するときに web テストによっては、アプリの url を正常に達するを実行します。

![Web テストの実行](../media-drafts/3-webtestrun.png)

## <a name="summary"></a>まとめ

このユニットでは、ビルドのカスタマイズやリリース パイプラインの方法を学習しました。 インストールして、Marketplace からタスクを使用する方法も学習しました。