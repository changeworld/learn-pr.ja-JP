<span data-ttu-id="81fbb-101">::: zone pivot="csharp" .NET Core アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。</span><span class="sxs-lookup"><span data-stu-id="81fbb-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="81fbb-102">JSON ファイルで構成を管理するために必要なプラミングを追加することから始めます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="81fbb-103">JSON 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="81fbb-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="81fbb-104">プロジェクトの正しい作業ディレクトリで操作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="81fbb-104">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="81fbb-105">コマンド ラインで `touch` ツールを使用して、**appsettings.json** という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="81fbb-106">対話型エディターを使用してプロジェクトを開きます。ローカルで作業している場合は、任意のエディターを使用します。拡張可能なクロスプラットフォーム IDE である **Visual Studio Code** をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="81fbb-106">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="81fbb-107">次のコマンドは、Cloud Shell エディター用ですが、VS Code でもほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="81fbb-107">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="81fbb-108">エディターで **appsettings.json** ファイルを選択し、次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-108">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="81fbb-109">ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-109">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. <span data-ttu-id="81fbb-110">次に、プロジェクト ファイル (**PhotoSharingApp.csproj**) を選択して、エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-110">Next, select the project file (**PhotoSharingApp.csproj**) to open it in the editor.</span></span>

1. <span data-ttu-id="81fbb-111">プロジェクトに新しいファイルを含めるために次の構成ブロックを追加し、それを出力フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="81fbb-111">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="81fbb-112">これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-112">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="81fbb-113">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-113">Save the file.</span></span> <span data-ttu-id="81fbb-114">(これは必ず行ってください。そうしないと、以下のパッケージを追加するときの変更内容が失われます。)</span><span class="sxs-lookup"><span data-stu-id="81fbb-114">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="81fbb-115">JSON 構成ファイルを読み込むためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="81fbb-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="81fbb-116">.NET Core アプリケーションでは、JSON 構成ファイルを読み取るために追加の NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="81fbb-116">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="81fbb-117">ウィンドウのコマンド プロンプト セクションで、**Microsoft.Extensions.Configuration.Json** NuGet パッケージへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-117">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="81fbb-118">構成ファイルを読み取るためのコードを追加する</span><span class="sxs-lookup"><span data-stu-id="81fbb-118">Add code to read the configuration file</span></span>

<span data-ttu-id="81fbb-119">読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-119">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="81fbb-120">エディターで **Program.cs** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-120">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="81fbb-121">ファイルの一番上に **using System;** 行があります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-121">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="81fbb-122">その行の下に次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-122">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="81fbb-123">**Main** メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-123">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="81fbb-124">このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-124">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="81fbb-125">**Program.cs** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-125">Your **Program.cs** file should now look like the following:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();            
        }
    }
}
```

<span data-ttu-id="81fbb-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81fbb-126">::: zone-end</span></span>

<span data-ttu-id="81fbb-127">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="81fbb-127">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="81fbb-128">Node.js アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。</span><span class="sxs-lookup"><span data-stu-id="81fbb-128">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="81fbb-129">JavaScript ファイルから構成を管理するために必要なプラミングを追加することから始めます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-129">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="81fbb-130">.env 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="81fbb-130">Create a .env configuration file</span></span>

1. <span data-ttu-id="81fbb-131">プロジェクトの正しい作業ディレクトリで操作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="81fbb-131">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="81fbb-132">コマンド ラインで `touch` ツールを使用して、**.env** という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-132">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="81fbb-133">対話型エディターを使用してプロジェクトを開きます。ローカルで作業している場合は、任意のエディターを使用します。拡張可能なクロスプラットフォーム IDE である **Visual Studio Code** をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="81fbb-133">Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="81fbb-134">次のコマンドは、Cloud Shell エディター用ですが、VS Code でもほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="81fbb-134">The following commands are for the Cloud Shell editor, but are very similar to VS Code.</span></span>
    
    ```bash
    code .
    ```

1. <span data-ttu-id="81fbb-135">エディターで **.env** ファイルを選択し、次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-135">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="81fbb-136">ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-136">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="81fbb-137">**AZURE_STORAGE_CONNECTION_STRING** 値は、Storage API でアクセス キーを検索するために使用される、ハード コーディングされた環境変数です。</span><span class="sxs-lookup"><span data-stu-id="81fbb-137">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="81fbb-138">必要に応じて独自の名前を使用することもできますが、`BlobService` オブジェクトの作成時の名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-138">You can use your own name if you prefer - but you must supply the name to the when you create the `BlobService` object.</span></span>

1. <span data-ttu-id="81fbb-139">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-139">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="81fbb-140">環境構成ファイルを読み取るためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="81fbb-140">Add support to read an environment configuration file</span></span>

<span data-ttu-id="81fbb-141">Node.js アプリに **.env** ファイルから読み取るためのサポートを含めるには、**dotenv** パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-141">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="81fbb-142">ウィンドウのコマンド プロンプト セクションで、依存関係を *dotenv*\* パッケージに追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-142">In the command prompt section of the window, add a dependency to the  *dotenv*\* package.</span></span>

    ```bash
    node install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="81fbb-143">構成ファイルを読み取るためのコードを追加する</span><span class="sxs-lookup"><span data-stu-id="81fbb-143">Add code to read the configuration file</span></span>

<span data-ttu-id="81fbb-144">構成の読み取りを有効にするために必要なライブラリが追加されたので、アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-144">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="81fbb-145">エディターで *index.js*\* を選択します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-145">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="81fbb-146">ファイルの一番上に **#!/usr/bin/env node** 行があります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-146">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="81fbb-147">その行の下に、**dotenv** パッケージを読み込むための `require` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-147">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="81fbb-148">これにより、**.env** ファイル内で定義された環境変数がプログラムで使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="81fbb-148">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="81fbb-149">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81fbb-149">::: zone-end</span></span>

## <a name="add-the-connection-string-to-the-configuration-file"></a><span data-ttu-id="81fbb-150">構成ファイルに接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="81fbb-150">Add the connection string to the configuration file</span></span>

<span data-ttu-id="81fbb-151">次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-151">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="81fbb-152">[Azure Portal](https://portal.azure.com/?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="81fbb-152">Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="81fbb-153">ストレージ アカウントに移動します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-153">Navigate to your storage account.</span></span> <span data-ttu-id="81fbb-154">**[すべてのリソース]** セクションを使用して、ストレージ アカウントを検索することができます。また、ポータル ウィンドウの上部にある_検索ボックス_から名前で検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-154">You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window.</span></span> 

1. <span data-ttu-id="81fbb-155">ポータルでストレージ アカウントの **[アクセス キー]** ブレードを選択します。</span><span class="sxs-lookup"><span data-stu-id="81fbb-155">Select the **Access Keys** blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="81fbb-156">**key1** 接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="81fbb-156">Copy the **key1** Connection string.</span></span>

1. <span data-ttu-id="81fbb-157">接続文字列構成変数の値としてポータルからコピーしたアクセス キーの内容を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-157">Paste in the contents of the access key you copied from the portal as the value for the connection string configuration variable.</span></span>

<span data-ttu-id="81fbb-158">これで構成が次のようになるはずです。</span><span class="sxs-lookup"><span data-stu-id="81fbb-158">Your configuration should now look similar to the following:</span></span>

<span data-ttu-id="81fbb-159">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="81fbb-159">::: zone pivot="csharp"</span></span>
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
<span data-ttu-id="81fbb-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81fbb-160">::: zone-end</span></span>

<span data-ttu-id="81fbb-161">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="81fbb-161">::: zone-pivot="javascript"</span></span>
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
<span data-ttu-id="81fbb-162">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="81fbb-162">::: zone-end</span></span>

<span data-ttu-id="81fbb-163">これですべて接続したので、ストレージ アカウントを使用するためのコードの追加を開始できます。</span><span class="sxs-lookup"><span data-stu-id="81fbb-163">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>