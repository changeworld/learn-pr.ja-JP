<span data-ttu-id="ae1ae-101">このユニットでは、ASP.NET Core Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-101">In this unit, you will create an ASP.NET Core web app.</span></span>

## <a name="create-a-new-web-project"></a><span data-ttu-id="ae1ae-102">新しい Web プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="ae1ae-102">Create a new web project</span></span>

<span data-ttu-id="ae1ae-103">.NET CLI ツールの中心になるのは、`dotnet` コマンド ライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-103">The heart of the .NET CLI tools is the `dotnet` command line tool.</span></span> <span data-ttu-id="ae1ae-104">このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-104">Using this command, you will create a new ASP.NET Core web project.</span></span>

<span data-ttu-id="ae1ae-105">右側の Cloud Shell で、新しい ASP.NET Core MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-105">In the Cloud Shell on the right, create a new ASP.NET Core MVC application.</span></span> <span data-ttu-id="ae1ae-106">それに "BestBikeApp" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-106">Name it "BestBikeApp".</span></span>

```bash
dotnet new mvc --name BestBikeApp
```

<span data-ttu-id="ae1ae-107">コマンドにより、プロジェクトを保持するための "BestBikeApp" という名前の新しいフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-107">The command will create a new folder named "BestBikeApp" to hold your project.</span></span> <span data-ttu-id="ae1ae-108">`cd` 次に、アプリケーションをビルドして実行し、完了したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-108">`cd` there, then build and run the application to verify it is complete.</span></span>

```bash
cd BestBikeApp
dotnet run
```

<span data-ttu-id="ae1ae-109">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-109">You should get something like:</span></span>

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

<span data-ttu-id="ae1ae-110">出力ではアプリ開始後の状況が説明されます。アプリケーションは実行され、ポート 5000 でリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-110">The output describes the situation after starting your app: the application is running and listening at port 5000.</span></span>

<span data-ttu-id="ae1ae-111">独自のコンピューター上でアプリを実行していた場合は、ブラウザーで http://localhost:5000 を開くことができます。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-111">If were running the app on our own machine we'd be able to open a browser to http://localhost:5000.</span></span> <span data-ttu-id="ae1ae-112">ここでは Azure Cloud Shell を使用しているため、パブリック エンドポイントで任意の場所にアプリをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-112">Since we're on Azure Cloud Shell, we'll need to deploy the app to somewhere with a public endpoint.</span></span> <span data-ttu-id="ae1ae-113">その場合、以前に作成した App Service インスタンスが最適です。</span><span class="sxs-lookup"><span data-stu-id="ae1ae-113">The App Service instance we created earlier is perfect for that.</span></span>