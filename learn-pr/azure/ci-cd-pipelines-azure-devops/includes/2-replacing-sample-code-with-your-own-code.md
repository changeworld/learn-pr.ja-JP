Azure DevOps プロジェクトを作成すると、誰もが最初に尋ねることは､どのようにしてサンプル アプリを自分のアプリに置き換えるのかということです｡ これ非常に簡単であり､このユニットでは、そのための 2 つの方法を学びます｡

1. VSTS git リポジトリ内のコードの､実際のコードへの置き換え

2. 実際のコードを保持している外部 git リポジトリへビルド パイプラインをポイントする

## <a name="replacing-code-in-vsts-git-repository"></a>VSTS git リポジトリ内のコードの置き換え

1 つの簡単な方法は、VSTS 内の git レポジトリのクローンを自分のハードドライブに作成して､あらゆるものを独自のコードに置き換え、VSTS および voila に戻すことです｡ これでコードがビルドされ、CI/CD パイプライン経由でデプロイされます｡

このユニットでは、最初にソースコードをハード ドライブにあるnode.js アプリにダウンロードして保存します｡

1. <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip> からソース コードをダウンロードします｡

2. MicrosoftLearnDevOps.zip の内容をハード ドライブの適当な場所に抽出します。 この例では､`C:\users\abel\Downloads\MicrosoftLearnDevOps` を使用しています｡  
![ディレクトリの解凍](/media-draft/2-unzippedfolder.png)

次に、ハード ドライブにリポジトリのクローンを作成し､サンプル アプリを実際の node.js アプリに置き換える必要があります。 このユニットでは、コンピューターにすでに git がインストールされているものと仮定します｡

1. Azure portal から、Azure DevOps プロジェクトを参照し、コード リポジトリのリンクをクリックします。  
![](/media-draft/2-browsetorepolink.png)

2. [Clone] をクリックして、右上にある git リポジトリの URL をコピーします。  
![クローン用 URL のコピー](/media-draft/2-copycloneurl.png)  
これで、リポジトリの URL がクリップボードにコピーされます。

3. ハード ドライブにリポジトリのクローンを作成します。  
![Git Clone の作成](/media-draft/2-gitclone.png)  
この例では、C:\Users\abel\Source\TripleCrown\DevOps にレポジトリのクローンを作成しています｡

4. ローカルのレポジトリから､`.git` ディレクトリ以外のすべてを削除します｡  
![レポジトリから削除](/media-draft/2-deleterepoofeverything.png)

5. ダウンロードした node.js アプリのソース コードをリポジトリ フォルダーにコピーします。  
![コードの置き換え](/media-draft/2-replacedeverything.png)

6. コマンド ラインから `git add *` を入力して、ローカル リポジトリにすべての変更を追加します｡  
![Git すべて追加](/media-draft/2-gitaddall.png)

7. `git commit -m "replace sample app with real code"` と入力して、ローカル リポジトリに変更をコミットします。  
![Git コミット](/media-draft/2-gitcommit.png)

8. `git push`を使用して、変更内容を VSTS の git リポジトリにプッシュします。  
![Git プッシュ](/media-draft/2-gitpush.png)

9. 変更を VSTS に戻すと、ビルドから実際のアプリ コードが送信されます｡  
![ビルドの開始](/media-draft/2-buildkickedoff.png)  
Azure までの ![Build in Action (ビルドの進捗状況)](/media-draft/2-buildinaction.png) とリリースパイプライン  
 ![リリース中](/media-draft/2-releaserunning.png)

 デプロイが完了すると、Azure portal に戻ることで実際のアプリが展開されたことを確認することができます。

 1. Azure portal に移動して､ Azure DevOps プロジェクトを参照し、右側にあるデプロイされたアプリをクリックします  
 ![サンプルのアプリ リンクを起動する](/media-draft/2-launchapp.png)

 2. これにより、ブラウザーでアプリが起動します。  
 ![App Running](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a>外部 git リポジトリの利用

サンプルコードを実際のアプリ コードに置き換えるもう 1 つの方法は､アプリのコードを保持している外部 git リポジトリへビルド パイプラインをポイントすることです｡ この例では、実際のアプリのコードを github リポジトリにアップロードします。

実際のコードを github にアップロードしたら､次の操作を行って､その github リポジトリへビルド パイプラインをポイントします｡

1. Azure portal から Azure DevOps プロジェクトを参照し、ビルド リンクをクリックします  
![Build Link](/media-draft/2-buildlink.png)

2. ビルド パイプラインに移動したら､ビルド パイプラインをクリックし、 `Edit` をクリックします｡  
![[Edit Build] をクリックします。](/media-draft/2-clickeditbuildlink.png)

3. ビルド エディターに移動したら､`Get sources` をクリックします｡  
![[Get Source] をクリックします。](/media-draft/2-clickgetsource.png)

4. Select a source page に移動します｡ ビルド パイプラインでは､VSTS の Git ばかりでなく､ GitHub や GitHub Enterprise、Subversion、Bitbucket Cloud､さらには外部 git に基づくあらゆるレポジトリを利用できることに注意してください｡ この課題では、GitHub を選択します。  
![[GitHub] を選択します。](/media-draft/2-selectgithub.png)

5. GitHub の接続ページに移動します。 OAuth または Personal Access Token のいずれかを使用して、自分の GitHub アカウントに接続します｡

6. 実際のアプリのコードを保持している github リポジトリを選択して、`Save & queue` をクリックします｡  
![[保存] と [Queue (キュー)]](/media-draft/2-saveandqueue.png)

7. `Save & queue` ボタンをクリックします。  
![](/media-draft/2-saveandqueuedialog.png)

8. この操作によりビルドが保存されて､開始されます｡GitHub にホストされていた実際のアプリ コードがのビルド パイプライン､リリース パイプラインを経由して Azure に送信されます｡  
![ビルド中](/media-draft/2-buildrunning.png)

## <a name="summary"></a>まとめ

このユニットでは、DevOps プロジェクトにあるサンプル コードを実際のアプリ コードに置き換える 2 つの方法を学びました｡ この置き換えは、VSTS git リポジトリにあるコードを置き換えることでも､あるいはアプリのコードを保持している別の外部レポジトリにビルド パイプラインをリンクさせることによっても行うことができます｡

この後は、ビルドおよびリリース パイプラインのカスタマイズ方法を学びます｡