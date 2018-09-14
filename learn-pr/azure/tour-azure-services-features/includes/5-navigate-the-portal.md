アカウントを作成したら、**Azure portal** にサインインできます。 ポータルは、作成したすべてのサブスクリプションとリソースを操作できる Web ベースの管理サイトです。 Azure で行うほぼすべてのことを、この Web インターフェイスから実行できます。

## <a name="azure-portal-layout"></a>Azure portal のレイアウト

Azure portal は、Microsoft Azure を制御するための主要な GUI (グラフィカル ユーザー インターフェイス) です。 管理操作の大半はポータルで実行できます。一般に、ポータルはシングル タスクを実行する場合や、構成オプションの詳細を確認する場合に最適なインターフェイスです。

![Azure portal](../media-draft/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media-draft/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

ポータル ビューの残りの部分は、作業中の特定の要素に対応しています。 既定の (メイン) ページは "_ダッシュボード_" です。 これについては後ほど説明しますが、これはリソースのカスタマイズ可能な鳥瞰図を表しています。 ダッシュボードを使用して、管理する特定のリソースにジャンプしたり、リソース パネルの **[すべてのリソース]** エントリでリソースを検索したりできます。 仮想マシンや Web アプリなどのリソースを管理するときは、そのリソースに関する具体的な情報が表示される "_ブレード_" を使用します。

## <a name="what-is-a-blade"></a>ブレードとは

Azure portal では、ナビゲーションにブレード モデルが使用されます。 _ブレード_とはスライド式のパネルであり、ナビゲーション シーケンスにおけるシングル レベルの UI が含まれます。 たとえば、**仮想マシン**  >  **コンピューティング**  >  **Ubuntu Server** というシーケンスのそれぞれの要素が 1 つのブレードで表されます。

各ブレードには、情報と構成可能なオプションが含まれています。 これらのオプションの中には、別のブレードが生成されるものもあります。そのブレードは既存のブレードの右側に表示されます。 新しいブレードでは、さらに構成可能なオプションによって別のブレードが生成され、そのブレードからも別のブレードが生成されるというように続きます。 すぐに、いくつかのブレードを同時に開くようになります。 ブレードを最大化して画面全体に表示することもできます。

新しいブレードは常に所有者の右側に追加されるので、ウィンドウの下部にあるスクロールバーを使って戻ることで、構成内の現在の場所にどのように到達したかを確認できます。 また、ブレードの上隅にある `X` ボタンをクリックして、ブレードを個別に閉じることもできます。 保存していない変更がある場合は、続行すると変更内容が失われることを通知するメッセージが表示されます。

## <a name="configuring-settings-in-the-azure-portal"></a>Azure portal での設定の構成

Azure portal には、構成オプションがいくつか表示されます。ほとんどの場合、画面右上のステータス バーに表示されます。

### <a name="notifications"></a>通知

ベルのアイコンをクリックすると、**[通知]** ウィンドウが表示されます。 このウィンドウには、実行された過去のアクションとそのステータスが一覧表示されます。

![[通知] ブレード](../media-draft/5-notifications-blade.png)

### <a name="cloud-shell"></a>Cloud Shell

**Cloud Shell** アイコン (>_) をクリックすると、新しい Azure Cloud Shell セッションが作成されます。 Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーからアクセスできる対話型シェルです。 Azure Cloud Shell は、作業に最適なシェル エクスペリエンスを選択できる柔軟性を備えています。 Linux ユーザーは Bash エクスペリエンスを選ぶことができ、Windows ユーザーは PowerShell を選ぶことができます。 このブラウザベースのターミナルでは、ポータルに組み込まれているコマンド ライン インターフェイスを使用して、現在のサブスクリプション内のすべての Azure リソースを制御および管理できます。

![Cloud Shell](../media-draft/5-choose-shell.png)

### <a name="settings"></a>設定

Azure portal 設定を変更するには、**歯車**アイコンをクリックします。 これらの設定には、以下が含まれます。

- ログアウト時
- 配色
- ハイ コントラスト テーマ
- (モバイル デバイスへの) トースト通知
- ダブルクリックしてテーマを変更
- 言語
- 地域の形式

![ポータル設定](../media-draft/5-settings-blade.png)

設定を変更したら、**[適用]** をクリックして変更を確定します。

### <a name="feedback-blade"></a>[フィードバック] ブレード

**笑顔アイコン**をクリックすると、**[フィードバックの送信]** ブレードが開きます。 ここで Azure に関するフィードバックを Microsoft に送信できます。 Microsoft がメールでフィードバックに回答できるかどうかを指定できます。

![フィードバック](../media-draft/5-feedback-blade.png)

### <a name="help-blade"></a>[ヘルプ] ブレード

**疑問符**のアイコンをクリックすると、**[ヘルプ]** ブレードが表示されます。 ここでは、次のようなオプションから選択します。

- 新機能
- Azure のロードマップ
- ガイド付きツアーの起動
- キーボード ショートカット
- 診断の表示
- プライバシーと使用条件

### <a name="directory-and-subscription"></a>ディレクトリとサブスクリプション

**本とフィルター**のアイコンをクリックすると、**[ディレクトリ + サブスクリプション]** ブレードが表示されます。

Azure では、1 つのディレクトリに複数のサブスクリプションを関連付けることができます。 **[ディレクトリ + サブスクリプション]** ブレードで、サブスクリプションを切り替えることができます。 ここでは、サブスクリプションを変更するか、別のディレクトリに変更できます。

![ディレクトリ](../media-draft/5-directory-blade-1.png)

### <a name="profile-settings"></a>プロファイルの設定

右上隅にある自分の名前をクリックすると、自分のプロファイル設定を変更できます。
次のオプションがあります。

- Azure からサインアウトする
- パスワードの変更
- 連絡先情報の変更
- アクセス許可の表示
- Azure チームにアイデアを送信する
- 課金状況の表示
- ディレクトリを切り替えます (前のセクションと同じように、**[ディレクトリ + サブスクリプション]** ブレードが表示されます)。

![プロファイルの設定](../media-draft/5-portal-menu.png)

**[明細の表示]** をクリックすると、**[コスト管理 + 課金 - 請求書]** ページが表示されます。このページは、Azure におけるコストの発生場所の分析に役立ちます。

![課金ページ](../media-draft/5-portal-billing.png)

Azure は大規模な製品であり、Azure portal UI (ユーザー インターフェイス) もこれを反映しています。 スライド式ブレードのアプローチにより、さまざまな管理タスク間を簡単に移動できます。 練習のために、この UI を少し使ってみましょう。