Azure DevOps プロジェクトを使用してカスタム アプリケーション用の CI/CD パイプラインを正しく作成しました。 

## <a name="clean-up-resources"></a>リソースのクリーンアップ

この CI/CD パイプラインの作業が終わったら、チュートリアルの間に作成したすべてのリソースを削除できます。

1. Azure portal から [`Resource Groups`] を参照して `VstsResourceGroup-LearnCluster-xxxx` をクリックします。xxx はアルファベットと数字のランダムなグループです。  
![LearnClusterRG](/media-draft/4-learnclusterrg.png)

2. `Learn` という名前の DevOps プロジェクトをクリックします。  
![Learn のリンク](/media-draft/4-learnlink.png)

3. [`Delete`] をクリックします。  
![削除](/media-draft/4-deleteproj.png)

4. [`Yes`] をクリックします。  
![はい](/media-draft/4-yes.png)

これにより、Azure で作成されたすべてのリソースが削除されます。

## <a name="summary"></a>まとめ

このチュートリアルで学習した内容は次のとおりです。
> [!div class="checklist"]
> * Azure DevOps プロジェクトを作成する
> * Azure DevOps プロジェクトのサンプル アプリを独自のものと置き換える
> * ビルドとリリースのパイプラインをカスタマイズする
