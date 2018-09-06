<span data-ttu-id="3225c-101">これで、Alpine Ski House の Web アプリが起動および実行されたので、あなたはこれを上司に見せる必要があります。</span><span class="sxs-lookup"><span data-stu-id="3225c-101">Your Alpine Ski House web app is up and running, but now you need to show it to your boss.</span></span> <span data-ttu-id="3225c-102">サイトを更新し、それらの更新を Azure に発行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3225c-102">You are going to have to update the site and publish those updates to Azure.</span></span>

## <a name="update-your-web-app"></a><span data-ttu-id="3225c-103">Web アプリを更新する</span><span class="sxs-lookup"><span data-stu-id="3225c-103">Update your web app</span></span>

### <a name="replace-the-boilerplate-code"></a><span data-ttu-id="3225c-104">定型のコードを置き換える</span><span class="sxs-lookup"><span data-stu-id="3225c-104">Replace the boilerplate code</span></span>

1. <span data-ttu-id="3225c-105">**[ページ]** フォルダーで **[About.cshtml]** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="3225c-105">In the **Pages** folder, open the **About.cshtml** file.</span></span>
1. <span data-ttu-id="3225c-106">コードの下部で `<p> Use this area to provide additional information. </p>` を検索します。</span><span class="sxs-lookup"><span data-stu-id="3225c-106">At the bottom of the code, locate `<p> Use this area to provide additional information. </p>`</span></span>
1. <span data-ttu-id="3225c-107">定型のテキストを `Welcome to the Alpine Ski House!` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3225c-107">Replace the boilerplate text with `Welcome to the Alpine Ski House!`</span></span>
1. <span data-ttu-id="3225c-108">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="3225c-108">Save the file.</span></span>
1. <span data-ttu-id="3225c-109">**About.cshtml.cs** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="3225c-109">Open the **About.cshtml.cs** file.</span></span>
1. <span data-ttu-id="3225c-110">`Message` の文字列を、**Alpine Ski House is the premier ski hill in Northeast.** などに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3225c-110">Replace the `Message` string to say **Alpine Ski House is the premier ski hill in Northeast.**</span></span>
1. <span data-ttu-id="3225c-111">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="3225c-111">Save the file.</span></span>

### <a name="publish-your-updates"></a><span data-ttu-id="3225c-112">更新を発行する</span><span class="sxs-lookup"><span data-stu-id="3225c-112">Publish your updates</span></span>

1. <span data-ttu-id="3225c-113">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="3225c-113">In Solution Explorer, right-click the project.</span></span>
1. <span data-ttu-id="3225c-114">**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3225c-114">Select **Publish**.</span></span> <span data-ttu-id="3225c-115">お使いの [Web サイト]-Web 配置を含むオプションがあるはずです。</span><span class="sxs-lookup"><span data-stu-id="3225c-115">You should have an option that includes your [website]-web deploy.</span></span>
1. <span data-ttu-id="3225c-116">ご自分のサイトを選択すると、Visual Studio によって変更点が Azure に送信されます。</span><span class="sxs-lookup"><span data-stu-id="3225c-116">Select your site, and Visual Studio will send your changes to Azure.</span></span>

### <a name="view-your-changes"></a><span data-ttu-id="3225c-117">変更を確認する</span><span class="sxs-lookup"><span data-stu-id="3225c-117">View your changes</span></span>

<span data-ttu-id="3225c-118">変更を発行したら、Visual Studio によってサイトがブラウザーで開かれます。</span><span class="sxs-lookup"><span data-stu-id="3225c-118">Once you've published your changes, Visual Studio will open the site in your browser.</span></span> <span data-ttu-id="3225c-119">**[About]\([概要]\)** ページに移動し、変更が反映されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3225c-119">Navigate to the **About** page, and see that your changes are reflected.</span></span>

## <a name="congrats"></a><span data-ttu-id="3225c-120">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="3225c-120">Congrats!</span></span>

<span data-ttu-id="3225c-121">Web アプリが Visual Studio 2017 で正常に更新されました。</span><span class="sxs-lookup"><span data-stu-id="3225c-121">You have successfully updated your web app using Visual Studio 2017.</span></span>
