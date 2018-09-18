Azure DevOps プロジェクトにおけるすぐに使用できるエクスペリエンスでは、選択されたテクノロジに適したビルドとリリース パイプラインを作成します。 このモジュールでは、Kubernetes クラスターにホストされているコンテナー内で実行される Node.js アプリに適したビルドとリリース パイプラインを作成しました。 

自分のプロジェクト向けに特定の処理が行われるように、ビルドとリリース パイプラインをカスタマイズしなければならないことはよくあります。 VSTS でのビルドとリリース パイプラインは、100% カスタマイズ可能です。 パイプラインには、必要な任意の処理を行わせることができます。

このユニットでは、ビルドとリリース パイプラインをカスタマイズする方法について説明します。

## <a name="customize-the-build-pipeline"></a>ビルド パイプラインをカスタマイズする

VSTS のビルド エンジンは単なるタスク ランナーであり、タスクを 1 つずつ順に実行します。 ビルドをカスタマイズするには、タスクを追加または削除し、タスクに適切なパラメーターを指定します。

VSTS には、すぐに使用できる約 100 個のタスクが付属しています。 付属タスクにないことを行う必要がある場合は、マーケットプレースで探してみてください。ダウンロードして使用できるビルドおよびリリース タスクが 700 個以上あります。 また、独自のカスタム タスクを記述する機能もあります。

このユニットでは、コードのセキュリティ スキャンを行うために、マーケットプレース タスクの WhiteSource Bolt をインストールして、ビルド パイプラインをカスタマイズします。

1. Azure portal で Azure DevOps プロジェクトを参照し、ビルド定義のリンクをクリックします  
![ビルド リンク](/media-draft/3-buildlink.png)

2. これにより、ビルド パイプラインのページに移動します。 ビルドをクリックし、[`Edit`] を選択します  
![ビルドを編集する](/media-draft/3-editbuild.png)

3. これにより、ビルド定義に移動します。 [`+`] をクリックして、Agent Job 1 にタスクを追加します  
![タスクを追加する](/media-draft/3-addtask.png)

4. テキスト フィールドに「`bolt`」と入力し、[`Get it free`] をクリックします  
![無料で入手する](/media-draft/3-getitfree.png)

5. これにより、WhiteSource Bolt のマーケットプレース ページに移動します。 [`Get it free`] をクリックします  
![White Source Bolt を無料で入手する](/media-draft/3-getwhitesourceboltfree.png)

6. VSTS の組織を選択し、[`Install`] をクリックします  
![インストールする](/media-draft/3-install.png)

7. <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate> で説明されている手順に従って、WhiteSource Bolt のライセンス認証を行います

8. DevOps プロジェクトが読み込まれている Azure portal に戻り、ビルド パイプラインのリンクをクリックします  
![ビルド パイプライン リンク](/media-draft/3-buildpipelinelink.png)

9. ビルドを選択し、`Edit` をクリックします  
![ビルドを編集する](/media-draft/3-editbuild.png)

10. [`+`] をクリックして Agent job 1 にタスクを追加し、検索フィールドに「`bolt`」と入力して、[`Add`] をクリックします  
![Bolt を追加する](/media-draft/3-addbolt.png)

11. これにより、WhiteSource Bolt タスクがタスクの一覧の一番下に追加されます。これを一番上にドラッグします  
![Bolt を一番上に](/media-draft/3-boltattop.png)

12. [`Save & queue`] をクリックし、[`Save & queue`] を選択します  
![保存してキューに登録する](/media-draft/3-saveandqueue.png)

13. [`Save & queue`] をクリックします  
![[保存してキューに登録] ダイアログ](/media-draft/3-saveandqueuedialog.png)

これにより、変更したビルド パイプラインが保存され、ビルドがキューに登録されます。 ビルドが完了した後、ビルド `WhiteSource Bolt Build Report` を見ると、ソース コードが WhiteSource Bolt によってスキャンされ、セキュリティの脆弱性が探されていることがわかります。

![ビルド レポート](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a>リリース パイプラインをカスタマイズする

ビルドと同様に、リリース パイプラインもタスク ランナーであり、同じ方法でカスタマイズすることができます。 このユニットでは、リリースの最後に、Web パフォーマンス テストを追加します。 これにより、アプリが Kubernetes クラスターにデプロイされて正常に実行されていることが確認されます。

1. Azure portal で DevOps プロジェクトを参照し、リリース パイプラインのリンクをクリックします  
![リリース リンク](/media-draft/3-releaselink.png)

2. これにより、リリース パイプラインのページに移動します。 リリース パイプラインをクリックし、[`Edit`] をクリックします  
![リリース パイプラインを編集する](/media-draft/3-editreleasepipeline.png)

3. リリース `Dev` ステージのタスクをクリックします  
![リリース ステージ](/media-draft/3-releasestage.png)

4. `+` をクリックして Phase1 にタスクを追加し、検索フィールドに「`web test`」と入力して、Cloud-based Web Performance Test タスクの [`Add`] をクリックします  
![Web テストを追加する](/media-draft/3-addwebtest.png)

5. Quick Web Performance Test タスクをクリックし、[Web サイトの URL] にアプリへの URL を追加することで、タスクを編集します (URL を調べるには、Azure portal の DevOps プロジェクトのページに移動し、右側でサンプル アプリの外部エンドポイントを右クリックしてリンクをコピーします)。次に、TestName として、「`Ping Test`」と入力します  
![URL をコピーする](/media-draft/3-copyurl.png)  
![Web テスト タスクを編集する](/media-draft/3-editwebtesttask.png)

6. [`Save`] をクリックします  
![リリースを保存する](/media-draft/3-saverelease.png)

これで、リリースを実行すると、新しい helm パッケージのデプロイ後に Web テストが実行され、アプリの URL が正常に参照されます。

![Web テスト](/media-draft/3-webtest.png)


## <a name="summary"></a>まとめ

このユニットでは、ビルドとリリース パイプラインをカスタマイズする方法について説明しました。 また、マーケットプレースのタスクをインストールして使用する方法についても説明しました。