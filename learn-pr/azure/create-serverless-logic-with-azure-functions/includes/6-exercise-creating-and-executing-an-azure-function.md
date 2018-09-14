この演習では、歯車駆動の例を続け、温度サービスのロジックを追加します。 具体的には、HTTP 要求からデータを受信します。

## <a name="function-requirements"></a>関数の要件
ロジックにいくつかの要件を定義しましょう。
- 0 - 25 の温度に **[OK]** のフラグを設定してください
- 26 - 50 の温度に **[注意]** のフラグを設定してください
- 50 以上の温度に **[危険]** のフラグを設定してください

## <a name="add-a-function-to-an-azure-function-app"></a>Azure 関数アプリに関数を追加する

前のユニットで説明したように、Azure では関数のビルドを始めるのに役立つテンプレートを提供しています。 この演習では、HttpTrigger テンプレートを使用して、温度サービスを実装します。

1. ご自分の Azure アカウントを使用して [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。
1. 左側のメニューで **[すべてのリソース]** を選んで、**[escalator-functions-group]** を選択し、最初の演習で作成したリソース グループを選択します。
1. グループのリソースが表示されます。 **escalator-functions-xxxxxxx** 項目 (稲妻アイコンでも示されます) を選択し、前の演習で作成した関数アプリにアクセスします。
  ![escalator-functions-group と作成した関数アプリが強調表示されている、ポータル内の [すべてのリソース] のスクリーンショット。](../images/6-access-function-app.png)
1. 左側のメニューには、関数アプリ名と 3 項目 (*[関数]*、*[プロキシ]*、*[スロット]*) を含むサブメニューが表示されます。  最初の関数の作成を開始するには、ナビゲーション ツリーの **[関数]** にマウス ポインターを置き、表示された **+** ボタンをクリックします。
  ![[関数] メニュー項目をポイントしたときに表示されるプラス記号を示すスクリーンショット。クリックすると、関数の作成手順が開始されます。](../images/5-function-add-button.png)
1. クイック スタート画面で、次のスクリーンショットで示されているように、**[Get started on your own]\(独自に作業を開始する\)** セクションの **[カスタム関数]** リンクを選択します。
  ![[カスタム関数] ボタンが強調表示されているクイック スタート画面のスクリーンショット。](../images/6-custom-function.png)
1. 次のスクリーンショットに示すように、画面に表示されたテンプレートの一覧から、HTTP トリガー テンプレートでの JavaScript の実装を選択します。
  ![HTTP トリガーと JavaScript オプションが強調表示されているテンプレートの一覧のスクリーンショット。](../images/6-httptrigger-template.png)
1.  表示された **[新しい関数]** ダイアログの名前フィールドに「**DriveGearTemperatureService**」と入力します。 関数に名前を付けたら、次のスクリーンショットに示されているように、**[作成]** ボタンを押します。
  ![強調表示され、"DriveGearTemperatureService" の値が設定されている [名前] フィールドが示されている、新しい関数フォームのスクリーンショット](../images/6-create-httptrigger-form.png)
1. 関数の作成が完了すると、コード エディターが *index.js* コード ファイルの内容と共に表示されます。 テンプレートが生成された既定のコードは、次のスニペット内に一覧されます。

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

関数では、HTTP 要求のクエリ文字列から、または要求本文の一部として渡される名前が必要です。 この関数では、メッセージ "**こんにちは、{名前}**" を返し、要求で送信された名前をエコー バックすることによって応答が行われます。

1. ソース ビューの右側に 2 つのタブが表示されます。 **[ファイルの表示] には、関数のコードと構成ファイルが一覧表示されます。  関数の構成を表示するには、**function.json** を選択します。 次のスニペットは、**function.json** ファイルの内容を示しています。 この構成では、HTTP 要求を受け取ったときに関数が実行されることを宣言します。 出力バインドでは、HTTP 応答として送信される応答を宣言します。

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
> [!TIP]
> 上記の JSON ファイルでは、トリガーがバインドの配列で定義されていることに注意してください。 そのため、トリガーは実際には特殊な種類のバインドです。

## <a name="run-our-function"></a>関数を実行する

> [!TIP]
> **cURL** は、ファイルを送受信するために使用できるコマンドライン ツールです。 cURL.exe では、HTTP、HTTPS、FTP、FTPS、SFTP、LDAP、TELNET、SMTP、POP3 などのさまざまなプロトコルがサポートされます。詳細については、次のリンクを参照してください。
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

関数をテストするため、コマンド ラインで cURL を使用して、関数 URL に HTTP 要求を送信できます。 関数のエンドポイント URL を見つけるには、次のスクリーンショットに示すように、関数コードに戻り、**[関数の URL の取得]** リンクを選択します。 このリンクをクリップボードにコピーします。  

 ![ポータルの [Function App] セクションにあるコード エディターのスクリーンショット。 [関数の URL の取得] コマンドは、右上に強調表示されています。](../images/6-get-function-url.png)

この関数の URL を使用して、URL を自分の URL に置き換える次の cURL コマンドをコマンド ラインから実行します。

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

次のスクリーン ショットは、コマンドラインで既定の実装から受信した応答の例を示しています。

![サンプルの cUrl コマンドと応答のコマンド ラインのスクリーンショット。](../images/6-premadefunction-curl.png)

## <a name="add-business-logic-to-the-function"></a>関数にビジネス ロジックを追加する

次は、受け取った測定温度を確認し、それぞれの状態を設定する関数にロジックを追加します。

この関数では、温度測定値の配列が要求されます。 次の JSON スニペットは、この関数に送信する要求本文の例です。 `reading` エントリごとに、ID、タイムスタンプ、温度があります。

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

次に、関数にある既定のコードを、ビジネス ロジックを実装する次のコードに置き換えます。 

1. **index.js** ファイルを開き、次のコードで置き換えます。

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        req.body.readings.forEach(function(reading) {
            
            if(reading.temperature<=25) {
                reading.status = 'OK';
            } else if (reading.temperature<=50) {
                reading.status = 'CAUTION';
            } else {
                reading.status = 'DANGER'
            }
            context.log('Reading is ' + reading.status);
        });

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
ここで追加したロジックは簡単です。 測定値の配列を反復処理して、温度フィールドを確認します。 そのフィールドの値に応じて、**[OK]**、**[注意]**、または **[危険]** の状態を設定します。 次に、各エントリに追加された状態フィールドと共に、測定値の配列を返信します。

ログ ステートメントに注目してください。 関数が実行されると、これらのステートメントによってログ ウィンドウにメッセージが追加されます。

## <a name="test-our-business-logic"></a>ビジネス ロジックをテストする

この場合、ポータル内の **[テスト]** ウィンドウを使用して、関数をテストします。

1. 右側のポップアップ メニューから **[テスト]** ウィンドウを開きます。
1. 上記の要求のサンプルを要求本文のテキスト ボックスに貼り付けます。 
1. **[実行]** を選択して、出力ウィンドウに応答を表示します。 ログ メッセージを表示するには、ページの下部にあるポップアップで **[ログ]** タブを開きます。 次のスクリーンショットでは、出力ウィンドウに応答の例と **[ログ]** ウィンドウにメッセージが示されています。

![ポータルでの関数インターフェイス内の [テスト] と [ログ] タブのスクリーンショット。 関数からの応答のサンプルは、[テスト] タブの出力ウィンドウに表示されます。](../images/6-portal-testing.png)

出力ウィンドウで、状態フィールドが各測定値に正しく追加されていることを確認できます。

**[監視]** ダッシュボードに移動すると、Application Insights に要求が記録されていることを確認することができます。

![関数からの成功メッセージを表示している "監視" ダッシュボードの一部のスクリーンショット。](../images/6-app-insights.png)

