<span data-ttu-id="2db76-101">この演習では、アクセス キーを取得し、それをアプリ構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="2db76-101">In this exercise you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="2db76-102">また、.NET Core コンソールを作成したので、構成ファイルからアプリケーションに読み込むためのサポートを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2db76-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="2db76-103">JSON 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="2db76-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="2db76-104">前の演習で作成した Visual Studio プロジェクトで、プロジェクトを右クリックし、**[追加]** を選択し、**[新しい項目]** を選択します (あるいは CTRL-SHIFT-A を押します)。</span><span class="sxs-lookup"><span data-stu-id="2db76-104">In your Visual Studio project that we created in the previous unit, right click the project, select **Add**, then select **New Item** (or hold down CTRL-SHIFT-A).</span></span>
1. <span data-ttu-id="2db76-105">モーダル ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-105">A modal dialog is presented.</span></span> <span data-ttu-id="2db76-106">**[JavaScript JSON 構成ファイル]** を選択し、*[名前]* テキストボックスに「**appsettings.json**」と入力し、**[追加]**
  ![アプリ設定](..\media-draft\9-appsettings.png) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2db76-106">Select **JavaScript JSON Configuration File** and type **appsettings.json** into the *Name* textbox, then click **Add**
![appsettings](..\media-draft\9-appsettings.png)</span></span>
1. <span data-ttu-id="2db76-107">次のようなコンテンツのプロジェクトにファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-107">A file is added to the project with contents similar to the following:</span></span>
    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```
1. <span data-ttu-id="2db76-108">**除外**エントリとその関連コンテンツを削除し、**StorageAccountConnectionString** という 1 つのエントリを代わりに指定します。文字列値は空にします。</span><span class="sxs-lookup"><span data-stu-id="2db76-108">Remove the **exclude** entry and its associated contents and replace with a single entry **StorageAccountConnectionString** with an empty string value.</span></span> <span data-ttu-id="2db76-109">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2db76-109">It should look similar to the following:</span></span>
    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="2db76-110">**appsettings.json** ファイルを選択して右クリックし、**[プロパティ]** を選択します (あるいは、ALT + ENTER または F4 を押します)。</span><span class="sxs-lookup"><span data-stu-id="2db76-110">Select the **appsettings.json** file, right click it and select **Properties** (alternatively press ALT+ENTER or F4).</span></span>
1. <span data-ttu-id="2db76-111">**[出力ディレクトリにコピー]** を **[新しい場合はコピーする]**
  ![ビルド アクション](..\media-draft\10-build-action.png) に変更します。</span><span class="sxs-lookup"><span data-stu-id="2db76-111">Change the **Copy to Output Directory** to **Copy if newer**
![build action](..\media-draft\10-build-action.png)</span></span>

  > <span data-ttu-id="2db76-112">これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-112">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="2db76-113">JSON 構成ファイルを読み込むためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="2db76-113">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="2db76-114">.NET Core コンソール アプリケーションでは、JSON 構成ファイルからの読み込みを簡単にするために特定のライブラリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2db76-114">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="2db76-115">プロジェクトを右クリックし、**[Nuget パッケージの管理]**
  ![NuGet パッケージの管理](..\media-draft\11-manage-nuget-packages.png) を選択します。</span><span class="sxs-lookup"><span data-stu-id="2db76-115">Right click on the project and select **Manage Nuget Packages …**
![Manage NuGet packages](..\media-draft\11-manage-nuget-packages.png)</span></span>
1. <span data-ttu-id="2db76-116">現在インストールされている NuGet パッケージを示すページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-116">A page is displayed showing currently installed Nuget packages.</span></span> <span data-ttu-id="2db76-117">**[参照]** オプションをクリックし、検索フィールドに「**Microsoft.Extensions.Configuration.Json**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="2db76-117">Click on the **Browse** option and type in **Microsoft.Extensions.Configuration.Json** in the search field.</span></span> <span data-ttu-id="2db76-118">**Microsoft.Extensions.Configuration.Json** が検索結果に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-118">**Microsoft.Extensions.Configuration.Json** package is displayed in the search results.</span></span>
1. <span data-ttu-id="2db76-119">**Microsoft.Extensions.Configuration.Json** パッケージを選択し、右のウィンドウで **[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2db76-119">Select the **Microsoft.Extensions.Configuration.Json** package and in the right pane select **Install**</span></span>
1. <span data-ttu-id="2db76-120">**[変更のプレビュー]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-120">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="2db76-121">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2db76-121">Click **OK**</span></span>
1. <span data-ttu-id="2db76-122">ライセンス同意のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-122">A License acceptance dialog is displayed.</span></span> <span data-ttu-id="2db76-123">**[同意する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2db76-123">Click **I Accept**</span></span>

## <a name="read-from-the-configuration-file"></a><span data-ttu-id="2db76-124">構成ファイルから読み込む</span><span class="sxs-lookup"><span data-stu-id="2db76-124">Read from the configuration file</span></span>

<span data-ttu-id="2db76-125">読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2db76-125">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="2db76-126">プロジェクトで **Program.cs** ファイルを検索し、それを Visual Studio で開きます。</span><span class="sxs-lookup"><span data-stu-id="2db76-126">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>
1. <span data-ttu-id="2db76-127">ファイルの一番上に **using System;** があります。</span><span class="sxs-lookup"><span data-stu-id="2db76-127">At the top of the file, a **using System;** is present.</span></span> <span data-ttu-id="2db76-128">その行の下に次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="2db76-128">Underneath that line, add the following lines of code:</span></span>
    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```
1. <span data-ttu-id="2db76-129">**Main** メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2db76-129">Replace the contents of the **Main** method with the following code:</span></span>
    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```
1. <span data-ttu-id="2db76-130">**Program.cs** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2db76-130">Your **Program.cs** file should now look like the following:</span></span>
    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
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

> <span data-ttu-id="2db76-131">このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="2db76-131">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-storage-connection-string-to-the-configuration-file"></a><span data-ttu-id="2db76-132">構成ファイルにストレージ接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="2db76-132">Add your Storage connection string to the configuration file</span></span>

<span data-ttu-id="2db76-133">次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。</span><span class="sxs-lookup"><span data-stu-id="2db76-133">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="2db76-134">Azure portal に移動し、ストレージ アカウントを含むご利用のサブスクリプションを探します。</span><span class="sxs-lookup"><span data-stu-id="2db76-134">Navigate to the Azure portal for your subscription that contains your storage account.</span></span>
1. <span data-ttu-id="2db76-135">ストレージ アカウントに移動します。</span><span class="sxs-lookup"><span data-stu-id="2db76-135">Navigate to your storage account.</span></span>
1. <span data-ttu-id="2db76-136">ポータルでストレージ アカウントの [アカウント キー] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="2db76-136">Navigate to the Account Keys blade of the storage account in the portal.</span></span>
1. <span data-ttu-id="2db76-137">**key1** 接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="2db76-137">Copy the **key1** connection string.</span></span>
1. <span data-ttu-id="2db76-138">**StorageAccountConnectionString** の値としてポータルからコピーしたアクセス キーの内容を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="2db76-138">Paste in the contents of the access key you copied from the portal as the value for the **StorageAccountConnectionString**.</span></span> <span data-ttu-id="2db76-139">これで構成が次のようになるはずです。</span><span class="sxs-lookup"><span data-stu-id="2db76-139">Your configuration should now look similar to the following:</span></span>
    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```


