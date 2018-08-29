この演習では、歯車駆動の例を続け、温度サービスのロジックを追加します。 具体的には、HTTP 要求からデータを受信します。

## <a name="function-requirements"></a>関数の要件
ロジックにいくつかの要件を定義しましょう。
- 0 - 25 の温度に **[OK]** のフラグを設定してください
- 26 - 50 の温度に **[注意]** のフラグを設定してください
- 50 以上の温度に **[危険]** のフラグを設定してください

## <a name="adding-a-function-to-an-azure-function-app"></a>Azure 関数アプリに関数を追加する

既に理解していることですが、Azure Functions の使用方法を学習しているとき、Azure のヘルプを利用できます。 Functions を実際に始めるための優れた機能の 1 つがさまざまなテンプレートの 1 つを利用して関数を生成するという機能です。 この演習では、HttpTrigger テンプレートを使用して温度サービスを実装します。

1. Azure アカウントを使用して [Azure Portal](https://portal.azure.com) にサインインします。
1. 左側のメニューで **[すべてのリソース]** を選んで、**[escalator-functions-group]** を選択して、最初の演習で作成したリソース グループにアクセスします。
1. グループのリソースが表示されます。 **escalator-functions-xxxxxxx** 項目 (稲妻アイコンでも示されます) を選択し、前の演習で作成した関数アプリにアクセスします。
  ![関数アプリにアクセスする](../images/6-access-function-app.png)
1. ブレードに関数アプリの概要が表示されます。 左側にナビゲーション ツリーもあります。関数、プロキシ、スロットが定義されていれば、このツリーに表示されます。 関数はまだないため、空になります。 関数を作成するには、ナビゲーション ツリーの **[関数]** にマウス カーソルを合わせ、表示される **+** ボタンをクリックします。
  ![関数ナビゲーションを追加する](../images/5-function-add-button.png)
1. クイックスタートの用意されている関数フォームで、**[関数を独自に作成する]** セクションの **[カスタム関数]** リンクを選択します。
  ![用意されている関数フォーム](../images/6-custom-function.png)
1. これでテンプレートの一覧が提示されます。 HTTP トリガー テンプレートの JavaScript 実装を選択します。
  ![HTTP トリガー テンプレート](../images/6-httptrigger-template.png)
1. ブレードが表示されます。テンプレートで生成する内容を定義できます。 今回、**DriveGearTemperatureService** という名前の JavaScript 関数を生成します。 関数に名前を付けたら、**[作成]** ボタンを押します。
  ![HTTP トリガー フォームを作成する](../images/6-create-httptrigger-form.png)
1. 数秒後、関数のテンプレート化されたソース コードが提示されます。 用意されている関数は名前が渡されることを要求し、**Hello, {name}** を返します。

    ```javascript
    module.exports = function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + (req.query.name || req.body.name)
            };
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

1. ソース ビューの右側に 2 つのタブが表示されます。 **[ファイルの表示]** タブに、関数をサポートしているすべてのファイルが表示されます。**function.json** を選択すると、関数の構成が表示されます。 ここでは httpTrigger と出力バインドが定義されていることがわかります。 この構成からは、関数が HTTP 要求によって始動することがわかります。出力バインドからは、応答が HTTP 応答として送信されることがわかります。

    ```javascript
    {
      "disabled": false,
      "bindings": [
        {
          "authLevel": "function",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```

## <a name="running-the-premade-azure-function"></a>用意されている Azure 関数を実行する

関数を実行するには、コマンド プロンプトから cURL からの HTTP 要求を開始できます。 エンドポイント URL を見つけるには、関数コードに戻り、**[関数の URL の取得]** リンクを選択します。 このリンクをクリップボードにコピーします。  URL は次のようになります: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![エンドポイント URL の取得](../images/6-get-function-url.png)

その URL を使用し、cURL コマンドを実行して関数を開始します (URL は自分の URL に変更します)。

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![用意されている関数 cURL 応答](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a>関数にビジネス ロジックを追加する

この関数は、顧客からの温度測定値の配列を要求します。 これは要求本文の例です。

```javascript
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

用意されている関数コードを変更し、必要なビジネス ロジックを実装します。 index.js ファイルを開き、次のリストで置換します。

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        for(var i=0; i<req.body.readings.length; i++){
            var reading = req.body.readings[i];
            if(reading.temperature<=25){
                context.log('Reading is OK');
                reading.status = 'OK';
                continue;
            }
            if(reading.temperature<=50){
                context.log('Reading is CAUTION');
                reading.status = 'CAUTION';
                continue;
            }
            context.log('Reading is DANGER');
            reading.status = 'DANGER'
        }
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: {
                "readings": req.body.readings
            }
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please send an array of readings in the request body"
        };
    }
    context.done();
};
```

ログ ステートメントに注目してください。 関数が実行されると、ログ ウィンドウにメッセージが追加されます。

## <a name="testing-your-business-logic"></a>ビジネス ロジックのテスト

右側のポップアップ メニューからテスト ウィンドウを開き、要求本文テキスト ボックスに上のサンプル要求を貼り付けます。 **[実行]** ボタンを押し、出力を表示します。 下部のポップアップ メニューからログ ウィンドウを開き、ログのログ ステートメントを参照することもできます。
![ビジネス ロジックのテスト](../images/6-portal-testing.png) 出力ウィンドウの JSON データから、それぞれの測定値に温度ステータス フィールドが正しく追加されていることを確認できます。 場合によっては、モニター ダッシュボードにアクセスし、Application Insights に要求が記録されていることを確認することもできます。
![Application Insights の要求](../images/6-app-insights.png)
