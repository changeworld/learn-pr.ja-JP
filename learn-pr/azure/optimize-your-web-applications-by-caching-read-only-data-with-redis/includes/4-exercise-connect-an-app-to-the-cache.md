この演習では、よく使用される値を格納および返すために、Azure Redis Cache インスタンスを作成します。

## <a name="create-a-cache"></a>キャッシュを作成する

1. Azure portal サインインします。 **[Create a resource]** \(リソースの作成\)、**[データベース]**、**[Redis Cache]** の順にクリックします。

    次のスクリーン ショットでは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。

    ![[Create a resource]\(リソースの作成\)、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal データベースのオプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a>キャッシュを構成する

1. **DNS 名:** **ContosoSportsApp1028** など、グローバルに一意の名前を作成します。

1. **サブスクリプション**: ご使用のサブスクリプションを選択します。

1. **リソース グループ**: 新しいリソース グループを作成し、名前を付けます。

1. **場所:** ほとんどのお客様の地域はニューヨークなので、**[米国東部]** を選択する必要があります。

1. **価格レベル**: **[Basic C0]** を選択します。

1. **[作成]** をクリックします。

    次のスクリーンショットは、新しい Redis Cache のリソースの作成に使用された、代表的な構成を示しています。

    ![DNS 名、サブスクリプション、新しいリソース グループ、場所、価格レベルの構成例が入力された、新しい Redis Cache のリソースを作成するときの Azure portal のブレードのスクリーンショット。](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> 続行する前に、キャッシュがデプロイされるまで待つ必要があります。 このプロセスにはしばらく時間がかかることがあります。

## <a name="retrieve-the-access-keys-and-host-name"></a>アクセス キーおよびホスト名を取得する

1. Azure portal で、ご自分のキャッシュを参照し、**[設定]**  >  **[アクセス キー]** を選択します。 **[プライマリ接続文字列 (StackExchange.Redis)]** のメモをとります。

    このキーには、使用している StackExchange.Redis パッケージのアプリケーション設定で使用する完全な接続文字列に、ご使用のプライマリ キーとホスト名を含みます。

1. お使いのコンピューターでメモ帳か別のテキスト エディターを起動します。

1. 次の内容を追加します。

    `<connection-string>` を、上記のご使用のキャッシュのプライマリ接続文字列に置き換えます。

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. ファイルを、ご使用のサンプル アプリケーションのソース コードには含まれない場所に保存します。 このチュートリアルでは、ファイルを `C:\AppSecrets\CacheSecrets.config` として保存します。

## <a name="create-a-console-app"></a>コンソール アプリを作成する

1. Visual Studio を起動し、**[ファイル]**  >  **[新規]**  >  **[プロジェクト...]** メニュー項目の順にクリックします。

1. **[Visual C#]**  >  **[Windows デスクトップ]**  >  **[コンソール アプリ]** テンプレートの順に選択します。

1. ご自分のアプリに名前を付け、**[OK]** をクリックして、新しいコンソール アプリケーションを作成します。

## <a name="configure-the-cache-client"></a>キャッシュ クライアントを構成する

このセクションでは、.NET に **StackExchange.Redis** クライアントを使用するようにコンソール アプリを構成します。

1. Visual Studio で、**[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックし、[パッケージ マネージャー コンソール] ウィンドウから次のコマンドを実行します。

    ```powershell
    Install-Package StackExchange.Redis
    ```

インストールが完了すると、StackExchange.Redis キャッシュ クライアントをプロジェクトに使用できるようになります。

## <a name="connect-to-the-cache"></a>キャッシュに接続する

1. Visual Studio で **ソリューション エクスプローラー**内の **App.config** ファイルを開き、**CacheSecrets.config** ファイルを参照する **appSettings** ファイル属性を含むようにそれを更新します。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. ソリューション エクスプローラーで **[参照]** を右クリックし、**[参照の追加]** をクリックします。**[System.Configuration]** を **[アセンブリ]**  >  **[フレームワーク]** の一覧から選択し、**[OK]** をクリックします。

1. 次の `using` ステートメントを **Program.cs** に追加します。

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

Azure Redis Cache への接続は、ConnectionMultiplexer クラスによって管理されています。 このクラスは、クライアント アプリ全体で共有して再利用する必要があります。 操作ごとに新しい接続を作成しないでください。

ソース コード内に資格情報を保存することは絶対に避けてください。 このサンプルをシンプルにするために、外部のシークレット構成ファイルのみを使用しています。 実際には Azure Key Vault と証明書を使用することをお勧めします。

1. **Program.cs** で、次のメンバーをコンソール アプリの Program クラスに追加します。

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

アプリで ConnectionMultiplexer インスタンスを共有するこの方法では、接続されたインスタンスを返す静的プロパティを使用します。 このコードにより、接続された 1 つの ConnectionMultiplexer インスタンスだけがスレッドセーフな方法で初期化されます。 **abortConnect** は false に設定されており、Azure Redis Cache への接続が確立されていない場合でも呼び出しが成功します。 ConnectionMultiplexer の主な機能の 1 つは、ネットワーク問題などの原因が解決されると、キャッシュへの接続が自動的に復元されることです。

**CacheConnection** appSetting の値は、Azure portal からパスワード パラメーターとしてキャッシュ接続文字列を参照するために使用されます。
