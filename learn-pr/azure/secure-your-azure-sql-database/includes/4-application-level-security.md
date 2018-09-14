ハッカーがデータベースにアクセスしようとしていることを想像してください。 データベースに接続するアプリケーションは、攻撃に対して脆弱スポットです。 これらのアプリケーションをセキュリティで保護されたメソッドを使用してデータベースに接続されていない可能性があります。

データベースには、独自のセキュリティが必要がありますが、データのセキュリティの重要な役割を果たしますデータベースへのアクセスします。 データベースの正常な侵害は、通常、SQL インジェクション攻撃の結果です。 SQL インジェクション攻撃は、データベースへのアクセスに推奨されるプラクティスを使用していないアプリケーションの結果です。

アプリケーション層では、データベースを保護するための手法を見てみましょう。

## <a name="sql-injection-attacks"></a>SQL インジェクション攻撃

[OWASP foundation](https://owasp.org)標準を信頼するアプリケーションをビルドするように設計された非営利組織です。 定期的にその発行する上位 10 個のセキュリティの脆弱性の一覧。

OWASP に従って最も一般的な脆弱性は、インジェクション攻撃は、通常 SQL インジェクション攻撃の形式になります。 SQL インジェクション攻撃では、SQL ステートメントに渡される情報が変更されます。 これらの変更されたクエリは、機密情報を返すか、データベース上の悪意のある操作を実行します。

ADO.NET を使用して、特定の顧客との対話をフェッチする次の ASP.NET Core コードを見てみましょう。

```csharp
public List<CustomerInteraction> GetCustomerInteractions(string customerId)
{
    var result = new List<CustomerInteraction>();

    using (var conn = new SqlConnection(ConnectionString))
    {
        var sql = "select Id, CustomerId, InteractionDate, Details " +
            "from CustomerInteractions where CustomerId = '" + customerId + "'";

        using (var command = conn.CreateCommand())
        {
            command.CommandText = sql;
            conn.Open();

            using (var reader = command.ExecuteReader())
            {
                if (reader.HasRows)
                {
                    while (reader.Read())
                        result.Add(GetInteractionsFromReader(reader));
                }
            }
        }
        return result.OrderByDescending(i => i.InteractionDate).ToList();
    }
}
```

CustomerId パラメーターがクエリに進む前にサニタイズされていないため、変更できることと、SQL 攻撃は、クエリ自体が先読みです。 クエリを呼び出している web サイト パラメーターを渡して、customerId、URL クエリ文字列に次のように。

.../Home/ViewInteractions?customerId=8c69a607-3c09-45ac-9beb-c59ca2de2385

この戦略では、問題は、customerId パラメーターを変更できます。 たとえば、クエリに追加の情報を格納し、別のテーブルからデータを選択するには、customerId パラメーターを変更できます。

```sql
select Id, CustomerId, InteractionDate, Details from CustomerInteractions where CustomerId = '8c69a607-3c09-45ac-9beb-c59ca2de2385'
```

クエリに、次を追加することで、情報を追加する UNION ステートメントを使用して情報を追加するようになりました変更できます。

```sql
union select Id, Id as CustomerId, getdate() as InteractionDate, CreditCardNumber + '/' + STR(CreditCardExpiryMonth, 2) + '/' + STR(CreditCardExpiryYear, 4) + ' cvv ' + STR(CreditCardCVV, 3) as Details from Customers --
```

SQL クエリは、URL でエンコードされた値が有効な web URL の一部として受け入れられるようにします。 URL は、web サイトに渡され、データベースで追加の SQL クエリを実行します。

```sql
%27+union+select+Id%2C+Id+as+CustomerId%2C+getdate%28%29+as+InteractionDate%2C+CreditCardNumber+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryMonth%2C+2%29+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryYear%2C+4%29+%2B+%27+cvv+%27+%2B+STR%28CreditCardCVV%2C+3%29+as+Details+from+Customers+--
```

この例はない方法ではデータ セキュリティの問題につながる、サイトから情報をサニタイズすることです。

![Web ブラウザーのアドレス バーの web アプリを使用して SQL インジェクション攻撃の試行の例を示すスクリーン ショット。](../media-draft/4-view-web-page-after-sql-injection.png)

> [!Note]
> インジェクション攻撃の詳細については、次を参照してください。、 [OWASP Foundation](https://www.owasp.org/)します。

## <a name="avoiding-sql-injection-attacks"></a>SQL インジェクション攻撃の回避

SQL インジェクション攻撃を回避するためには、常で任意のユーザーが入力した入力をサニタイズすることを確認する必要があります。 クエリにパラメーターとして使用されるユーザー入力を文字列の連結を使用して構築されたが実際のクエリ パラメーターとして渡されることはできません。

次の例では、CustomerId が今すぐで渡す方法を使用して、パラメーターとしてを表示できます、@ 記号。 パラメーターが明示的に定義されているし、クエリに渡される値。

```csharp
using (var command = conn.CreateCommand())
{
    var sql = "select Id, CustomerId, InteractionDate, Details " +
        "from CustomerInteractions where CustomerId = @CustomerId order by InteractionDate";

    command.CommandText = sql;
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);

    conn.Open();

    using (var reader = command.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
                result.Add(GetInteractionsFromReader(reader));
        }
    }
}
```

中核となる行のコードは次のとおりです。

```csharp
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);
```

先頭が英文字のデータの入力とパラメーター化クエリを使用して、データベースで SQL インジェクション攻撃の可能性が減少します。

この例では、概念を示す ASP.NET Core を使用します。 ただし、SQL Server へのアクセスをサポートするすべてのプログラミング システムのクエリの値にパラメーターを渡すメカニズムであることに留意してください。

## <a name="dynamic-data-masking"></a>動的データ マスク

お気付きかもしれません。 特に機密性の高いいくつかのデータベースの情報があります。クレジット_カード情報などです。 実際のシステムで暗号化されていないクレジット_カード情報を保存するとしないでください。 残念ながら、クレジット カード情報が暗号化されていないと、データを非表示にする別の方法を検索する必要があります。

ショッピング web サイトを構築して、プロセス中に、サイトがクレジット_カードの詳細を表示すると仮定します。 Xxxx xxxx xxxx 1234 のように表示する、ユーザーからのブロック最初の 12 桁の数字を表示するには。

この手法では、動的データ マスクは呼び出されます。 動的データ マスク、データベース内の列に対してマスクを追加することができます。

1. マスクを適用して、選択するデータベースを選択するために、ポータルを使用して、**動的データ マスク**オプションを**セキュリティ**設定します。

    マスク ルールの画面には、既存の動的データ マスク、および推奨事項の適用、動的データ マスクを持つ可能性のある列の一覧が表示されます。

    ![サンプル データベースの列をさまざまなデータベースの推奨されるマスクの一覧を示す Azure ポータルのスクリーン ショット。](../media-draft/4-view-recommended-masked-columns.png)

1. 列にマスクを追加するには、クリックして、**追加マスク**列に推奨されるマスクを追加するボタンをクリックします。

    ![使用されるマスク関数と Auzre ポータルを表示中の推奨される適用マスクされた列のスクリーン ショット。](../media-draft/4-recommended-masks-applied.png)

1. 新しい各マスクは、マスク ルールの一覧に追加されます。 をクリックして、**保存**マスクを適用するボタンをクリックします。

列を照会するときにデータベース管理者、元の値が引き続き表示されますが、管理者以外のユーザーはマスクの値を参照してください。

一覧にマスクから除外する SQL ユーザーを追加することによって、マスクされた以外のバージョンを表示するには、他のユーザーを許可することができます。

ご覧のとおり、マスクされたデータがどのようにと、管理者以外で照会します。

![電子メール、電話番号、SocialSecurityNumber、および CreditCardNumber を表示するデータベース クエリのスクリーン ショットには、管理者以外は表示は、マスクされた列があります。](../media-draft/4-sql-query-showing-masks.png)

ディスプレイ上のマスクはに基づいて推奨事項は、追加されたものすぎるマスクを手動で追加できます。 選択、+ マスク ボタンを追加して、pop over が許可すると、スキーマ、テーブル、および使用する列を選択します。 使用されるマスクを定義します。 など、使用できる標準のマスクがあります。

- 既定値は、代わりに、そのデータ型の既定値を表示します
- クレジット_カードの値は、大文字と小文字の x; その他のすべての数値に変換する番号の最後の 4 桁のみが表示されます。
- 電子メール、名前と電子メール アカウントの名の最初の文字を除くすべてのドメインを非表示にします。
- 数は、乱数値の範囲を指定します。 たとえば、クレジット_カードの有効期限の月と年で、1 から 12 か月間のランダムを選択して 2018 から年の範囲を 3000; に設定または
- カスタム文字列。 これにより、データの開始、文字、データの末尾から公開されると、データの残りの部分を繰り返す文字の数から公開されている文字数を設定することができます。
