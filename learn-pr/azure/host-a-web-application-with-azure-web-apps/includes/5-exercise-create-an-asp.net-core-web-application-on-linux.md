<span data-ttu-id="b68cb-101">この演習では、Ubuntu マシン上で .NET CLI を使用して ASP.NET Core Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-101">In this exercise, you will create an ASP.NET Core web app using the .NET CLI on an Ubuntu machine.</span></span>

## <a name="aspnet-core-installation-on-linux-environment"></a><span data-ttu-id="b68cb-102">Linux 環境への ASP.NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="b68cb-102">ASP.NET Core installation on Linux environment</span></span>

<span data-ttu-id="b68cb-103">Microsoft の [.NET ダウンロード ページ](https://www.microsoft.com/net/download)にアクセスし、.NET Core SDK のページで説明されているのと同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="b68cb-103">Visit the Microsoft [.NET Downloads page](https://www.microsoft.com/net/download) and follow the same steps mentioned on the .NET Core SDK page.</span></span> <span data-ttu-id="b68cb-104">以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b68cb-104">They are below.</span></span> <span data-ttu-id="b68cb-105">コマンドを実行するには、次のような新しい**ターミナル** インスタンスを開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-105">In order to execute the commands, you need to open a new **Terminal** instance, something similar to this:</span></span>

![ターミナルのインスタンス](../media-draft/5-terminal-instance.PNG)

### <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="b68cb-107">Microsoft のキーとフィードを登録する</span><span class="sxs-lookup"><span data-stu-id="b68cb-107">Register Microsoft key and feed</span></span>

<span data-ttu-id="b68cb-108">.NET をインストールする前に、Microsoft キーを登録し、製品のリポジトリを登録し、必要な依存関係をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-108">Before installing .NET, you'll need to register the Microsoft key, register the product repository, and install the required dependencies.</span></span> <span data-ttu-id="b68cb-109">これは、マシンごとに 1 回だけ行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-109">This only needs to be done once per machine.</span></span>

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-sdk"></a><span data-ttu-id="b68cb-110">.NET SDK をインストールする</span><span class="sxs-lookup"><span data-stu-id="b68cb-110">Install the .NET SDK</span></span>

<span data-ttu-id="b68cb-111">インストールに使用可能な製品を更新し、.NET SDK をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b68cb-111">Update the products available for installation, then install the .NET SDK.</span></span>

<span data-ttu-id="b68cb-112">コマンド プロンプトで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-112">At your command prompt, run the following commands:</span></span>

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

<span data-ttu-id="b68cb-113">インストール プロセスの間に、アカウント パスワードの入力を求められる場合がします。</span><span class="sxs-lookup"><span data-stu-id="b68cb-113">During the installation process, you might be asked to enter your account password.</span></span> <span data-ttu-id="b68cb-114">パスワードを入力し、Enter キーを押して続行します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-114">Simply provide your password and hit Enter to continue.</span></span>

<span data-ttu-id="b68cb-115">インストールを確認するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-115">To verify the installation, type the following:</span></span>

```console
dotnet --version
```

<span data-ttu-id="b68cb-116">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b68cb-116">And the output displayed is:</span></span>

```console
2.1.302
```

<span data-ttu-id="b68cb-117">.NET Core SDK をインストールしたので、ASP.NET Core もインストールされています。</span><span class="sxs-lookup"><span data-stu-id="b68cb-117">Now that you have the .NET Core SDK installed, you also have the ASP.NET Core installed.</span></span> <span data-ttu-id="b68cb-118">.NET CLI と学習したコマンドを使用して、新しい ASP.NET Core プロジェクトを作成する方法を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="b68cb-118">Let's see together how you can make use of the .NET CLI to create a new ASP.NET Core project using the commands we just learned.</span></span>

## <a name="open-a-terminal-window"></a><span data-ttu-id="b68cb-119">ターミナル ウィンドウを開く</span><span class="sxs-lookup"><span data-stu-id="b68cb-119">Open a Terminal window</span></span>

<span data-ttu-id="b68cb-120">最初に、ターミナル ウィンドウを探して開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-120">First, you need to locate and open the Terminal window.</span></span> <span data-ttu-id="b68cb-121">ターミナル ウィンドウの役割は Windows コマンド プロンプト ウィンドウと似ており、コマンドを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="b68cb-121">The Terminal window allows you to execute commands, and has a similar role to the Windows Command Prompt window.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="b68cb-122">新しい Web プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="b68cb-122">Create a new web project</span></span>

<span data-ttu-id="b68cb-123">.NET CLI ツールの中心になるのは、*dotnet* ドライバー ツールです。</span><span class="sxs-lookup"><span data-stu-id="b68cb-123">The heart of the .NET CLI tools is the *dotnet* driver tool.</span></span> <span data-ttu-id="b68cb-124">このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-124">Using this command, you will create a new ASP.NET Core web project.</span></span>

<span data-ttu-id="b68cb-125">新しい ASP.NET Core MVC アプリケーションを作成するには、次のコマンドを入力することだけが必要です。</span><span class="sxs-lookup"><span data-stu-id="b68cb-125">To create a new ASP.NET Core MVC application, you only need to type the following commands:</span></span>

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

<span data-ttu-id="b68cb-126">上のコマンドを実行すると、*Documents* ルート フォルダーに移動して、新しいディレクトリ/フォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b68cb-126">The above commands navigate to the *Documents* root folder, and then create a new directory / folder.</span></span> <span data-ttu-id="b68cb-127">その後、そのディレクトリ/フォルダー内に移動し、最後に、.NET CLI コマンドを発行して、すべてのパッケージが復元された新しい ASP.NET MVC アプリケーションを生成します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-127">Then, you go inside that directory/folder, and finally, you issue the .NET CLI command to generate a new ASP.NET MVC application with all the packages restored:</span></span>

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

<span data-ttu-id="b68cb-128">ここで行う必要があるのは、次のコマンドを発行してアプリケーションを実行することだけです。</span><span class="sxs-lookup"><span data-stu-id="b68cb-128">All you have to do now is run the application by issuing this command:</span></span>

```console
dotnet run
```

<span data-ttu-id="b68cb-129">"*ターミナル*" では、*dotnet* コマンドによりアプリの実行に役立つ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b68cb-129">In the *terminal*, the *dotnet* command displays some useful information for the running app:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b68cb-130">出力ではアプリ開始後の状況が説明されます。アプリケーションは実行するとポート 5001 および 5002 (HTTPS を介して) をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="b68cb-130">The output describes the situation after starting your app: the application is running and listening at port 5001 and 5002 (via HTTPS).</span></span>

<span data-ttu-id="b68cb-131">開発モードのコンピューターで HTTPS をローカルに実行するには、**自己署名証明書**を作成して信頼する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-131">In order to run HTTPS locally on the machine in development mode, you need to create and trust a **self-signed certificate**.</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="b68cb-132">自己署名証明書を作成する</span><span class="sxs-lookup"><span data-stu-id="b68cb-132">Create a self-signed certificate</span></span>

<span data-ttu-id="b68cb-133">開発用の自己署名証明書を生成するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-133">Run the command below to generate a development self-signed certificate:</span></span>

```console
dotnet dev-certs https -v
```

<span data-ttu-id="b68cb-134">このコマンドを実行すると、次の出力が返されます。</span><span class="sxs-lookup"><span data-stu-id="b68cb-134">Running the command returns the following:</span></span>

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a><span data-ttu-id="b68cb-135">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="b68cb-135">Run the application</span></span>

<span data-ttu-id="b68cb-136">このデモでは、Firefox ブラウザーを使っています。</span><span class="sxs-lookup"><span data-stu-id="b68cb-136">For this demonstration, I am using the Firefox browser.</span></span>

<span data-ttu-id="b68cb-137">ブラウザーを開き、アドレス `http://localhost:5001` を入力して、アプリケーションに移動します。</span><span class="sxs-lookup"><span data-stu-id="b68cb-137">Open the browser and type in the address `http://localhost:5001` to go to the application.</span></span>

![ASP.NET Core MVC の既定のテンプレート](../media-draft/5-asp-core-mvc-default-template.PNG)

> <span data-ttu-id="b68cb-139">開発用の自己署名証明書は Firefox では検証できないため、アプリケーションの URL に対する**例外を追加する**必要があります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-139">You need to **add an exception** for the application's URL because the development self-signed certificate couldn't be verified by Firefox.</span></span>

<span data-ttu-id="b68cb-140">Web アプリが正常に起動して稼働するようになります。</span><span class="sxs-lookup"><span data-stu-id="b68cb-140">The web app is up and running successfully!</span></span>
