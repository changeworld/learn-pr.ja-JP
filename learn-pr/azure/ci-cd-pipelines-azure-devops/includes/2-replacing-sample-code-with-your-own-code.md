Azure DevOps プロジェクトを作成すると、最初の要求のすべてのユーザーは、サンプル アプリを独自のアプリでする置換方法でしょうか。 非常に簡単と、このユニットでは、これを行う 2 つの方法を学習します。

1. VSTS git リポジトリにコードを実際のコードに置き換えます

2. ビルド パイプラインを実際のコードを保持する、外部の git リポジトリを指す

## <a name="replacing-code-in-vsts-git-repository"></a>VSTS git リポジトリ内のコードを置き換える

1 つの簡単な方法は、すべてアップロード VSTS に戻ると、独自のコードに置き換えて、ハード ドライブ上に vsts git リポジトリを複製することです。 コードのビルドし、CI/CD パイプラインを使用してデプロイされたようになりましたが。

このユニットは、ダウンロードして、ハード ドライブ上に node.js アプリのソース コードを格納する始めます。

1. ソース コードをダウンロードします。 <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. ハード ドライブのどこかに MicrosoftLearnDevOps.zip の内容を抽出します。 この例の`C:\users\abel\Downloads\MicrosoftLearnDevOps`が使用されました  
![解凍したディレクトリ](../media-drafts/2-unzippedfolder.png)

次に、ハード ドライブ上にリポジトリを複製し、実際の node.js アプリでサンプル アプリを置き換える必要があります。 このユニットは、コンピューターにインストールされている git が既にあると仮定します。

1. Azure portal からは、Azure DevOps プロジェクトを参照し、コード リポジトリのリンクをクリックします。  
![](../media-drafts/2-browsetorepolink.png)

2. [複製] をクリックし、右上に git リポジトリの url をコピーします。  
![リポジトリを複製](../media-drafts/2-clonerepo2.png)このリポジトリの url をクリップボードにコピーされます

3. ハード ドライブにリポジトリを複製します。  
![Git クローン](../media-drafts/2-gitclone.png)  
この例では、リポジトリが C:\Users\abel\Source\TripleCrown\DevOps に複製されました

4. すべてのもの以外のローカル リポジトリから削除、`.git`ディレクトリ  
![すべてのリポジトリを削除します。](..//media-drafts/2-deleterepoofeverything.png)

5. リポジトリ フォルダーにダウンロードした node.js アプリのソース コードをコピーします。  
![置き換えられたコード](../media-drafts/2-replacedeverything.png)

6. 」と入力して、ローカル リポジトリにすべての変更が加え`git add *`コマンド ラインから  
![Git は、すべてを追加します。](../media-drafts/2-gitaddall.png)

7. 」と入力して、ローカル リポジトリに変更をコミットします。 `git commit -m "replace sample app with real code"`  
![Git コミット](../media-drafts/2-gitcommit.png)

8. 変更を使用した VSTS の git リポジトリにプッシュします。 `git push`  
![Git プッシュ](../media-drafts/2-gitpush.png)

9. 変更を VSTS に戻すには後、ビルドからの実際のアプリ コードを送信このする必要があります。  
![ビルド開始オフ](../media-drafts/2-buildkickedoff2.png)
![ビルド実行](../media-drafts/2-buildrunning2.png)と azure に至るリリース パイプライン  
 ![実行しているリリース](../media-drafts/2-releaserunning2.png)

 デプロイが完了すると、Azure portal に戻って実際のアプリが展開されたことを確認できます。

 1. 移動する Azure portal の Azure DevOps プロジェクトを参照し、右側にあるデプロイされているアプリをクリックします  
 ![サンプル アプリのリンクを起動します。](../media-drafts/2-launchapp.png)

 2. これにより、ブラウザーで実行中のアプリが起動します。  
 ![実行中のアプリ](../media-drafts/2-apprunning.png)

## <a name="using-external-git-repo"></a>外部の git リポジトリの使用

別の方法を実際のアプリ コードを使ってサンプル アプリを入れ替えるビルド パイプラインをアプリのコードを保持する外部の git リポジトリを指すことです。 この例では、実際のアプリのコードを github リポジトリにアップロードします。

実際のコードを github にアップロードした後、ビルド パイプラインを指すこちらの github リポジトリには、次の操作を行います

1. Azure portal から Azure DevOps プロジェクトを参照し、ビルド リンクをクリックします  
![リンクを構築します。](../media-drafts/2-buildlink.png)

2. ビルド パイプラインに移動、ビルド パイプラインをクリックし、 `Edit`  
![エディット ビルド パイプラインをクリックします。](../media-drafts/2-editbuildpipelinelink.png)

3. ビルド エディターに移動をクリックします `Get sources`  
![取得元のタスクをクリックします。](../media-drafts/2-clickgetsourcetask.png)

4. すると、選択するソース ページ。 方法はしか使用できません VSTS の Git も GitHub、GitHub Enterprise、Subversion、Bitbucket Cloud、および任意の外部 Git ベースのビルド パイプラインのリポジトリ。 この演習では、GitHub を選択します。  
![GitHub を選択します。](../media-drafts/2-selectgithub2.png)

5. GitHub の [接続] ページに移動します。 OAuth または個人用アクセス トークンを使用するか、GitHub アカウントに接続するには

6. 実際のアプリのコードを保持している github リポジトリを選択し、をクリックしてください `Save & queue`  
![保存してキュー](../media-drafts/2-saveandqueue2.png)

7. をクリックして、`Save & queue`ボタン  
![保存して、ボタンのキュー](../media-drafts/2-saveandqueuebutton.png)

8. このアクションは、保存とオフは、実際のアプリを送信する、ビルドが開始され、ビルド経由でホストされている GitHub のコードしリリース パイプラインを Azure に  
![ビルドを実行中](../media-drafts/2-buildpipelinerunning.png)

## <a name="summary"></a>まとめ

このユニットでは、DevOps プロジェクトのサンプル コードを実際のアプリ コードに置き換えますに 2 つの方法について説明しました。 これは、VSTS git リポジトリでコードを置き換えることで、または、アプリのコードを保持する別の外部リポジトリとビルド パイプラインをリンクすることで実行できます。

次に、ビルドのカスタマイズし、リリース パイプラインの方法について説明します。