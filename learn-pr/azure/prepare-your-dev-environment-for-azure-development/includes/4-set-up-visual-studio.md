Visual Studio は、幅広いプログラミング アプリケーションの種類と言語に対応するフル機能の統合開発環境 (IDE) です。 Visual Studio には、Microsoft Azure でのアプリケーション開発を特に目的とするツールと機能の完全なセットが含まれます。 つまり、Azure の開発、デバッグ、およびデプロイ ツールは IDE と緊密に統合されています。

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio 2017 は、Windows、Android、iOS、Web、Azure など、幅広い種類のアプリケーション開発に使用できるフル機能を備える IDE です。

Visual Studio をインストールするときに、利用できる複数の "*ワークロード*" が表示されます。 ワークロードは、インストールできる機能の領域を定義するライブラリとコンポーネントのコレクションです。 コンポーネントを個別にインストールする場合はそれぞれの間の依存関係を理解している必要がありますが、ワークロードを使用すると "テーマ" でインストールを行うことができます。 これにより、すべての必要なコンポーネントが確実に含まれます。

Visual Studio の基本インストールには、Azure 開発用のツールまたはライブラリは付属しません。 そのため、Azure SDK、ツール、テンプレート プロジェクトなどの Azure 開発ワークロードを含める必要があります。

Visual Studio をインストールするには、[インストーラーをダウンロード](https://visualstudio.microsoft.com/)します。 インストーラーではインストールするワークロードの指定を求められるので、そこで Azure 開発ワークロードを指定します。 追加の機能が必要な場合は、通常、NuGet パッケージまたは Visual Studio 拡張機能を介して追加します。

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Visual Studio for Mac は、macOS 用にネイティブに設計されて開発された IDE です。 Android や iOS 上のモバイル アプリ、Web、.NET Core 向けにソリューションを構築できます。

Visual Studio for Mac の基本インストールには、Azure ツールのコンテキスト統合が付属しています。 たとえば、Android 向けの Xamarin アプリを構築している場合、含まれている接続済みサービス テンプレートで、Azure App Service でモバイル バックエンドを作成するためのリンクが提供されます。 Azure 関数を作成する場合は、クラウド カテゴリの下のプロジェクト テンプレートを使用します。

基本的なインストールに含まれていない Azure の特性と機能用のツールが必要な場合は、NuGet パッケージまたは Visual Studio for Mac 拡張機能を追加する必要があります。

Visual Studio for Mac をインストールするには、[インストーラーをダウンロード](https://visualstudio.microsoft.com/)します。 インストーラーでは、システムが調べられて、必要なコンポーネントまたは更新する必要があるコンポーネントが特定されます。

> [!NOTE]
> 特定のコンポーネントをインストールする場合、コンピューターでの管理者資格情報の指定を求められることがあります。