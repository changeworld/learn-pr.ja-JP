このユニットでは、Ubuntu マシン上で .NET CLI を使用して ASP.NET Core Web アプリを作成します。

## <a name="aspnet-core-installation-on-linux-environment"></a>Linux 環境での ASP.NET Core のインストール

Microsoft の [.NET ダウンロード ページ](https://www.microsoft.com/net/download)にアクセスし、.NET Core SDK のページで説明されているのと同じ手順に従います。 以下のとおりです。 コマンドを実行するには、新しい**ターミナル** コマンド ライン インスタンスを開く必要があります。

### <a name="register-microsoft-key-and-feed"></a>Microsoft のキーとフィードを登録する

.NET をインストールする前に、Microsoft キーを登録し、製品のリポジトリを登録し、必要な依存関係をインストールする必要があります。 これは、マシンごとに 1 回だけ行う必要があります。

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-sdk"></a>.NET SDK をインストールする

インストールに使用可能な製品を更新し、.NET SDK をインストールします。

コマンド プロンプトで、次のコマンドを実行します。

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

インストール プロセスの間に、アカウント パスワードの入力を求められる場合がします。 パスワードを入力し、Enter キーを押して続行します。

インストールを確認するには、次のコマンドを実行します。

```console
dotnet --version
```

次のような出力が表示されます。

```console
2.1.302
```

.NET Core SDK をインストールしたので、ASP.NET Core もインストールされています。 .NET CLI と学習したコマンドを使用して、新しい ASP.NET Core プロジェクトを作成する方法を見ていきましょう。

## <a name="open-a-terminal-window"></a>ターミナル ウィンドウを開く

最初に、ターミナル ウィンドウを開く必要があります。 ターミナル ウィンドウの役割は Windows コマンド プロンプト ウィンドウと似ており、コマンドを実行することができます。

## <a name="create-a-new-web-project"></a>新しい Web プロジェクトを作成する

.NET CLI ツールの中心になるのは、*dotnet* ドライバー ツールです。 このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。

新しい ASP.NET Core MVC アプリケーションを作成するには、次のコマンドを入力することだけが必要です。

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

上記のコマンドを実行すると、*Documents* ルート フォルダーに移動し、新しいフォルダーが作成されます。 次に、そのフォルダー内に移動します。 最後に、.NET CLI コマンドを発行して、すべてのパッケージが復元された新しい ASP.NET MVC アプリケーションを生成します。

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

ここで行う必要があるのは、次のコマンドを発行してアプリケーションを実行することだけです。

```console
dotnet run
```

"*ターミナル*" では、*dotnet* コマンドによりアプリの実行に役立つ情報が表示されます。

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

出力ではアプリ開始後の状況が説明されます。アプリケーションは実行するとポート 5001 および 5002 (HTTPS を介して) をリッスンします。

開発モードのコンピューターで HTTPS をローカルに実行するには、**自己署名証明書**を作成して信頼する必要があります。

## <a name="create-a-self-signed-certificate"></a>自己署名証明書を作成する

開発用の自己署名証明書を生成するには、次のコマンドを実行します。

```console
dotnet dev-certs https -v
```

このコマンドを実行すると、次の出力が返されます。

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a>アプリケーションを実行する

このデモでは、Firefox ブラウザーを使っています。

ブラウザーを開き、アドレス「`http://localhost:5001`」を入力して、アプリケーションが正常に実行されていることを確認します。

![ASP.NET Core MVC テンプレートの既定の Web ページの Web ブラウザー ビューを示すスクリーンショット。](../media/5-asp-core-mvc-default-template.PNG)

> [!NOTE]
> 開発用の自己署名証明書は Firefox では検証できないため、アプリケーションの URL に対する**例外を追加する**必要があります。
