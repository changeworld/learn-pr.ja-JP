Azure DevOps projects を使用してカスタム アプリケーションの CI/CD パイプラインを正常に作成されました。 

## <a name="clean-up-resources"></a>リソースのクリーンアップ

完了したらこの CI/CD パイプラインを使用、チュートリアルの中に作成されたすべてのリソースを削除することができます。

1. Azure portal から参照`Resource Groups` をクリック`VstsResourceGroup-LearnCluster-xxxx`xxx はアルファベットと数字のランダムなグループ  
![LearnClusterRG](../media-drafts/4-learnclusterrg.png)

2. DevOps プロジェクトという名前をクリックします。 `Learn`  
![リンクをについて説明します。](../media-drafts/4-learnlink.png)

3. [`Delete`] をクリックします。  
![削除](../media-drafts/4-deleteproj.png)

4. [`Yes`] をクリックします。  
![はい](../media-drafts/4-yes.png)

これにより、Azure で作成されたすべてのリソースが削除されます。

## <a name="summary"></a>まとめ

このチュートリアルで学習した内容は次のとおりです。
> [!div class="checklist"]
> * Azure DevOps プロジェクトを作成する
> * Azure DevOps プロジェクトでサンプル アプリを独自で置き換えます
> * ビルドとリリース パイプラインをカスタマイズします。
