<span data-ttu-id="84ae8-101">このユニットでは、アクセス キーを取得し、それをアプリ構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-101">In this unit you will retrieve the access key and add it to your app configuration.</span></span> <span data-ttu-id="84ae8-102">また、.NET Core コンソールを作成したので、構成ファイルからアプリケーションに読み込むためのサポートを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-102">In addition, since we created a .NET Core console application, we need to add support for reading from configuration files to the application.</span></span>

## <a name="create-a-json-configuration-file"></a><span data-ttu-id="84ae8-103">JSON 構成ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="84ae8-103">Create a JSON configuration file</span></span>

1. <span data-ttu-id="84ae8-104">前の演習で作成した Visual Studio プロジェクトで、プロジェクトを右クリックし、**[追加]** を選択して、**[新しい項目]** を選択します (あるいは CTRL-SHIFT-A を押します)。</span><span class="sxs-lookup"><span data-stu-id="84ae8-104">In your Visual Studio project that we created in the previous unit, right click the project, select **Add**, and then select **New Item** (or hold down CTRL-SHIFT-A).</span></span>

1. <span data-ttu-id="84ae8-105">モーダル ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-105">A modal dialog is presented.</span></span> <span data-ttu-id="84ae8-106">**[JavaScript JSON 構成ファイル]** を選択し、*[名前]* テキストボックスに「**appsettings.json**」と入力し、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="84ae8-106">Select **JavaScript JSON Configuration File** and type **appsettings.json** into the *Name* textbox, and then click **Add**.</span></span>

  ![アプリケーション設定](..\media-draft\7-appsettings.png)

1. <span data-ttu-id="84ae8-108">次のようなコンテンツのプロジェクトにファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-108">A file is added to the project with contents similar to the following:</span></span>

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
1. <span data-ttu-id="84ae8-109">**除外**エントリとその関連コンテンツを削除し、**StorageAccountConnectionString** という 1 つのエントリを代わりに指定します。文字列値は空にします。</span><span class="sxs-lookup"><span data-stu-id="84ae8-109">Remove the **exclude** entry and its associated contents and replace it with the single entry **StorageAccountConnectionString**, with an empty string value.</span></span> <span data-ttu-id="84ae8-110">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-110">It should look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. <span data-ttu-id="84ae8-111">**appsettings.json** ファイルを選択して右クリックし、**[プロパティ]** を選択します (あるいは、ALT + ENTER または F4 を押します)。</span><span class="sxs-lookup"><span data-stu-id="84ae8-111">Select the **appsettings.json** file, right click it and select **Properties** (alternatively, press ALT+ENTER or F4).</span></span>

1. <span data-ttu-id="84ae8-112">**[出力ディレクトリにコピー]** を **[新しい場合はコピーする]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-112">Change **Copy to Output Directory** to **Copy if newer**.</span></span>

  ![ビルド アクション](..\media-draft\7-build-action.png)

  > <span data-ttu-id="84ae8-114">これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-114">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="84ae8-115">JSON 構成ファイルを読み込むためのサポートを追加する</span><span class="sxs-lookup"><span data-stu-id="84ae8-115">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="84ae8-116">.NET Core コンソール アプリケーションでは、JSON 構成ファイルからの読み込みを簡単にするために特定のライブラリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-116">A .NET Core console application requires certain libraries to be added to allow it to easily read from a JSON configuration file.</span></span>

1. <span data-ttu-id="84ae8-117">プロジェクトを右クリックし、**[NuGet パッケージの管理…]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-117">Right click on the project and select **Manage Nuget Packages…**.</span></span>

![Manage NuGet packages](..\media-draft\5-manage-nuget-packages.png)

1. <span data-ttu-id="84ae8-119">現在インストールされている NuGet パッケージを示すページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-119">A page is displayed, showing currently installed Nuget packages.</span></span> <span data-ttu-id="84ae8-120">**[参照]** オプションをクリックして、検索フィールドに「**Microsoft.Extensions.Configuration.Json**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-120">Click on the **Browse** option and type **Microsoft.Extensions.Configuration.Json** in the search field.</span></span> <span data-ttu-id="84ae8-121">**Microsoft.Extensions.Configuration.Json** パッケージが検索結果に表示されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-121">The **Microsoft.Extensions.Configuration.Json** package is displayed in the search results.</span></span>

1. <span data-ttu-id="84ae8-122">**Microsoft.Extensions.Configuration.Json** パッケージを選択し、右のウィンドウで **[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-122">Select the **Microsoft.Extensions.Configuration.Json** package, and in the right pane select **Install**.</span></span>

1. <span data-ttu-id="84ae8-123">**[変更のプレビュー]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-123">A **Preview Changes** dialog is displayed.</span></span> <span data-ttu-id="84ae8-124">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="84ae8-124">Click **OK**.</span></span>

1. <span data-ttu-id="84ae8-125">ライセンス同意のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-125">A license acceptance dialog is displayed.</span></span> <span data-ttu-id="84ae8-126">**[同意する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="84ae8-126">Click **I Accept**.</span></span>


## <a name="read-from-the-configuration-file"></a><span data-ttu-id="84ae8-127">構成ファイルから読み込む</span><span class="sxs-lookup"><span data-stu-id="84ae8-127">Read from the configuration file</span></span>

<span data-ttu-id="84ae8-128">読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-128">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="84ae8-129">プロジェクトで **Program.cs** ファイルを検索し、それを Visual Studio で開きます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-129">Locate the **Program.cs** file in your project and open it in Visual Studio.</span></span>

1. <span data-ttu-id="84ae8-130">ファイルの一番上に **using System;** 行があります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-130">At the top of the file, a **using System;** line is present.</span></span> <span data-ttu-id="84ae8-131">その行の下に次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-131">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="84ae8-132">**Main** メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-132">Replace the contents of the **Main** method with the following code:</span></span>

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. <span data-ttu-id="84ae8-133">**Program.cs** ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="84ae8-133">Your **Program.cs** file should now look like the following:</span></span>

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

> <span data-ttu-id="84ae8-134">このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-134">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

## <a name="add-your-connection-string-to-the-configuration-file"></a><span data-ttu-id="84ae8-135">構成ファイルに接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="84ae8-135">Add your connection string to the configuration file</span></span>

<span data-ttu-id="84ae8-136">次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-136">Now we need to get the storage account connection string and place it into the configuration for our app.</span></span>

1. <span data-ttu-id="84ae8-137">Azure portal に移動し、ストレージ アカウントを含むご利用のサブスクリプションを探します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-137">Navigate to the Azure portal for your subscription that contains your storage account.</span></span>

1. <span data-ttu-id="84ae8-138">ストレージ アカウントに移動します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-138">Navigate to your storage account.</span></span>

1. <span data-ttu-id="84ae8-139">ポータルでストレージ アカウントの [アカウント キー] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="84ae8-139">Navigate to the Account Keys blade of the storage account in the portal.</span></span>

1. <span data-ttu-id="84ae8-140">**key1** 接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="84ae8-140">Copy the **key1** connection string.</span></span>

1. <span data-ttu-id="84ae8-141">**StorageAccountConnectionString** の値としてポータルからコピーしたアクセス キーの内容を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="84ae8-141">Paste in the contents of the access key you copied from the portal as the value for **StorageAccountConnectionString**.</span></span> <span data-ttu-id="84ae8-142">これで構成が次のようになるはずです。</span><span class="sxs-lookup"><span data-stu-id="84ae8-142">Your configuration should now look similar to the following:</span></span>

    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```
