この演習では、Azure 関数アプリを作成します。

1. ご自分の Azure アカウントを使用して [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。
1. Azure portal の左上隅にある **[リソースの作成]** ボタンを選択し、**[計算]、[Function App]** の順に選択して、Function App の *[作成]* ブレードを開きます。
  ![*[リソースの作成]* に続き *[計算]* と *[Function App]* の選択が強調表示されている Azure portal のスクリーンショット](../images/4-create-function-app-blade.png)
1. グローバルに一意のアプリ名を選択します。 これはサービスのベース URL として機能します。 たとえば、**escalator-functions-xxxxxxx** という名前を付けることができます。xxxxxxx は自分のイニシャルや誕生年に置き換えることができます。 グローバルに一意でない場合、別の組み合わせをお試しください。 有効な文字は a-z、0-9、- (ハイフン) です。
1. 関数アプリをホストする Azure サブスクリプションを選択します。
1. **escalator-functions-group** という名前の新しいリソース グループを作成します。 このモジュールで使用されるすべてのリソースを保持するためのリソース グループを使用すると、後でクリーンアップするときに役立ちます。
1. OS に **Windows** を選択します。
1. **[ホスティング プラン]** には、Azure のサーバーなしという特徴を最大限に活用できるように、**[従量課金プラン]** を選択します。
1. 自分または顧客が住んでいる場所に最も近い地域を選択します。
1. 新しいストレージ アカウントを作成し、それに **escalator functions** という名前を付けます。
1. Azure Application Insights が **[オン]** になっていることを確認し、自分 (または顧客) から最も近いリージョンを選択します。
完了すると、構成は次のスクリーンショットにある構成のようになります。

  ![前の手順に従ってすべてのフィールドが構成された Function App の *[作成]* 構成画面のスクリーンショット。](../images/4-create-function-app-settings.png)

1. **[作成]** を選択します。デプロイには数分かかります。 完了すると、通知が届きます。

## <a name="verify-your-azure-function-app"></a>Azure 関数アプリを確認する

1. Azure Portal の左側にあるメニューから、**[リソース グループ]** を選択します。 利用できるグループの一覧には **escalator-functions-group** が表示されるはずです。
  ![ビューにリソース グループ **escalator-functions-group** がある、Azure portal のリソース グループ画面のスクリーンショット。](../images/4-resource-group.png)
1. **escalator-functions-group** を選択します。 次のようなリソース リストが表示されるはずです。
  ![App Service プラン、ストレージ アカウント、Application Insights および App Service へのエントリを含む、**escalator-functions-group** グループ内のすべてのリソースのスクリーンショット](../images/4-resource-list.png)

App Service として一覧表示されている稲妻アイコンの付いた項目は、あなたの新しい関数アプリです。 
