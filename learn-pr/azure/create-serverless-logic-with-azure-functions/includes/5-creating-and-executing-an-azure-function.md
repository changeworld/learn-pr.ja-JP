関数アプリを作成したので、Azure 関数をビルド、構成、実行する方法について学習しましょう。

### <a name="triggers"></a>トリガー
Azure Functions はイベント ドリブンです。HTTP 要求やキューに追加されているメッセージの受信などのイベントに応答して実行されます。 

関数を開始するイベントの種類は、トリガーと呼ばれます。 Azure Functions では、少なくとも 1 つのトリガーを構成する必要があります。 構成しないと、関数を実行する方法がありません。

 Azure では、次のようなさまざまなトリガーがサポートされています。
* HTTPTrigger
* TimerTrigger
* GitHub webhook
* CosmosDBTrigger
* BlobTrigger
* QueueTrigger
* EventHubTrigger
* ServiceBusQueueTrigger
* ServiceBusTopicTrigger

### <a name="bindings"></a>バインド
Azure 関数バインドは、プログラムでデータを関数に接続する宣言型の方法です。

バインドはデータ サービスへの接続です。 ゼロまたは 1 つ以上のバインドを持つ Azure 関数では、複数のデータ ソースにアクセスできます。 また、バインドを入力バインドまたは出力バインドとして定義することも、バインドを読み取りバインドまたは書き込みバインドと認識することもできます。

BLOB がキューに追加されたときに関数を開始する QueueTrigger があるとします。 入力 BLOB バインドは、Azure BLOB ストレージへの接続に使用されます。 

出力バインドは、データの格納に使用されます。 例として、関数の戻り値は、出力バインドによって Azure テーブル ストレージに送信されます。

使用可能なバインドの完全なリストは、[Azure ドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)で入手できます。

### <a name="a-sample-trigger-and-binding-functionjson"></a>サンプルのトリガーとバインド (function.json)
これは、関数のトリガーとバインドのサンプルの定義です。 記述するバインドの種類によっては、異なるプロパティ値を設定する必要があることに注意してください。 各バインドには、それが入力バインドなのか出力バインドなのかを定義する方向もあります。 トリガーは常に入力バインドです。 このサンプルは、**myqueue-items** という名前のキューに追加されるメッセージによってトリガーされる関数を示しています。 次に、関数の戻り値を Azure テーブル ストレージ内の **outTable** テーブルに送信します。

```javascript
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
## <a name="premade-functions-and-templates"></a>作成済みの関数とテンプレート
Azure では、Azure 関数の一般的なシナリオ向けに、事前構成済みのテンプレートが用意されています。

### <a name="quickstart-templates"></a>クイック スタート テンプレート
Azure 関数を追加するには、関数アプリを選択する必要があります。 関数アプリは、山かっこ内の稲妻アイコンによって識別できます。  
![関数アイコン](../images/5-function-icon.png)

初めて Azure 関数を追加するときに、クイックスタート画面が表示されます。 この画面を使用して、目的のトリガーの種類とプログラミング言語を選択することができます。 その後、選択内容に基づいて、関数のコードと構成が Azure によって生成されます。  
![関数のクイックスタート](../images/5-quickstart-form.png)

### <a name="function-templates"></a>関数テンプレート
テンプレートの選択範囲は、クイックスタートで使用可能なものに限りません。 30 個を超えるさまざまなテンプレートから選択するオプションもあります。 テンプレート リストの画面には、後続の関数の作成時、またはクイックスタート画面で **[カスタム関数]** オプションを選択してアクセスすることができます。  
![関数テンプレート](../images/5-template-list.png)

## <a name="navigating-to-your-function-and-files"></a>関数とファイルに移動する
テンプレートから関数を作成すると、複数のファイルが作成されます。 JavaScript を使用して Webhook + API のクイックスタートを使用するよう選択したとします。 この場合、構成ファイルの **function.json** とソース コード ファイルの **index.js** が生成されます。 関数アプリにアクセスするときに、アプリで作成された関数を表示するメニュー ツリーが表示されます。 これもこの **[関数]** メニュー項目にあります。このメニュー項目から関数アプリに関数を追加できます (メニュー項目の上にカーソルを置くと、プラス記号 (+) アイコンが表示されます)。  
![[関数の追加] ボタン](../images/5-function-add-button.png) 

上記で説明したこのクイックスタートの場合、ツリー ビューで **[関数]** を展開すると、**HttpTriggerJS1** という既定の名前を持つ 1 つの関数が表示されます (関数アイコンでも示されます)。 この関数を選択すると、コード ウィンドウが開き、**index.js** ソース ファイルが表示されます。 コード ウィンドウの右側には、**[ファイルの表示]** タブを含むポップアップ メニューがあります。 このタブを選択すると、その関数を構成しているファイル構造が (ストレージを摸して) 表示されます。  
![関数テンプレート](../images/5-file-navigation.png)

## <a name="execution-options-for-testing-your-azure-function"></a>Azure 関数をテストするための実行オプション
Azure 関数を作成したら、それをテストする方法を知る必要があります。 手動で実行する方法と、Azure portal 内からテストする方法の 2 種類があります。

### <a name="manual-execution-of-a-function"></a>関数の手動での実行
構成されたトリガーを手動でトリガーすることによって、関数の実行を開始できます。 たとえば、HttpTrigger を使用している場合、Postman または cURL などのツールを使用して、自分の関数エンドポイント URL への HTTP 要求を開始することができます。  
![Postman での関数の実行](../images/5-postman-execution.png) エンドポイント URL を取得するには、左側のナビゲーションから、自分の HTTP トリガーを選択してから、**[関数の URL の取得]** ボタンを選択します。  
![関数の URL の取得](../images/5-get-function-url.png)

### <a name="testing-a-function-in-the-azure-portal"></a>Azure portal での関数のテスト
もう 1 つの方法として、Azure portal にも関数をテストするための便利な手段が用意されています。 コード ウィンドウの右側に、タブ付きのポップアップ ナビゲーション メニューがあります。 このメニューには、**[テスト]** 項目が含まれています。 メニューを展開してこのタブを選択して、別の方法で関数を実行し、結果を表示することができます。 HttpTrigger シナリオに合わせて、HTTP メソッドを設定し、クエリ文字列パラメーターと HTTP ヘッダーを要求に追加することができます。 要求本文を変更して、追加のシナリオをテストすることもできます。 出力ウィンドウに関数の結果が表示されます。  
![関数の portal での実行](../images/5-portal-execution.png)

## <a name="monitoring-an-azure-function"></a>Azure 関数の監視
関数を運用環境に対応させるためには、Azure 関数の開発時に、メッセージを記録することと例外シナリオを判断できることが重要です。 欠陥があるソースを追跡している場合、これは運用環境のシナリオでも同様に重要です。 Azure portal には、監視ダッシュボードを提供するユーザー インターフェイスと、ご使用の Azure Functions から取得した実行ログと例外を確認するための方法が用意されています。

### <a name="monitoring-dashboard"></a>監視ダッシュボード
関数アプリのナビゲーション メニューでは、関数ノードを展開すると **[監視]** メニュー項目が表示されます。 監視ダッシュボードには、関数の実行履歴を表示する簡単な方法が用意されています。 このビューには、タイムスタンプ、結果コード、期間、および操作 ID も表示されます (正常に完了した場合)。 データは、Azure Application Insights を通じて入力されます。  
![関数の監視](../images/5-monitor-function.png)

### <a name="log-window"></a>ログ ウィンドウ
関数にログ ステートメントを追加することもできます。 これらのステートメントは、関数のログ ウィンドウに表示されます。 ログ ウィンドウは、コード ウィンドウの下部にあるタブ付きのポップアップ メニューにあります。 JavaScript ベースの関数を使用している場合は、次の構文を使用してログ ステートメントを追加します。
```javascript
  context.log('Enter your logging statement here');
```  
![ログ ウィンドウ](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a>エラーと警告ウィンドウ
エラーと警告ウィンドウ タブは、ログ ウィンドウと同じポップアップ メニューで検索できます。 このウィンドウには、コード内のコンパイル エラーと警告が表示されます。 JavaScript は動的なインタープリタ型言語であるため、次の図は、C# 関数でのコンパイル エラーを示しています。  
![エラーと警告ウィンドウ](../images/5-errors-window.png)
