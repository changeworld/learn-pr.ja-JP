<span data-ttu-id="d8aa6-101">このユニットは、ASP.NET Core web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-101">In this unit, you will create an ASP.NET Core web app.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="d8aa6-102">新しい Web プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="d8aa6-102">Create a new web project</span></span>

<span data-ttu-id="d8aa6-103">.NET CLI ツールの中核部分は、`dotnet`コマンド ライン ツール。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-103">The heart of the .NET CLI tools is the `dotnet` command line tool.</span></span> <span data-ttu-id="d8aa6-104">このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-104">Using this command, you will create a new ASP.NET Core web project.</span></span>

1. <span data-ttu-id="d8aa6-105">右側の Cloud Shell では、新しい ASP.NET Core MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-105">In the Cloud Shell on the right, create a new ASP.NET Core MVC application.</span></span> <span data-ttu-id="d8aa6-106">"BestBikeApp"名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-106">Name it "BestBikeApp".</span></span>

```bash
dotnet new mvc -name BestBikeApp
```

1. <span data-ttu-id="d8aa6-107">"BestBikeApp"をという名前の新しいフォルダーを作成するコマンドは、プロジェクトを保持するためにします。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-107">The command will create a new folder named "BestBikeApp" to hold your project.</span></span> <span data-ttu-id="d8aa6-108">そのフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-108">Change into that folder.</span></span>

1. <span data-ttu-id="d8aa6-109">ビルドし、それが完了することを確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-109">Build and run the application to verify it is complete.</span></span>

```bash
dotnet run
```

<span data-ttu-id="d8aa6-110">ようなものを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-110">You should get something like:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

<span data-ttu-id="d8aa6-111">出力には、アプリを起動した後、状況がについて説明します。 アプリケーションが実行され、ポート 5000 でリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-111">The output describes the situation after starting your app: the application is running and listening at port 5000.</span></span>

<span data-ttu-id="d8aa6-112">ブラウザーを開いて、アプリを独自のマシンで実行されていた場合は予定 http://localhost:5000します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-112">If were running the app on our own machine we'd be able to open a browser to http://localhost:5000.</span></span> <span data-ttu-id="d8aa6-113">任意の場所にアプリを展開する必要がありますので、私たちは Azure Cloud Shell で、パブリック エンドポイントを使用します。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-113">Since we're on Azure Cloud Shell, we'll need to deploy the app to somewhere with a public endpoint.</span></span> <span data-ttu-id="d8aa6-114">以前に作成した App Service インスタンスが最適です。</span><span class="sxs-lookup"><span data-stu-id="d8aa6-114">The App Service instance we created earlier is perfect for that.</span></span>