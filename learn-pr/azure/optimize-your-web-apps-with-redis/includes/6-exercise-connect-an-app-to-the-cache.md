<span data-ttu-id="b6fc4-101">Azure で Redis Cache を作成したので、それを使用するアプリケーションを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-101">Now that we have a Redis cache created in Azure, let's create an application to use it.</span></span> <span data-ttu-id="b6fc4-102">Azure portal から取得した接続文字列情報があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-102">Make sure you have your connection string information from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="b6fc4-103">統合された Cloud Shell は右側に用意されています。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-103">The integrated Cloud Shell is available on the right.</span></span> <span data-ttu-id="b6fc4-104">そのコマンド プロンプトを使用して、ここでビルドするサンプル コードを作成し、実行できます。また、開発環境をセットアップしている場合は、以下の手順をローカルで実行できます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-104">You can use that command prompt to create and run the example code we are building here, or perform these steps locally if you have a development environment setup.</span></span> <span data-ttu-id="b6fc4-105">Cloud Shell を使用する場合は、選択を求めるメッセージが表示されたら Bash シェルを選択してください。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-105">If you decide to use the Cloud Shell, please select the Bash shell if you are prompted for a selection.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="b6fc4-106">コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-106">Create a Console Application</span></span>

<span data-ttu-id="b6fc4-107">Redis 実装に集中できるように、簡単なコンソール アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-107">We'll use a simple Console Application so we can focus on the Redis implementation.</span></span>

1. <span data-ttu-id="b6fc4-108">Cloud Shell で、新しい .NET Core コンソール アプリケーションを作成し、"SportsStatsTracker" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-108">In the Cloud Shell, create a new .NET Core Console Application, name it "SportsStatsTracker"</span></span>

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. <span data-ttu-id="b6fc4-109">これにより、プロジェクトのフォルダーが作成され、現在のディレクトリが変更されます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-109">This will create a folder for the project, go ahead and change the current directory.</span></span>

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a><span data-ttu-id="b6fc4-110">接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-110">Add the connection string</span></span>

<span data-ttu-id="b6fc4-111">Azure portal から取得した接続文字列をコードに追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-111">Let's add the connection string we got from the Azure portal into the code.</span></span> <span data-ttu-id="b6fc4-112">このような資格情報は、ソース コードに保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-112">Never store credentials like this in your source code.</span></span> <span data-ttu-id="b6fc4-113">このサンプルをシンプルにするために、構成ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-113">To keep this sample simple, we're going to use a configuration file.</span></span> <span data-ttu-id="b6fc4-114">Azure のサーバー側アプリケーションに適した方法は、Azure Key Vault と証明書を使用することです。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-114">A better approach for a server-side application in Azure would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="b6fc4-115">新しい **appsettings.json** ファイルを作成してプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-115">Create a new **appsettings.json** file to add to the project.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="b6fc4-116">プロジェクト フォルダーで「`code .`」と入力してコード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-116">Open the code editor by typing `code .` in the project folder.</span></span> <span data-ttu-id="b6fc4-117">ローカルで作業している場合は、**Visual Studio Code** を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-117">If you are working locally, we recommend using **Visual Studio Code**.</span></span> <span data-ttu-id="b6fc4-118">ほとんどの場合、ここでの手順は Visual Studio Code でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-118">The steps here will mostly align with it's usage.</span></span>

1. <span data-ttu-id="b6fc4-119">エディターで **appsettings.json** ファイルを選択し、次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-119">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="b6fc4-120">接続文字列を設定の**値**に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-120">Paste your connection string into the **value** of the setting.</span></span>

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. <span data-ttu-id="b6fc4-121">ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-121">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

1. <span data-ttu-id="b6fc4-122">次の構成ブロックを追加してプロジェクトに新しいファイルを含め、それを出力フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-122">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="b6fc4-123">これにより、アプリをコンパイル/ビルドしたときに、アプリ構成ファイルが出力ディレクトリに配置されます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-123">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. <span data-ttu-id="b6fc4-124">ファイルを保存します </span><span class="sxs-lookup"><span data-stu-id="b6fc4-124">Save the file.</span></span> <span data-ttu-id="b6fc4-125">(これを必ず行ってください。そうしないと、下記のパッケージを追加したときに変更内容が失われます)。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-125">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="b6fc4-126">JSON 構成ファイルを読み取るためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-126">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="b6fc4-127">.NET Core アプリケーションには、JSON 構成ファイルを読み取るための追加の NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-127">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="b6fc4-128">ウィンドウのコマンド プロンプト セクションで、**Microsoft.Extensions.Configuration.Json** NuGet パッケージへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-128">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="b6fc4-129">構成ファイルを読み取るコードを追加する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-129">Add code to read the configuration file</span></span>

<span data-ttu-id="b6fc4-130">構成の読み取りを有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-130">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="b6fc4-131">エディターで **Program.cs** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-131">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="b6fc4-132">ファイルの先頭に **using System;** 行があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-132">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="b6fc4-133">その行の下に次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-133">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="b6fc4-134">**Main** メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-134">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="b6fc4-135">このコードにより、**appsettings.json** ファイルから読み取る構成システムが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-135">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

<span data-ttu-id="b6fc4-136">**Program.cs** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-136">Your **Program.cs** file should now look like the following:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a><span data-ttu-id="b6fc4-137">構成から接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-137">Get the connection string from configuration</span></span>

1. <span data-ttu-id="b6fc4-138">**Program.cs** の **Main** メソッドの末尾で、新しい **config** 変数を使用して接続文字列を取得し、**connectionString** という名前の新しい変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-138">In **Program.cs**, at the end of the **Main** method, use the new **config** variable to retrieve the connection string and store it in a new variable named **connectionString**.</span></span>
    - <span data-ttu-id="b6fc4-139">**config** 変数には、**appSettings.json** ファイルから取得する文字列を渡すことができるインデクサーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-139">The **config** variable has an indexer where you can pass in a string to retrieve from your **appSettings.json** file.</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a><span data-ttu-id="b6fc4-140">Redis Cache .NET クライアントのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-140">Add support for the Redis cache .NET client</span></span>

<span data-ttu-id="b6fc4-141">次に、.NET 用の **StackExchange.Redis** クライアントを使用するようにコンソール アプリケーションを構成しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-141">Next, let's configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="b6fc4-142">Cloud Shell エディターの下部にあるコマンド プロンプトを使用して、**StackExchange.Redis** NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-142">Add the **StackExchange.Redis** NuGet package to the project using the command prompt at the bottom of the Cloud Shell editor.</span></span>

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. <span data-ttu-id="b6fc4-143">エディターで **Program.cs** を選択し、**StackExchange.Redis** 名前空間の `using` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-143">Select **Program.cs** in the editor and add a `using` for the namespace **StackExchange.Redis**</span></span>

    ```csharp
    using StackExchange.Redis;
    ```
    
<span data-ttu-id="b6fc4-144">インストールが完了すると、Redis Cache クライアントをプロジェクトで使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-144">Once the installation is completed, the Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="b6fc4-145">キャッシュに接続する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-145">Connect to the cache</span></span>

<span data-ttu-id="b6fc4-146">キャッシュに接続するコードを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-146">Let's add the code to connect to the cache.</span></span>

1. <span data-ttu-id="b6fc4-147">エディターで **Program.cs** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-147">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="b6fc4-148">`ConnectionMultiplexer.Connect` を使用し、これに接続文字列を渡して `ConnectionMultiplexer` を作成します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-148">Create a `ConnectionMultiplexer` using `ConnectionMultiplexer.Connect` by passing it your connection string.</span></span> <span data-ttu-id="b6fc4-149">戻り値に **cache** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-149">Name the returned value **cache**.</span></span>

1. <span data-ttu-id="b6fc4-150">作成された接続は "_破棄可能_" であるため、`using` ブロックにラップします。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-150">Since the created connection is _disposable_, wrap it in a `using` block.</span></span> <span data-ttu-id="b6fc4-151">コードは次のような内容になります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-151">Your code should look something like:</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> <span data-ttu-id="b6fc4-152">Azure Redis Cache への接続は、`ConnectionMultiplexer` クラスによって管理されます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-152">The connection to Azure Redis Cache is managed by the `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="b6fc4-153">このクラスは、クライアント アプリケーション全体で共有して再利用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-153">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="b6fc4-154">操作ごとに新しい接続を作成するのは望ましく_ありません_。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-154">We do _not_ want to create a new connection for each operation.</span></span> <span data-ttu-id="b6fc4-155">代わりに、接続をクラスのフィールドとして保存し、各操作で再利用します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-155">Instead, we want to store it off as a field in our class and reuse it for each operation.</span></span> <span data-ttu-id="b6fc4-156">ここでは、**Main** メソッドでのみ使用しますが、実稼働アプリケーションでは、クラス フィールド (シングルトン) に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-156">Here we are only going to use it in the **Main** method, but in a production application, it should be stored in a class field, or a singleton.</span></span>

## <a name="add-a-value-to-the-cache"></a><span data-ttu-id="b6fc4-157">キャッシュに値を追加する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-157">Add a value to the cache</span></span>

<span data-ttu-id="b6fc4-158">接続が作成されたので、キャッシュに値を追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-158">Now that we have the connection, let's add a value to the cache.</span></span>

1. <span data-ttu-id="b6fc4-159">接続が作成された後の `using` ブロック内で、`GetDatabase` メソッドを使用して `IDatabase` インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-159">Inside the `using` block after the connection has been created, use the `GetDatabase` method to retrieve an `IDatabase` instance.</span></span>

1. <span data-ttu-id="b6fc4-160">`IDatabase` オブジェクトに対して `StringSet` を呼び出して、キー "test:key" を値 "some value" に設定します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-160">Call `StringSet` on the `IDatabase` object to set the key "test:key" to the value "some value".</span></span>
    - <span data-ttu-id="b6fc4-161">`StringSet` からの戻り値は、キーが追加されたかどうかを示す `bool` です。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-161">the return value from `StringSet` is a `bool` indicating whether the key was added.</span></span>

1. <span data-ttu-id="b6fc4-162">`StringSet` からの戻り値をコンソールに表示します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-162">Display the return value from `StringSet` onto the console.</span></span>

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a><span data-ttu-id="b6fc4-163">キャッシュから値を取得する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-163">Get a value from the cache</span></span>

1. <span data-ttu-id="b6fc4-164">次に、`StringGet` を使用して値を取得します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-164">Next, retrieve the value using `StringGet`.</span></span> <span data-ttu-id="b6fc4-165">これはキーを取得して値を返します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-165">This takes the key to retrieve and returns the value.</span></span>

1. <span data-ttu-id="b6fc4-166">戻り値を出力します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-166">Output the returned value.</span></span>

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="b6fc4-167">コードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-167">Your code should look like this:</span></span>

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. <span data-ttu-id="b6fc4-168">アプリケーションを実行して結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-168">Run the application to see the result.</span></span> <span data-ttu-id="b6fc4-169">エディターの下のターミナル ウィンドウに「`dotnet run`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-169">Type `dotnet run` into the terminal window below the editor.</span></span> <span data-ttu-id="b6fc4-170">必ずプロジェクト フォルダーに移動してください。そうしないと、ビルドして実行するコードを見つけることはできません。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-170">Make sure you are in the project folder or it won't find your code to build and run.</span></span>
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a><span data-ttu-id="b6fc4-171">メソッドの非同期バージョンを使用する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-171">Use the async versions of the methods</span></span>

<span data-ttu-id="b6fc4-172">キャッシュから値を取得および設定できましたが、古い同期バージョンを使用しています。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-172">We have been able to get and set values from the cache, but we are using the older synchronous versions.</span></span> <span data-ttu-id="b6fc4-173">サーバー側アプリケーションでは、同期バージョンを使用すると、スレッドを効率的に使用できません。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-173">In server-side applications, these are not an efficient use of our threads.</span></span> <span data-ttu-id="b6fc4-174">代わりに、メソッドの_非同期_バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-174">Instead, we want to use the _asynchronous_ versions of the methods.</span></span> <span data-ttu-id="b6fc4-175">非同期バージョンはすべて末尾が **Async** であるため、簡単に見分けることができます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-175">You can easily spot them - they all end in **Async**.</span></span>

<span data-ttu-id="b6fc4-176">これらのメソッドを操作しやすくするために、C# の `async` および `await` キーワードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-176">To make these methods easy to work with, we can use the C# `async` and `await` keywords.</span></span> <span data-ttu-id="b6fc4-177">ただし、これらのキーワードを **Main** メソッドに適用するには、C# 7.1 "_以上_" を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-177">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="b6fc4-178">C# 7.1 に切り替える</span><span class="sxs-lookup"><span data-stu-id="b6fc4-178">Switch to C# 7.1</span></span>

<span data-ttu-id="b6fc4-179">C# 7.1 より前では、C# の `async` および `await` キーワードは **Main** メソッドで有効なキーワードではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-179">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="b6fc4-180">**.csproj** ファイルでフラグを使用することで、そのコンパイラに簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-180">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="b6fc4-181">エディターで **SportsStatsTracker.csproj** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-181">Open the **SportsStatsTracker.csproj** file in the editor.</span></span>

1. <span data-ttu-id="b6fc4-182">ビルド ファイル内の最初の `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-182">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="b6fc4-183">完了すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-183">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="b6fc4-184">async キーワードを適用する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-184">Apply the async keyword</span></span>

<span data-ttu-id="b6fc4-185">次に、`async` キーワードを **Main** メソッドに適用します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-185">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="b6fc4-186">次の 3 つのことを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-186">We will have to do three things.</span></span>

1. <span data-ttu-id="b6fc4-187">`async` キーワードを **Main** メソッド シグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-187">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="b6fc4-188">戻り値の型を `void` から `Task` に変更します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-188">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="b6fc4-189">`System.Threading.Tasks` を含めるための `using` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-189">Add a `using` statement to include `System.Threading.Tasks`.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a><span data-ttu-id="b6fc4-190">値を非同期に取得および設定する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-190">Get and set values asynchronously</span></span>

1. <span data-ttu-id="b6fc4-191">`StringSetAsync` メソッドと `StringGetAsync` メソッドを使用して、"counter" という名前のキーを設定および取得します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-191">Use the `StringSetAsync` and `StringGetAsync` methods to set and retrieve a key named "counter".</span></span> <span data-ttu-id="b6fc4-192">値を "100" に設定します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-192">Set the value to "100".</span></span>

1. <span data-ttu-id="b6fc4-193">`await` キーワードを適用して、各メソッドから結果を取得します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-193">Apply the `await` keyword to get the results from each method.</span></span>

1. <span data-ttu-id="b6fc4-194">同期バージョンで行ったように、コンソール ウィンドウに結果を出力します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-194">Output the results to the console window - just as you did with the synchronous versions.</span></span>

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="b6fc4-195">アプリケーションをもう一度実行します。アプリケーションは引き続き動作し、2 つの値を取得します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-195">Run the application again - it should still work and now have two values.</span></span>

#### <a name="increment-the-value"></a><span data-ttu-id="b6fc4-196">値を増分する</span><span class="sxs-lookup"><span data-stu-id="b6fc4-196">Increment the value</span></span>

1. <span data-ttu-id="b6fc4-197">`StringIncrementAsync` メソッドを使用して **counter** 値を増分します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-197">Use the `StringIncrementAsync` method to increment your **counter** value.</span></span> <span data-ttu-id="b6fc4-198">カウンターに追加する数値 **50** を渡します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-198">Pass the number **50** to add to the counter.</span></span>
    - <span data-ttu-id="b6fc4-199">このメソッドは、キー_と_ `long` または `double` を取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-199">Notice that the method takes the key _and_ either a `long` or `double`.</span></span>
    - <span data-ttu-id="b6fc4-200">渡されたパラメーターに応じて、`long` または `double` が返されます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-200">Depending on the parameters passed, it either returns a `long` or `double`.</span></span>

1. <span data-ttu-id="b6fc4-201">メソッドの結果をコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-201">Output the results of the method to the console.</span></span>

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a><span data-ttu-id="b6fc4-202">その他の操作</span><span class="sxs-lookup"><span data-stu-id="b6fc4-202">Other operations</span></span>

<span data-ttu-id="b6fc4-203">最後に、`ExecuteAsync` をサポートするその他のメソッドをいくつか実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-203">Finally, let's try executing a few additional methods with the `ExecuteAsync` support.</span></span>

1. <span data-ttu-id="b6fc4-204">サーバー接続をテストするには、"PING" を実行します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-204">Execute "PING" to test the server connection.</span></span> <span data-ttu-id="b6fc4-205">このメソッドは "PONG" で応答します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-205">It should respond with "PONG".</span></span>
1. <span data-ttu-id="b6fc4-206">データベース値をクリアするには、"FLUSHDB" を実行します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-206">Execute "FLUSHDB" to clear the database values.</span></span> <span data-ttu-id="b6fc4-207">このメソッドは "OK" で応答します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-207">It should respond with "OK".</span></span>

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a><span data-ttu-id="b6fc4-208">チャレンジ</span><span class="sxs-lookup"><span data-stu-id="b6fc4-208">Challenge</span></span>

<span data-ttu-id="b6fc4-209">チャレンジとして、オブジェクト型をシリアル化してキャッシュに格納してみてください。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-209">As a challenge, try serializing an object type to the cache.</span></span> <span data-ttu-id="b6fc4-210">基本的な手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-210">Here are the basic steps.</span></span>

1. <span data-ttu-id="b6fc4-211">いくつかのパブリック プロパティを持つ新しい `class` を作成します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-211">Create a new `class` with some public properties.</span></span> <span data-ttu-id="b6fc4-212">独自のクラス ("Person" や "Car" が一般的) を作成することも、前のユニットで提供された "GameStats" の例を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-212">You can invent one of your own ("Person" or "Car" are popular), or use the "GameStats" example given in the previous unit.</span></span>
1. <span data-ttu-id="b6fc4-213">`dotnet add package` を使用して、**Newtonsoft.Json** NuGet パッケージのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-213">Add support for the **Newtonsoft.Json** NuGet package using `dotnet add package`.</span></span>
1. <span data-ttu-id="b6fc4-214">`Newtonsoft.Json` 名前空間の `using` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-214">Add a `using` for the `Newtonsoft.Json` namespace.</span></span>
1. <span data-ttu-id="b6fc4-215">オブジェクトのいずれかを作成します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-215">Create one of your objects.</span></span>
1. <span data-ttu-id="b6fc4-216">`JsonConvert.SerializeObject` を使用してオブジェクトをシリアル化し、`StringSetAsync` を使用してキャッシュにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-216">Serialize it with `JsonConvert.SerializeObject` and use `StringSetAsync` to push it into the cache.</span></span>
1. <span data-ttu-id="b6fc4-217">`StringGetAsync` を使用してオブジェクトをキャッシュから取り出し、`JsonConvert.DeserializeObject<T>` を使用して逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="b6fc4-217">Get it back from the cache with `StringGetAsync` and then deserialize it with `JsonConvert.DeserializeObject<T>`.</span></span>

