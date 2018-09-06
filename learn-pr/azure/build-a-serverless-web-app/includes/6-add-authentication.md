<span data-ttu-id="b9dd1-101">Azure App Service 認証を使用すると、Azure Functions アプリでターン キー認証がサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-101">Azure App Service authentication enables turn-key authentication support in an Azure Functions app.</span></span> <span data-ttu-id="b9dd1-102">Facebook、Twitter、Microsoft アカウント、Google、および Azure Active Directory とシームレスに統合されます。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-102">It integrates seamlessly with Facebook, Twitter, Microsoft accounts, Google, and Azure Active Directory.</span></span> <span data-ttu-id="b9dd1-103">Web アプリのバックエンド API を保護するには、App Service 認証を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-103">You'll add App Service authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="b9dd1-104">App Service 認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="b9dd1-104">Enable App Service authentication</span></span>

1. <span data-ttu-id="b9dd1-105">Azure portal で関数アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-105">Open the function app in the Azure portal.</span></span>

1. <span data-ttu-id="b9dd1-106">**[プラットフォーム機能]** で、**[認証/承認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-106">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![認証と承認を選択する](../media/6-authorization.jpg)


1. <span data-ttu-id="b9dd1-108">次の値を選択します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-108">Select the following values:</span></span>
    
    | <span data-ttu-id="b9dd1-109">設定</span><span class="sxs-lookup"><span data-stu-id="b9dd1-109">Setting</span></span>      |  <span data-ttu-id="b9dd1-110">推奨値</span><span class="sxs-lookup"><span data-stu-id="b9dd1-110">Suggested value</span></span>   | <span data-ttu-id="b9dd1-111">説明</span><span class="sxs-lookup"><span data-stu-id="b9dd1-111">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="b9dd1-112">**App Service 認証**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-112">**App Service Authentication**</span></span> | <span data-ttu-id="b9dd1-113">On</span><span class="sxs-lookup"><span data-stu-id="b9dd1-113">On</span></span> | <span data-ttu-id="b9dd1-114">認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-114">Enable authentication.</span></span> |
    | <span data-ttu-id="b9dd1-115">**要求が認証されない場合のアクション**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-115">**Action when request is not authenticated**</span></span> | <span data-ttu-id="b9dd1-116">Azure Active Directory でサインインします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-116">Sign in with Azure Active Directory.</span></span> | <span data-ttu-id="b9dd1-117">構成済みの認証方法 (下記) を選択します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-117">Select a configured authentication method (See below).</span></span> |
    | <span data-ttu-id="b9dd1-118">**認証プロバイダー**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-118">**Authentication Providers**</span></span> | <span data-ttu-id="b9dd1-119">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-119">See below.</span></span> | <span data-ttu-id="b9dd1-120">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-120">See below.</span></span> |
    | <span data-ttu-id="b9dd1-121">**トークン ストア**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-121">**Token store**</span></span> | <span data-ttu-id="b9dd1-122">On</span><span class="sxs-lookup"><span data-stu-id="b9dd1-122">On</span></span> | <span data-ttu-id="b9dd1-123">App Service でトークンを格納および管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-123">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="b9dd1-124">**許可される外部リダイレクト URL**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-124">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="b9dd1-125">アプリケーションの URL。例: https://firstserverlessweb.z4.web.core.windows.net/。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-125">The URL of your application, for example https://firstserverlessweb.z4.web.core.windows.net/.</span></span> | <span data-ttu-id="b9dd1-126">ユーザーが認証された後に App Service からリダイレクトできる URL。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-126">URLs that App Service is allowed to redirect to, after a user is authenticated.</span></span> |

1. <span data-ttu-id="b9dd1-127">**[Azure Active Directory]** を選択して **[Azure Active Directory の設定]** を表示します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-127">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="b9dd1-128">**[管理モード]** として **[簡易]** を選択し、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-128">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="b9dd1-129">設定</span><span class="sxs-lookup"><span data-stu-id="b9dd1-129">Setting</span></span>      |  <span data-ttu-id="b9dd1-130">推奨値</span><span class="sxs-lookup"><span data-stu-id="b9dd1-130">Suggested value</span></span>   | <span data-ttu-id="b9dd1-131">説明</span><span class="sxs-lookup"><span data-stu-id="b9dd1-131">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="b9dd1-132">**管理モード**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-132">**Management mode**</span></span> | <span data-ttu-id="b9dd1-133">[簡易]、[新しい AD アプリを作成する]</span><span class="sxs-lookup"><span data-stu-id="b9dd1-133">Express, Create new AD app</span></span> | <span data-ttu-id="b9dd1-134">サービス プリンシパルと Azure Active Directory 認証が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-134">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="b9dd1-135">**アプリの作成**</span><span class="sxs-lookup"><span data-stu-id="b9dd1-135">**Create app**</span></span> | <span data-ttu-id="b9dd1-136">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="b9dd1-136">my-serverless-webapp</span></span> | <span data-ttu-id="b9dd1-137">一意のアプリケーション名を入力します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-137">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="b9dd1-138">**[OK]** をクリックして Azure Active Directory の設定を保存します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-138">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![認証と承認と Azure Active Directory の設定](../media/6-create-aad.png)


1. <span data-ttu-id="b9dd1-140">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-140">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="b9dd1-141">Web アプリを変更して認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="b9dd1-141">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="b9dd1-142">Cloud Shell で、現在のディレクトリが **www/dist** フォルダーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-142">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="b9dd1-143">関数アプリで認証を有効にするには、次のコード行を **settings.js** ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-143">To enable authentication in your function app, append the following line of code to the **settings.js** file:</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="b9dd1-144">次のコマンドを使用するか、VIM などのコマンド ライン エディターを使用して、変更を加え結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-144">Make the change and view the result by using the following commands, or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="b9dd1-145">ファイルが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-145">Confirm that the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="b9dd1-146">ファイルを Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-146">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="b9dd1-147">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="b9dd1-147">Test the application</span></span>

1. <span data-ttu-id="b9dd1-148">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-148">Open the application in a browser.</span></span> <span data-ttu-id="b9dd1-149">**[ログイン]** をクリックしてログインします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-149">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="b9dd1-150">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-150">Select an image file and upload it.</span></span>

    ![サインイン ページ](../media/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="b9dd1-152">まとめ</span><span class="sxs-lookup"><span data-stu-id="b9dd1-152">Summary</span></span>

<span data-ttu-id="b9dd1-153">このユニットでは、Azure App Service 認証を使用してアプリケーションに認証を追加する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="b9dd1-153">In this unit, you learned how to add authentication to the application using Azure App Service authentication.</span></span>