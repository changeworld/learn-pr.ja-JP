<span data-ttu-id="f026e-101">Web アプリの作成と Azure への発行は問題なくできましたが、変更したいときはどうなるのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="f026e-101">You've successfully created your web app and published it to Azure, but what happens when you want to make changes?</span></span> <span data-ttu-id="f026e-102">Visual Studio ではアプリの発行先が記憶されているので、2 回クリックするだけでアプリの更新や変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f026e-102">Visual Studio will remember where the app is published, which makes updating and changing your app a two-click process.</span></span>

## <a name="explore-the-project-structure"></a><span data-ttu-id="f026e-103">プロジェクトの構造を調べる</span><span class="sxs-lookup"><span data-stu-id="f026e-103">Explore the project structure</span></span>

<span data-ttu-id="f026e-104">Visual Studio での ASP.NET Web アプリの作成が済んだので、今度は Web サイトを編集したりカスタマイズしたりする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f026e-104">We've created an ASP.NET web app in Visual Studio, and now you will need to edit and customize your website.</span></span> <span data-ttu-id="f026e-105">プロジェクトの構造を調べて、Visual Studio によってどのようなものが作成されたのかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="f026e-105">Let's explore the project structure to see what Visual Studio has created for us.</span></span>

### <a name="dependencies"></a><span data-ttu-id="f026e-106">依存関係</span><span class="sxs-lookup"><span data-stu-id="f026e-106">Dependencies</span></span>

<span data-ttu-id="f026e-107">依存関係には、アプリを稼働させるための ASP.NET の内部構造が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-107">Dependencies include the ASP.NET internals to get your app up and running.</span></span> <span data-ttu-id="f026e-108">特定のサード パーティ製パッケージを追加していない場合、このフォルダーに多くの時間を費やす必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f026e-108">Unless you are adding specific third-party packages, you won't need to spend much time in this folder.</span></span>

### <a name="properties"></a><span data-ttu-id="f026e-109">プロパティ</span><span class="sxs-lookup"><span data-stu-id="f026e-109">Properties</span></span>

<span data-ttu-id="f026e-110">プロパティ フォルダーには、Web アプリをホストしている場所についての構成データが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-110">The properties folder contains configuration data for where you are hosting your web app.</span></span> <span data-ttu-id="f026e-111">**PublishProfiles** フォルダーを展開すると、Alpine Ski Hill サイトの URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-111">If you expand the **PublishProfiles** folder now, you should see the URL for the Alpine Ski Hill site.</span></span> <span data-ttu-id="f026e-112">各発行プロファイルは .xml ファイルであり、Visual Studio がファイルのアップロードに使用する Azure のアドレスなど、発行の構成情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-112">Each publishing profile is an .xml file containing publishing configuration information such as the Azure address that Visual Studio uses to upload your files.</span></span>

### <a name="wwwroot"></a><span data-ttu-id="f026e-113">wwwroot</span><span class="sxs-lookup"><span data-stu-id="f026e-113">wwwroot</span></span>

<span data-ttu-id="f026e-114">wwwroot ファイルには、.css、.js、イメージ、.lib ファイルなど、サイトのすべての静的資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-114">The wwwroot file contains all of your static assets for your site, such as the .css, .js, images, and .lib files.</span></span> <span data-ttu-id="f026e-115">サイトのスタイルを設定したり、サイトに機能を追加したりする準備ができたら、ここで作業します。</span><span class="sxs-lookup"><span data-stu-id="f026e-115">When you are ready to style and add more functionality to your site, you will work in here.</span></span>

### <a name="pages"></a><span data-ttu-id="f026e-116">ページ</span><span class="sxs-lookup"><span data-stu-id="f026e-116">Pages</span></span>

<span data-ttu-id="f026e-117">**[ページ]** フォルダーには、サイトのページ用の _**Razor**_ テンプレートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-117">The **Pages** folder includes _**Razor**_ templates for the pages of your site.</span></span>
<span data-ttu-id="f026e-118">Razor は HTML を基にして作成されている構文であり、サイトでのデータの表示とロジックの実行のための特別な規則を備えています。</span><span class="sxs-lookup"><span data-stu-id="f026e-118">Razor is a syntax that is built up around HTML, which has special conventions for displaying data and executing logic on your site.</span></span>

<span data-ttu-id="f026e-119">サイト内の各ページは、2 つのコード ファイルで表されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-119">Each page in your site is represented with two code files:</span></span>

- <span data-ttu-id="f026e-120">1 つ目の `.cshtml` ファイルは、Razor マークアップ ファイルです。</span><span class="sxs-lookup"><span data-stu-id="f026e-120">The first is a `.cshtml` file, which is the Razor markup file.</span></span> <span data-ttu-id="f026e-121">このファイルには、表示用の HTML と若干の C# ロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f026e-121">This file includes your display HTML and some C# logic.</span></span>

- <span data-ttu-id="f026e-122">2 つ目の `.cs` ファイルは、`PageModel` クラスを含む C# コードビハインドです。</span><span class="sxs-lookup"><span data-stu-id="f026e-122">The second file is is a `.cs` file, which is the C# code-behind that includes `PageModel` class.</span></span> <span data-ttu-id="f026e-123">このファイルでは、HTTP 要求をインターセプトして、Razor ファイルにデータが渡される前に要求に対して何らかの処理を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f026e-123">This file allows you to intercept HTTP requests and do some processing on that request before passing off any data to the Razor file.</span></span>

### <a name="appsettingjson"></a><span data-ttu-id="f026e-124">appsetting.json</span><span class="sxs-lookup"><span data-stu-id="f026e-124">appsetting.json</span></span>

<span data-ttu-id="f026e-125">これは、ASP.NET 用の構成ファイルです。</span><span class="sxs-lookup"><span data-stu-id="f026e-125">This is a configuration file for ASP.NET.</span></span>

### <a name="bundleconfigjson"></a><span data-ttu-id="f026e-126">bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="f026e-126">bundleconfig.json</span></span>

<span data-ttu-id="f026e-127">bundleconfig.json では構成の前処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="f026e-127">The bundleconfig.json is preprocessing configuration.</span></span> <span data-ttu-id="f026e-128">このファイルにより、発行されるときの .css ファイルと .js ファイルが小さくなります。</span><span class="sxs-lookup"><span data-stu-id="f026e-128">This file is making your .css and .js files smaller when they are published.</span></span>

### <a name="programcs-and-startupcs"></a><span data-ttu-id="f026e-129">Program.cs、Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f026e-129">Program.cs and Startup.cs</span></span>

<span data-ttu-id="f026e-130">Program.cs と Startup.cs では、サイトに対する Web ホストの構成と起動が行われます。</span><span class="sxs-lookup"><span data-stu-id="f026e-130">Program.cs and Startup.cs configure and launch your web host for your site.</span></span>

## <a name="updating-your-website-using-razor"></a><span data-ttu-id="f026e-131">Razor を使用して Web サイトを更新する</span><span class="sxs-lookup"><span data-stu-id="f026e-131">Updating your website using Razor</span></span>

<span data-ttu-id="f026e-132">Web サイトに対していくつかの基本的な変更を行います。</span><span class="sxs-lookup"><span data-stu-id="f026e-132">We will want to make some basic changes to our website.</span></span> <span data-ttu-id="f026e-133">これを行うには、Razor テンプレートを使用して Web アプリをカスタマイズする方法を基本的に理解している必要があります。</span><span class="sxs-lookup"><span data-stu-id="f026e-133">In order to do this, you will need to have a basic understanding of how to leverage the Razor templates to customize your web app.</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="f026e-134">Razor とは</span><span class="sxs-lookup"><span data-stu-id="f026e-134">What is Razor?</span></span>

<span data-ttu-id="f026e-135">Razor は、C# で動的な Web ページを作成するために使用される ASP.NET 構文です。</span><span class="sxs-lookup"><span data-stu-id="f026e-135">Razor is an ASP.NET syntax used to create dynamic web pages with C#.</span></span> <span data-ttu-id="f026e-136">サーバーで Razor ページが読み取られると、HTML をレンダリングする前に、C# コードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-136">When a server reads a Razor page, the C# code is run before it renders the HTML.</span></span> <span data-ttu-id="f026e-137">これにより、動的なコンテンツをすばやく生成できます。</span><span class="sxs-lookup"><span data-stu-id="f026e-137">This allows you to generate dynamic content quickly.</span></span>

<span data-ttu-id="f026e-138">Razor では、`@` ディレクティブを使用してページの処理方法を ASP.NET に伝えます。</span><span class="sxs-lookup"><span data-stu-id="f026e-138">Razor uses `@` directives to tell ASP.NET how to process a page.</span></span>

<span data-ttu-id="f026e-139">例として、`Contact.cshtml` ページのコードを見てください。</span><span class="sxs-lookup"><span data-stu-id="f026e-139">For example, take a look at the code in the `Contact.cshtml` page.</span></span>

```aspx-csharp
@page
@model ContactModel
@{
    ViewData["Title"] = "Contact";
}
<h2>@ViewData["Title"]</h2>
<h3>@Model.Message</h3>
...
```

<span data-ttu-id="f026e-140">たとえば、`@page` ディレクティブは、Razor ページとしてこのファイルを処理するよう ASP.NET に指示します。</span><span class="sxs-lookup"><span data-stu-id="f026e-140">For example: The `@page` directive is telling ASP.NET to process this file as a Razor page.</span></span>
<span data-ttu-id="f026e-141">`@model` ディレクティブは、この Razor ページを `ContactModel` という名前の C# クラスと結び付けるよう ASP.NET に指示します。</span><span class="sxs-lookup"><span data-stu-id="f026e-141">The `@model` directive is telling ASP.NET to tie this Razor page with a C# class called `ContactModel`.</span></span>

<span data-ttu-id="f026e-142">Razor では、コードと HTML の切り替えのためにも `@` 記号が使用されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-142">Razor also uses the `@` symbol to transition between code and HTML.</span></span>
<span data-ttu-id="f026e-143">たとえば、上のスニペットを見ると、`@{ ... }` があることがわかります。</span><span class="sxs-lookup"><span data-stu-id="f026e-143">For example, if you look at the snippet above, you'll notice `@{ ... }`.</span></span> <span data-ttu-id="f026e-144">これは **Razor コード ブロック**です。</span><span class="sxs-lookup"><span data-stu-id="f026e-144">This is a **Razor code block**.</span></span> <span data-ttu-id="f026e-145">このコードは、"_実行されますが、レンダリングはされません_"。</span><span class="sxs-lookup"><span data-stu-id="f026e-145">It's code which is _executed but not rendered_.</span></span>

<span data-ttu-id="f026e-146">コード ステートメントの出力をレンダリングするには、C# 式の前に `@` を付けます。</span><span class="sxs-lookup"><span data-stu-id="f026e-146">To render the output of a code statement, we use the `@` in front of a C# expression.</span></span> <span data-ttu-id="f026e-147">上記のコード ブロックの `<h2>` タグと `<h3>` タグに、それの 2 つの例があります。</span><span class="sxs-lookup"><span data-stu-id="f026e-147">We have two examples of that in the code block above in the `<h2>` and `<h3>` tags.</span></span>

## <a name="publish-your-updates"></a><span data-ttu-id="f026e-148">更新を発行する</span><span class="sxs-lookup"><span data-stu-id="f026e-148">Publish your updates</span></span>

<span data-ttu-id="f026e-149">Web サイトを変更した後は、それを Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="f026e-149">Once you've made the changes to your website, you will want to publish them to Azure.</span></span> <span data-ttu-id="f026e-150">このプロセスは、最初に発行したときの方法と似ています。</span><span class="sxs-lookup"><span data-stu-id="f026e-150">This process is similar to how we initially published.</span></span>

1. <span data-ttu-id="f026e-151">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="f026e-151">Right-click the project in Solution Explorer.</span></span>

1. <span data-ttu-id="f026e-152">後に "Web 配置" が付いた Web サイトの名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-152">Now you should see the name of your website followed by Web Deploy.</span></span> <span data-ttu-id="f026e-153">たとえば、Web サイトの名前を AlpineSkiHouse42 にした場合、使用可能なオプションに "**AlpineSkiHouse42 - Web 配置**" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-153">For example if you named your website AlpineSkiHouse42, you would see **AlpineSkiHouse42 - Web Deploy** in the available options.</span></span> <span data-ttu-id="f026e-154">それを選択すると、Azure でサイトが更新されます。</span><span class="sxs-lookup"><span data-stu-id="f026e-154">Select that and your site will update in Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="f026e-155">まとめ</span><span class="sxs-lookup"><span data-stu-id="f026e-155">Summary</span></span>

<span data-ttu-id="f026e-156">Web サイトの作成と発行は、優れた Web サイトを作成する最初のステップに過ぎません。</span><span class="sxs-lookup"><span data-stu-id="f026e-156">Creating and publishing a website are just the first steps in creating a good website.</span></span> <span data-ttu-id="f026e-157">コンテンツの追加を始めたら、サイトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f026e-157">Once you start to add content, you'll need to update your site.</span></span> <span data-ttu-id="f026e-158">最初にサイトを Azure に発行した後は、ソリューション エクスプローラーでいつでも更新できます。</span><span class="sxs-lookup"><span data-stu-id="f026e-158">Once you've initially published your site to Azure, you can update at any time from the Solution Explorer.</span></span>
