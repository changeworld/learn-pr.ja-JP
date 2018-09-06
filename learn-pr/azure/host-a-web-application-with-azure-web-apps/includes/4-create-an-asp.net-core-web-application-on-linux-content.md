<span data-ttu-id="03b9b-101">Web アプリケーションをビルドするためにオープン ソース テクノロジを使用することにしました。</span><span class="sxs-lookup"><span data-stu-id="03b9b-101">You've decided to use an open-source technology for building your web application.</span></span> <span data-ttu-id="03b9b-102">ASP.NET Core がクロス プラットフォームのオープン ソース フレームワークであることはわかっています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-102">You know that ASP.NET Core is a cross-platform and open-source framework.</span></span> <span data-ttu-id="03b9b-103">ASP.NET Core を使用して、Linux 環境で Web アプリを開発することにします。</span><span class="sxs-lookup"><span data-stu-id="03b9b-103">You decide to develop your web app in a Linux environment using ASP.NET Core!</span></span>

<span data-ttu-id="03b9b-104">Azure の Web Apps では好みの Web テクノロジを使用できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-104">Web Apps in Azure allows you to use your favorite web technology.</span></span> <span data-ttu-id="03b9b-105">最も使い慣れているのが Node.js、PHP、.NET Core のいずれであれ、Web Apps は適切に動作します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-105">Whether you're most comfortable with Node.js, PHP, or .NET Core, Web Apps will work for you.</span></span>

<span data-ttu-id="03b9b-106">ここでは、.NET コマンド ライン インターフェイス (CLI) を使用して ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-106">Here, you will create an ASP.NET Core application using the .NET command-line interface (CLI).</span></span>

## <a name="what-is-aspnet-core"></a><span data-ttu-id="03b9b-107">ASP.Net Core とは</span><span class="sxs-lookup"><span data-stu-id="03b9b-107">What is ASP.NET Core?</span></span>

<span data-ttu-id="03b9b-108">ASP.NET Core は Microsoft の一般的な ASP.NET Core Web フレームワークが進化した最新のものであり、最新のクラウド ベースのインターネット接続アプリケーションをビルドするためのクロスプラットフォームのオープン ソース フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-108">ASP.NET Core is the latest evolution of Microsoft's popular ASP.NET web framework, a cross-platform, open-source framework for building modern, cloud-based, and internet-connected applications.</span></span>

<span data-ttu-id="03b9b-109">ASP.NET Core アプリケーションは、.NET Core Framework または既存の完全な .NET Framework を対象に記述できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-109">ASP.NET Core applications can be written to target the .NET Core Framework or the existing, full .NET Framework.</span></span>

<span data-ttu-id="03b9b-110">クロス プラットフォームのオープン ソース フレームワークであるため、Windows、macOS、および Linux を含むさまざまなプラットフォーム上に ASP.NET Core アプリをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-110">Being a cross-platform and open-source framework, you can build ASP.NET Core apps on a variety of platforms, including Windows, macOS, and Linux.</span></span> <span data-ttu-id="03b9b-111">これまで、Microsoft では Windows と macOS の両方の環境向けに Visual Studio IDE を提供しています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-111">Thus far, Microsoft offers the Visual Studio IDE for both Windows and macOS environments.</span></span> <span data-ttu-id="03b9b-112">また、Visual Studio Code エディターはクロス プラットフォームであり、これらと互換性があります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-112">In addition, the Visual Studio Code editor is cross-platform and compatible with them.</span></span>

><span data-ttu-id="03b9b-113">ASP.NET Core アプリケーションをさまざまなプラットフォームでビルドできるようにするため、Microsoft では .NET Core CLI ツールを導入しました。このツールは、豊富な一貫性のあるクロスプラットフォームの API セットを使用して、アプリケーションをビルド、テスト、公開するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-113">To support building ASP.NET Core applications on different platforms, Microsoft introduced the .NET Core CLI tools to help you build, test, and publish your applications using a rich, consistent, and cross-platform set of APIs.</span></span>

<span data-ttu-id="03b9b-114">ASP.NET Core を使用することで、Web アプリとサービス、IoT アプリ、およびモバイル バック エンドをビルドできます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-114">With ASP.NET Core, you can build web apps and services, IoT apps, and mobile back ends.</span></span> <span data-ttu-id="03b9b-115">ASP.NET Core アプリケーションはクラウド内またはオンプレミスでホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-115">ASP.NET Core applications can be hosted either in the cloud or on-premises.</span></span>

<span data-ttu-id="03b9b-116">意図的に、ASP.NET Core は、埋め込みの Web サーバーと、アプリケーション コードを実行するランタイム環境で構成されています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-116">By design, ASP.NET Core consists of an embedded web server and a runtime environment that runs the application code.</span></span> <span data-ttu-id="03b9b-117">アプリケーション コードは、より小さいモジュールとパッケージに依存する再作成された ASP.NET MVC フレームワークを使用して記述されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-117">The application code is written using a reworked ASP.NET MVC framework that relies on smaller modules and packages.</span></span> <span data-ttu-id="03b9b-118">その結果、Web アプリケーション ブループリントがより小さくなり、クラウド環境での管理とホストが容易になります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-118">The result is a smaller web application blueprint that is easy to maintain and host over cloud environments.</span></span>

![ASP.NET Core のアーキテクチャ](../media-draft/4-asp-net-core-architecture.png)

<span data-ttu-id="03b9b-120">ASP.NET Core アプリケーションは、**dotnet** ドライバー ツールを通じて呼び出されるスタンドアロンの**コンソール** アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-120">ASP.NET Core applications are standalone **console** applications invoked through the **dotnet** driver tool.</span></span> <span data-ttu-id="03b9b-121">ASP.NET Core アプリケーションは、IIS ワーカー プロセスに読み込まれるのではなく、外部のコンソール アプリケーションを実行する **AspNetCoreModule** というネイティブ IIS モジュールを通じて読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-121">ASP.NET Core applications are not loaded into the IIS worker process, but rather, are loaded through a native IIS module called **AspNetCoreModule** that executes the external console application.</span></span>

## <a name="how-to-create-an-aspnet-core-web-project"></a><span data-ttu-id="03b9b-122">ASP.NET Core Web プロジェクトの作成方法</span><span class="sxs-lookup"><span data-stu-id="03b9b-122">How to create an ASP.NET Core web project</span></span>

<span data-ttu-id="03b9b-123">新しい ASP.NET Core プロジェクトを作成するためのいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-123">There are a few options for creating a new ASP.NET Core project:</span></span>

- <span data-ttu-id="03b9b-124">Visual Studio (Windows および macOS バージョン) のテンプレートを使用して、新しいプロジェクトを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-124">You can use Visual Studio (Windows and macOS versions) templates to generate a new project.</span></span> <span data-ttu-id="03b9b-125">Visual Studio では、Web プロジェクトの作成に使用できるさまざまなテンプレートが提供されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-125">Visual Studio offers a variety of templates that you can use to create web projects.</span></span> <span data-ttu-id="03b9b-126">たとえば、**空**のテンプレートを使用して、基本的なセットアップで必要最小限の ASP.NET Core プロジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-126">For instance, you can use the **Empty** template to create a bare-bones ASP.NET Core project with the basic setup.</span></span> <span data-ttu-id="03b9b-127">さらに、**[Web Application (Modal-View-Controller)]\(Web アプリケーション (モデル ビュー コントローラー)\)** テンプレートを使用して、アプリケーションのコーディングを開始するのに役立つサンプルの**コントローラー**と**ビュー**で本格的な ASP.NET Core MVC アプリケーションを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-127">In addition, you can use the **Web Application (Modal-View-Controller)** template to generate a full-fledged ASP.NET Core MVC application, with sample **controllers** and **views** that can help you start coding your application.</span></span> <span data-ttu-id="03b9b-128">最新の **Web アプリケーション** プロジェクト テンプレートを使用すれば、従来の MVC プロジェクトの構造ではなく、Razor ページに基づく ASP.NET Core プロジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-128">The latest arrival is the **Web Application** project template that is used to create an ASP.NET Core project based on Razor pages and not on the traditional MVC project structure.</span></span>

- <span data-ttu-id="03b9b-129">.NET Core CLI ツールを使用して、新しい ASP.NET Core プロジェクトを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-129">You can use the .NET Core CLI tools to generate a new ASP.NET Core project.</span></span> <span data-ttu-id="03b9b-130">Microsoft では、Visual Studio および CLI ツールの両方でほぼ共通の ASP.NET Core プロジェクト テンプレートのセットが保持されています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-130">Microsoft maintains an almost common set of ASP.NET Core project templates for both Visual Studio and the CLI tools.</span></span> <span data-ttu-id="03b9b-131">CLI ツールとの唯一の違いは、新しい ASP.NET Core プロジェクトを作成するためにコマンドを入力する必要があることです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-131">The only difference with the CLI tools is that you need to type commands to create a new ASP.NET Core project.</span></span>
> <span data-ttu-id="03b9b-132">.NET CLI ツールでは、**テンプレート エンジン**を利用して、さまざまなプロジェクト テンプレートをサポートします。</span><span class="sxs-lookup"><span data-stu-id="03b9b-132">The .NET CLI tools make use of the **templating engine** to support different project templates.</span></span>  <span data-ttu-id="03b9b-133">詳細については、GitHub リポジトリの .NET CLI ツールで内部的に使用される[テンプレート エンジン](https://github.com/dotnet/templating)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="03b9b-133">To learn more, visit the GitHub repository for the [templating engine](https://github.com/dotnet/templating) used internally by the .NET CLI tools.</span></span>

<span data-ttu-id="03b9b-134">ここでは、ASP.NET Core プロジェクトを生成するための代表的なツールについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="03b9b-134">The above are considered the top tools for generating ASP.NET Core projects.</span></span> <span data-ttu-id="03b9b-135">しかし、検索および探索できるツールは他にもあります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-135">However, there are more tools out there that you can search for and explore.</span></span>

<span data-ttu-id="03b9b-136">特筆すべきは、さまざまなツールで生成されるプロジェクトは若干異なる場合がありますが、すべて有効で最適化された ASP.NET Core プロジェクトが生成されることです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-136">It's worth mentioning that the projects generated by the different tools can be slightly different, however, they all generate valid and optimized ASP.NET Core projects.</span></span>

## <a name="net-cli-tools"></a><span data-ttu-id="03b9b-137">.NET CLI ツール</span><span class="sxs-lookup"><span data-stu-id="03b9b-137">.NET CLI tools</span></span>

<span data-ttu-id="03b9b-138">.NET CLI ツール (.NET Core CLI とも呼ばれる) はクロスプラットフォーム ツールであり、パッケージの作成や復元、およびコマンド ラインからの </span><span class="sxs-lookup"><span data-stu-id="03b9b-138">The .NET CLI tools, also known as .NET Core CLI, is a cross-platform tool that provides commands for creating and restoring packages, and for building, running.</span></span> <span data-ttu-id="03b9b-139">.NET アプリケーションのビルド、実行、公開のためのコマンドが提供されます。全機能装備の IDE は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="03b9b-139">and publishing .NET applications from the command line without the need for a full-featured IDE.</span></span>

<span data-ttu-id="03b9b-140">.NET CLI は、.NET Core SDK の一部としてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-140">The .NET CLI is installed as part of the .NET Core SDK.</span></span> <span data-ttu-id="03b9b-141">複数のバージョンの CLI を同じコンピューター上に共存させ、並行して実行することができます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-141">Multiple versions of the CLI can coexist on the same machine and run side by side.</span></span>

<span data-ttu-id="03b9b-142">.NET CLI の使用を開始するには、ご利用のアプリ (Windows、macOS、または Linux ) の開発に使用する環境に適した .NET Core SDK をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-142">To start using the .NET CLI, you need to install the relevant .NET Core SDK with respect to the environment that you are using to develop your app: Windows, macOS, or Linux.</span></span> <span data-ttu-id="03b9b-143">[.NET のダウンロード](https://www.microsoft.com/net/download?initial-os=linux) ページに移動して、Linux 用の .NET Core SDK を取得します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-143">Go to [.NET Downloads](https://www.microsoft.com/net/download?initial-os=linux) and grab the .NET Core SDK for Linux.</span></span> <span data-ttu-id="03b9b-144">ここでは、VMware Workspace Player 14 を使用する代表的な Windows 10 上で実行されている Ubuntu 18.04 OS を使って、.NET CLI によって提供されるさまざまなコマンドを示します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-144">I am using an Ubuntu 18.04 OS running on top Windows 10 using VMware Workspace Player 14 to demonstrate the different commands offered by .NET CLI.</span></span>

<span data-ttu-id="03b9b-145">コマンド ラインを開き、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-145">Open the command line and type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="03b9b-146">このコマンドではインストールされている .NET CLI のバージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-146">This command displays the version of the .NET CLI installed.</span></span>

<span data-ttu-id="03b9b-147">コンピューターで上記のコマンドを実行すると、`2.1.302` と表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-147">Running the above command on my machine displays this: `2.1.302`.</span></span>

<span data-ttu-id="03b9b-148">.NET CLI の一般的なコマンドをいくつか見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="03b9b-148">Let us start exploring some of the popular commands of the .NET CLI.</span></span>

<span data-ttu-id="03b9b-149">*dotnet* コマンドの一般的な構文は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-149">The *dotnet* command has the following general syntax:</span></span>

```console
dotnet [verb] [arguments]
```

<span data-ttu-id="03b9b-150">動詞は実行するアクションを表します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-150">The verb represents the action to execute.</span></span> <span data-ttu-id="03b9b-151">引数は、動詞を実行するために必要な入力引数の一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-151">The arguments represent the list of input arguments that the verb requires in order to execute.</span></span>

<span data-ttu-id="03b9b-152">*dotnet* の使用に関するヘルプを取得し、利用可能なすべての*動詞*を一覧表示し、またその他の関連情報を得るには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-152">To get help on the *dotnet* usage and list all of the *verbs* available, and other related information, type the following command:</span></span>

```console
dotnet --help
```

<span data-ttu-id="03b9b-153">このコマンドでは次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-153">This command displays the following:</span></span>

```console
.NET Command Line Tools (2.1.302)
Usage: dotnet [runtime-options] [path-to-application]
Usage: dotnet [sdk-options] [command] [arguments] [command-options]

path-to-application:
  The path to an application .dll file to execute.

SDK commands:
  new              Initialize .NET projects.
  restore          Restore dependencies specified in the .NET project.
  run              Compiles and immediately executes a .NET project.
  build            Builds a .NET project.
  publish          Publishes a .NET project for deployment (including the runtime).
  test             Runs unit tests using the test runner specified in the project.

...
```

<span data-ttu-id="03b9b-154">**SDK コマンド**の下には、.NET Core SDK に対して実行できるコマンドの完全な一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-154">Under the **SDK commands**, you can see the entire list of commands that you can execute against the .NET Core SDK.</span></span>

<span data-ttu-id="03b9b-155">常に最も役立つコマンドは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="03b9b-155">The most useful commands at all times are the following:</span></span>

- <span data-ttu-id="03b9b-156">**dotnet new**: このコマンドは新しい .NET アプリケーションをスキャフォールディング/生成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-156">**dotnet new**: This command is used to scaffold/generate a new .NET application.</span></span>

- <span data-ttu-id="03b9b-157">**dotnet restore**: このコマンドは、アプリケーションによって参照されているすべてのパッケージを復元/ダウンロードするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-157">**dotnet restore**: This command is used to restore/download all packages that are referenced by the application.</span></span>

- <span data-ttu-id="03b9b-158">**dotnet run**: このコマンドは、.NET アプリケーションを実行するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-158">**dotnet run**: This command is used to run your .NET application.</span></span>

<span data-ttu-id="03b9b-159">ここで、特定のコマンドを使用する方法について支援を得るには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-159">Now, to get assistance on how to use a specific command, you can type the following:</span></span>

```console
dotnet run --help
```

<span data-ttu-id="03b9b-160">このコマンドの結果は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-160">This command results in:</span></span>

```console
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.
  -n, --name          The name for the output being created. If no name is specified, the name of the current directory is used.
  -o, --output        Location to place the generated output.
  -i, --install       Installs a source or a template pack.
  -u, --uninstall     Uninstalls a source or a template pack.
  --nuget-source      Specifies a NuGet source to use during install.
  --type              Filters templates based on available types. Predefined values are "project", "item" or "other".
  --force             Forces content to be generated even if it would change existing files.
  -lang, --language   Filters templates based on language and specifies the language of the template to create.


Templates                                         Short Name         Language          Tags
----------------------------------------------------------------------------------------------------------------------------
Console Application                               console            [C#], F#, VB      Common/Console
Class library                                     classlib           [C#], F#, VB      Common/Library
...

Razor Page                                        page               [C#]              Web/ASP.NET
MVC ViewImports                                   viewimports        [C#]              Web/ASP.NET
MVC ViewStart                                     viewstart          [C#]              Web/ASP.NET
ASP.NET Core Empty                                web                [C#], F#          Web/Empty
ASP.NET Core Web App (Model-View-Controller)      mvc                [C#], F#          Web/MVC
ASP.NET Core Web App                              razor              [C#]              Web/MVC/Razor Pages
ASP.NET Core with Angular                         angular            [C#]              Web/MVC/SPA
...

Solution File                                     sln                                  Solution

Examples:
    dotnet new mvc --auth Individual
    dotnet new webapi
    dotnet new --help
```

<span data-ttu-id="03b9b-161">このコマンドでは、`dotnet new` コマンドで使用できるすべての利用可能なオプションが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-161">This command lists all the available options that you can use with the `dotnet new` command.</span></span> <span data-ttu-id="03b9b-162">また、次の .NET アプリケーションを生成するために使用できるすべての利用可能なプロジェクト テンプレートが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-162">Also, it lists all the available project templates that you can use to generate your next .NET application.</span></span> <span data-ttu-id="03b9b-163">最後のセクションでは、コマンドを使用して新しい .NET アプリケーションを生成する方法の例を示します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-163">Finally, a section shows examples on how to use the command to generate a new .NET application.</span></span>

<span data-ttu-id="03b9b-164">.NET CLI で利用可能なコマンドで `--help` 引数を使用することで、他のコマンドについて学習できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-164">You can learn the rest of the commands by using the `--help` argument for any command available in the .NET CLI.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="03b9b-165">Linux 環境への ASP.NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="03b9b-165">ASP.NET Core installation on Linux environment</span></span>

<span data-ttu-id="03b9b-166">ASP.NET Core の最新かつ最高のバージョンは、バージョン 2.1 です。</span><span class="sxs-lookup"><span data-stu-id="03b9b-166">The latest and greatest version of ASP.NET Core is v2.1.</span></span> <span data-ttu-id="03b9b-167">一般に、ASP.NET Core は .NET Core SDK に含まれています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-167">In general, ASP.NET Core ships as part of the .NET Core SDK.</span></span> <span data-ttu-id="03b9b-168">そのため、ご利用のコンピューター上に .NET Core SDK をインストールすることで、ASP.NET Core Web アプリを含む、.NET Core アプリケーションでの開発を開始できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-168">Therefore, by installing the .NET Core SDK on your machine, you can start developing with any .NET Core application, including ASP.NET Core web apps.</span></span>

<span data-ttu-id="03b9b-169">このデモでは、VMware Workstation Player を利用して、Windows 10 上で実行されている Ubuntu 18.04 OS を使用します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-169">For this demonstration, I am using an Ubuntu 18.04 OS running on Windows 10, with the help of VMware Workstation Player.</span></span>

<span data-ttu-id="03b9b-170">.NET Core SDK をダウンロードするには、[.NET のダウンロード](https://www.microsoft.com/net/download) ホーム ページにアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-170">To download the .NET Core SDK, you need to visit the [.NET Downloads](https://www.microsoft.com/net/download) home page.</span></span> <span data-ttu-id="03b9b-171">ページには、さまざまなプラットフォーム (Windows、Linux、および macOS) で .NET Core SDK をダウンロードするためのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-171">The page contains links to download the .NET Core SDK on different platforms: Windows, Linux, and macOS.</span></span>

<span data-ttu-id="03b9b-172">**[Linux]** タブを見つけてクリックします。次の 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-172">Locate and click on the **Linux** tab. You have two options:</span></span>

- <span data-ttu-id="03b9b-173">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="03b9b-173">.NET Core SDK</span></span>
- <span data-ttu-id="03b9b-174">.NET Core ランタイム</span><span class="sxs-lookup"><span data-stu-id="03b9b-174">.NET Core Runtime</span></span>

<span data-ttu-id="03b9b-175">.NET Core ランタイムは既に .NET Core SDK 内に含まれています。</span><span class="sxs-lookup"><span data-stu-id="03b9b-175">The .NET Core Runtime is already contained inside the .NET Core SDK.</span></span> <span data-ttu-id="03b9b-176">ランタイムはアプリケーションの実行に使用されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-176">The runtime is used to run an application.</span></span> <span data-ttu-id="03b9b-177">開発、テスト、実行、および公開のためのツールはすべて .NET Core SDK 内にあります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-177">All the tools to develop, test, run, and publish are inside the .NET Core SDK.</span></span> <span data-ttu-id="03b9b-178">したがって、**[.NET Core SDK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03b9b-178">Therefore, you will click on **.NET Core SDK**.</span></span>

<span data-ttu-id="03b9b-179">Microsoft Web サイトからダウンロード ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-179">The Microsoft website redirects you to the download page.</span></span> <span data-ttu-id="03b9b-180">そこで、**[Linux ディストリビューション]** を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="03b9b-180">There, you need to select the **Linux Distribution**.</span></span> <span data-ttu-id="03b9b-181">ここでは **[Ubuntu 18.04]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="03b9b-181">In my case, I will select **Ubuntu 18.04**.</span></span> <span data-ttu-id="03b9b-182">選択すると、Ubuntu 18.04 上に .NET Core SDK をインストールする手順が自動的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-182">Automatically, upon selection, the instructions on how to install .NET Core SDK on Ubuntu 18.04 is displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="03b9b-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="03b9b-183">Summary</span></span>

<span data-ttu-id="03b9b-184">Web アプリケーションをビルドするようにした場合は、多くの言語とフレームワークを選択できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-184">When deciding to build a web application, you have a choice of many languages and frameworks.</span></span> <span data-ttu-id="03b9b-185">Azure では、Node.js、PHP、.NET Core などのさまざまな種類のアプリケーションをホストできるようにすることで、この選択をより簡単に行えるようにします。</span><span class="sxs-lookup"><span data-stu-id="03b9b-185">Azure tries to make this choice easier by allowing you to host different types of applications, like Node.js, PHP, or .NET Core.</span></span> <span data-ttu-id="03b9b-186">これにより、Web ホストのニーズに合わせて変更するのではなく、最も使い慣れている言語とフレームワークを使用できます。</span><span class="sxs-lookup"><span data-stu-id="03b9b-186">This allows you to use the languages and frameworks that you're most comfortable with instead of changing to meet the needs of your web host.</span></span>
