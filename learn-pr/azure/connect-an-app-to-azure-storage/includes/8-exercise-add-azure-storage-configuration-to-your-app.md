<span data-ttu-id="9d196-101">::: zone pivot="csharp" .NET Core アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。</span><span class="sxs-lookup"><span data-stu-id="9d196-101">::: zone pivot="csharp" Let's add support to our .NET core application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="9d196-102">JSON ファイルで構成を管理するために必要なプラミングを追加することから始めます。</span><span class="sxs-lookup"><span data-stu-id="9d196-102">We'll start by adding the necessary plumbing to manage configuration in a JSON file.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="9d196-103">JSON 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="9d196-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="9d196-104">PhotoSharingApp ディレクトリに `cd` します (まだそのディレクトリに移動していない場合)。</span><span class="sxs-lookup"><span data-stu-id="9d196-104">`cd` to the PhotoSharingApp directory if you aren't already there.</span></span>

1. <span data-ttu-id="9d196-105">コマンド ラインで `touch` ツールを使用して、**appsettings.json** という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9d196-105">Use the `touch` tool on the command line to create a file named **appsettings.json**.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="9d196-106">エディターでプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="9d196-106">Open the project in an editor.</span></span> <span data-ttu-id="9d196-107">ローカルで作業している場合は、任意のエディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="9d196-107">If you are working locally, use your editor of choice.</span></span> <span data-ttu-id="9d196-108">拡張クロスプラットフォーム IDE である **Visual Studio Code** をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9d196-108">We recommend **Visual Studio Code**, which is an extensible cross-platform IDE.</span></span> <span data-ttu-id="9d196-109">Cloud Shell で作業する場合は、Cloud Shell エディターをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9d196-109">If you are working in the Cloud Shell, we recommend the Cloud Shell editor.</span></span> <span data-ttu-id="9d196-110">次のコマンドは、どちらでも機能します。</span><span class="sxs-lookup"><span data-stu-id="9d196-110">The following command works for both.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="9d196-111">エディターで **appsettings.json** ファイルを選択し、次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-111">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="9d196-112">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="9d196-112">Save the file.</span></span> <span data-ttu-id="9d196-113">Cloud Shell エディターで、ページの右上に一般的なファイル操作を含むメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="9d196-113">In the Cloud Shell editor, there is a menu in the top right corner that has common file operations.</span></span>

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. <span data-ttu-id="9d196-114">次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。</span><span class="sxs-lookup"><span data-stu-id="9d196-114">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="9d196-115">Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d196-115">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="9d196-116">そのコマンドから返された接続文字列をコピーし、**appsettings.json** ファイルの `"<value>"` をその接続文字列で置換します。</span><span class="sxs-lookup"><span data-stu-id="9d196-116">Copy the connection string that is returned from that command, and replace `"<value>"` in the **appsettings.json** file with this connection string.</span></span> <span data-ttu-id="9d196-117">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="9d196-117">Save the file.</span></span>

1. <span data-ttu-id="9d196-118">次に、プロジェクト ファイル (**PhotoSharingApp.csproj**) をエディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="9d196-118">Next, open the project file (**PhotoSharingApp.csproj**) in the editor.</span></span>

1. <span data-ttu-id="9d196-119">プロジェクトに新しいファイルを含めるために次の構成ブロックを追加し、それを出力フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="9d196-119">Add the following configuration block to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="9d196-120">これにより、アプリをコンパイル/ビルドしたときに、アプリ構成ファイルが出力ディレクトリに配置されます。</span><span class="sxs-lookup"><span data-stu-id="9d196-120">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

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

1. <span data-ttu-id="9d196-121">ファイルを保存します </span><span class="sxs-lookup"><span data-stu-id="9d196-121">Save the file.</span></span> <span data-ttu-id="9d196-122">(これを必ず行ってください。そうしないと、下記のパッケージを追加したときに変更内容が失われます)。</span><span class="sxs-lookup"><span data-stu-id="9d196-122">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="9d196-123">JSON 構成ファイルを読み取るためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="9d196-123">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="9d196-124">.NET Core アプリケーションには、JSON 構成ファイルを読み取るための追加の NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="9d196-124">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="9d196-125">ウィンドウのコマンド プロンプト セクションで、**Microsoft.Extensions.Configuration.Json** NuGet パッケージへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-125">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="9d196-126">構成ファイルを読み取るコードを追加する</span><span class="sxs-lookup"><span data-stu-id="9d196-126">Add code to read the configuration file</span></span>

<span data-ttu-id="9d196-127">構成の読み取りを有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-127">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="9d196-128">エディターで **Program.cs** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9d196-128">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="9d196-129">ファイルの先頭に **using System;** 行があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-129">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="9d196-130">その行の下に次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-130">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="9d196-131">**Main** メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9d196-131">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="9d196-132">このコードにより、**appsettings.json** ファイルから読み取る構成システムが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d196-132">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

<span data-ttu-id="9d196-133">**Program.cs** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9d196-133">Your **Program.cs** file should now look like the following:</span></span>

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

<span data-ttu-id="9d196-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="9d196-134">::: zone-end</span></span>

<span data-ttu-id="9d196-135">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="9d196-135">::: zone pivot="javascript"</span></span>

<span data-ttu-id="9d196-136">Node.js アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。</span><span class="sxs-lookup"><span data-stu-id="9d196-136">Let's add support to our Node.js application to retrieve a connection string from a configuration file.</span></span> <span data-ttu-id="9d196-137">JavaScript ファイルから構成を管理するために必要なプラミングを追加することから始めます。</span><span class="sxs-lookup"><span data-stu-id="9d196-137">We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.</span></span>

## <a name="create-a-env-configuration-file"></a><span data-ttu-id="9d196-138">.env 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="9d196-138">Create a .env configuration file</span></span>

1. <span data-ttu-id="9d196-139">プロジェクトの正しい作業ディレクトリで操作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="9d196-139">Make sure you are in the correct working directory for your project.</span></span>

1. <span data-ttu-id="9d196-140">コマンド ラインで `touch` ツールを使用して、**.env** という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9d196-140">Use the `touch` tool on the command line to create a file named **.env**.</span></span>

    ```bash
    touch .env
    ```

1. <span data-ttu-id="9d196-141">Cloud Shell エディターでプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="9d196-141">Open the project in the Cloud Shell editor.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="9d196-142">エディターで **.env** ファイルを選択し、次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-142">Select the **.env** file in the editor and add the following text.</span></span> <span data-ttu-id="9d196-143">場合によっては、コードで更新ボタンをクリックし、新しいファイルを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-143">You may need to click the refresh button in code to see the new files.</span></span> <span data-ttu-id="9d196-144">ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="9d196-144">Save the file - in the online editor, there is a menu in the top right corner which has common file operations.</span></span>

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > <span data-ttu-id="9d196-145">**AZURE_STORAGE_CONNECTION_STRING** 値は、Storage API でアクセス キーを検索するために使用される、ハード コーディングされた環境変数です。</span><span class="sxs-lookup"><span data-stu-id="9d196-145">The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys.</span></span> <span data-ttu-id="9d196-146">必要に応じて独自の名前を使用できますが、Node.js アプリで `BlobService` オブジェクトを作成したときの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-146">You can use your own name if you prefer, but you must supply the name to the when you create the `BlobService` object in your Node.js app.</span></span>

1. <span data-ttu-id="9d196-147">次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。</span><span class="sxs-lookup"><span data-stu-id="9d196-147">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span> <span data-ttu-id="9d196-148">Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d196-148">In Cloud Shell run the following command.</span></span>

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. <span data-ttu-id="9d196-149">そのコマンドから返された接続文字列をコピーし、**.env** ファイルの `<value>` をその接続文字列で置換します。引用符は含めません。</span><span class="sxs-lookup"><span data-stu-id="9d196-149">Copy the connection string that is returned from that command, minus the quotes, and replace `<value>` in the **.env** file with this connection string.</span></span>

1. <span data-ttu-id="9d196-150">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="9d196-150">Save the file.</span></span>

## <a name="add-support-to-read-an-environment-configuration-file"></a><span data-ttu-id="9d196-151">環境構成ファイルを読み取るためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="9d196-151">Add support to read an environment configuration file</span></span>

<span data-ttu-id="9d196-152">Node.js アプリに **.env** ファイルから読み取るためのサポートを含めるには、**dotenv** パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-152">Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.</span></span>

1. <span data-ttu-id="9d196-153">ウィンドウのコマンド プロンプト セクションで、`npm` を使用して、依存関係を *dotenv*\* パッケージに追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-153">In the command prompt section of the window, add a dependency to the  *dotenv*\* package using `npm`.</span></span>

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="9d196-154">構成ファイルを読み取るコードを追加する</span><span class="sxs-lookup"><span data-stu-id="9d196-154">Add code to read the configuration file</span></span>

<span data-ttu-id="9d196-155">構成の読み取りを有効にするために必要なライブラリが追加されたので、アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-155">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.</span></span>

1. <span data-ttu-id="9d196-156">エディターで *index.js*\* を選択します。</span><span class="sxs-lookup"><span data-stu-id="9d196-156">Select *index.js*\* in the editor.</span></span>

1. <span data-ttu-id="9d196-157">ファイルの一番上に **#!/usr/bin/env node** 行があります。</span><span class="sxs-lookup"><span data-stu-id="9d196-157">At the top of the file, a **#!/usr/bin/env node** line is present.</span></span> <span data-ttu-id="9d196-158">その行の下に、**dotenv** パッケージを読み込むための `require` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="9d196-158">Underneath that line, add a `require` statement to load the **dotenv** package.</span></span> <span data-ttu-id="9d196-159">これにより、**.env** ファイル内で定義された環境変数がプログラムで使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="9d196-159">This will make environment variables defined in our **.env** file available to the program.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
<span data-ttu-id="9d196-160">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="9d196-160">::: zone-end</span></span>

<span data-ttu-id="9d196-161">これですべて接続したので、ストレージ アカウントを使用するためのコードの追加を開始できます。</span><span class="sxs-lookup"><span data-stu-id="9d196-161">Now that we have that all wired up, we can start adding code to use our storage account.</span></span>