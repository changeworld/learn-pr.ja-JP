この演習では、Azure 関数アプリを作成します。

1. Azure アカウントを使用して [Azure Portal](https://portal.azure.com) にサインインします。
1. Azure Portal の左上隅にある **[リソースの作成]** ボタンを選択し、**[コンピューティング]、[Function App]** の順に選択します。
  ![関数アプリ リソースを作成する](../images/4-create-function-app-blade.png)
1. グローバルで一意になるアプリ名を選択します。 これはサービスのベース URL として機能します。 **escalator-functions-xxxxxxx** という名前を付けることができます。xxxxxxx は自分のイニシャルや誕生年に変更できます。 グローバルに一意でない場合、別の組み合わせをお試しください。 有効な文字は a-z、0-9、- (ハイフン) です。
1. 関数アプリをホストする Azure サブスクリプションを選択します。
1. **escalator-functions-group** という名前の新しいリソース グループを作成します。 これは後のクリーンアップに役立ちます。
1. OS に **Windows** を選択します。
1. **[ホスティング プラン]** には、Azure のサーバーなしという特徴を最大限に活用できるように、**[従量課金プラン]** を選択します。
1. 自分または顧客が住んでいる場所に最も近い地域を選択します。
1. 新しいストレージ アカウントを作成し、それに **escalatorfunctions** という名前を付けます。
1. Azure Application Insights が **[オン]** になっていることを確認し、最も近いリージョンを選択します。
1. **[作成]** を選択します。デプロイには数分かかります。 完了後、通知が届きます。
  ![関数アプリの設定](../images/4-create-function-app-settings.png)

## <a name="verify-your-azure-function-app"></a>Azure 関数アプリを確認する

1. Azure Portal の左側にあるメニューから、**[リソース グループ]** を選択します。 利用できるグループの一覧には **escalator-functions-group** が表示されるはずです。
  ![リソース グループ](../images/4-resource-group.png)
1. **escalator-functions-group** を選択します。 次のようなリソース リストが表示されるはずです。![リソース リスト](../images/4-resource-list.png)
