<span data-ttu-id="6365b-101">Azure App Service 認証を使用すると、Azure Functions アプリでターン キー認証がサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="6365b-101">Azure App Service authentication enables turn-key authentication support in an Azure Functions app.</span></span> <span data-ttu-id="6365b-102">Facebook、Twitter、Microsoft アカウント、Google、および Azure Active Directory とシームレスに統合されます。</span><span class="sxs-lookup"><span data-stu-id="6365b-102">It integrates seamlessly with Facebook, Twitter, Microsoft accounts, Google, and Azure Active Directory.</span></span> <span data-ttu-id="6365b-103">Web アプリのバックエンド API を保護するには、App Service 認証を追加します。</span><span class="sxs-lookup"><span data-stu-id="6365b-103">You'll add App Service authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="6365b-104">App Service 認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="6365b-104">Enable App Service authentication</span></span>

1. <span data-ttu-id="6365b-105">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="6365b-105">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="6365b-106">関数アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="6365b-106">Open the function app.</span></span>

1. <span data-ttu-id="6365b-107">**[プラットフォーム機能]** で、**[認証/承認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6365b-107">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![認証と承認を選択する](../media/6-authorization.jpg)

1. <span data-ttu-id="6365b-109">次の値を選択します。</span><span class="sxs-lookup"><span data-stu-id="6365b-109">Select the following values:</span></span>

    | <span data-ttu-id="6365b-110">設定</span><span class="sxs-lookup"><span data-stu-id="6365b-110">Setting</span></span>      |  <span data-ttu-id="6365b-111">推奨値</span><span class="sxs-lookup"><span data-stu-id="6365b-111">Suggested value</span></span>   | <span data-ttu-id="6365b-112">説明</span><span class="sxs-lookup"><span data-stu-id="6365b-112">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6365b-113">**App Service 認証**</span><span class="sxs-lookup"><span data-stu-id="6365b-113">**App Service Authentication**</span></span> | <span data-ttu-id="6365b-114">On</span><span class="sxs-lookup"><span data-stu-id="6365b-114">On</span></span> | <span data-ttu-id="6365b-115">認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6365b-115">Enable authentication.</span></span> |
    | <span data-ttu-id="6365b-116">**要求が認証されない場合のアクション**</span><span class="sxs-lookup"><span data-stu-id="6365b-116">**Action when request is not authenticated**</span></span> | <span data-ttu-id="6365b-117">Azure Active Directory でサインインします。</span><span class="sxs-lookup"><span data-stu-id="6365b-117">Sign in with Azure Active Directory.</span></span> | <span data-ttu-id="6365b-118">構成済みの認証方法 (下記) を選択します。</span><span class="sxs-lookup"><span data-stu-id="6365b-118">Select a configured authentication method (See below).</span></span> |
    | <span data-ttu-id="6365b-119">**認証プロバイダー**</span><span class="sxs-lookup"><span data-stu-id="6365b-119">**Authentication Providers**</span></span> | <span data-ttu-id="6365b-120">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6365b-120">See below.</span></span> | <span data-ttu-id="6365b-121">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6365b-121">See below.</span></span> |
    | <span data-ttu-id="6365b-122">**トークン ストア**</span><span class="sxs-lookup"><span data-stu-id="6365b-122">**Token store**</span></span> | <span data-ttu-id="6365b-123">On</span><span class="sxs-lookup"><span data-stu-id="6365b-123">On</span></span> | <span data-ttu-id="6365b-124">App Service でトークンを格納および管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="6365b-124">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="6365b-125">**許可される外部リダイレクト URL**</span><span class="sxs-lookup"><span data-stu-id="6365b-125">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="6365b-126">アプリケーションの URL。例: https://firstserverlessweb.z4.web.core.windows.net/。</span><span class="sxs-lookup"><span data-stu-id="6365b-126">The URL of your application, for example https://firstserverlessweb.z4.web.core.windows.net/.</span></span> | <span data-ttu-id="6365b-127">ユーザーが認証された後に App Service からリダイレクトできる URL。</span><span class="sxs-lookup"><span data-stu-id="6365b-127">URLs that App Service is allowed to redirect to, after a user is authenticated.</span></span> |

1. <span data-ttu-id="6365b-128">**[Azure Active Directory]** を選択して **[Azure Active Directory の設定]** を表示します。</span><span class="sxs-lookup"><span data-stu-id="6365b-128">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="6365b-129">**[管理モード]** として **[簡易]** を選択し、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="6365b-129">Select **Express** as the **Management Mode** and fill in the following information.</span></span>

        | <span data-ttu-id="6365b-130">設定</span><span class="sxs-lookup"><span data-stu-id="6365b-130">Setting</span></span>      |  <span data-ttu-id="6365b-131">推奨値</span><span class="sxs-lookup"><span data-stu-id="6365b-131">Suggested value</span></span>   | <span data-ttu-id="6365b-132">説明</span><span class="sxs-lookup"><span data-stu-id="6365b-132">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="6365b-133">**管理モード**</span><span class="sxs-lookup"><span data-stu-id="6365b-133">**Management mode**</span></span> | <span data-ttu-id="6365b-134">[簡易]、[新しい AD アプリを作成する]</span><span class="sxs-lookup"><span data-stu-id="6365b-134">Express, Create new AD app</span></span> | <span data-ttu-id="6365b-135">サービス プリンシパルと Azure Active Directory 認証が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="6365b-135">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="6365b-136">**アプリの作成**</span><span class="sxs-lookup"><span data-stu-id="6365b-136">**Create app**</span></span> | <span data-ttu-id="6365b-137">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="6365b-137">my-serverless-webapp</span></span> | <span data-ttu-id="6365b-138">一意のアプリケーション名を入力します。</span><span class="sxs-lookup"><span data-stu-id="6365b-138">Enter a unique application name.</span></span> |

    1. <span data-ttu-id="6365b-139">**[OK]** をクリックして Azure Active Directory の設定を保存します。</span><span class="sxs-lookup"><span data-stu-id="6365b-139">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![認証と承認と Azure Active Directory の設定](../media/6-create-aad.png)

1. <span data-ttu-id="6365b-141">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6365b-141">Click **Save**.</span></span>

## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="6365b-142">Web アプリを変更して認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="6365b-142">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="6365b-143">Cloud Shell で、現在のディレクトリが確実に **www/dist** フォルダーであるようにします。</span><span class="sxs-lookup"><span data-stu-id="6365b-143">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="6365b-144">**settings.js** を変更することで、ご自分の関数アプリで認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6365b-144">You enable authentication in your function app by modifying **settings.js**.</span></span> <span data-ttu-id="6365b-145">Cloud Shell エディターでファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6365b-145">Open the file in Cloud Shell Editor.</span></span>

    ```azurecli
    code settings.js
    ```

1. <span data-ttu-id="6365b-146">ファイルに次の行を追加し、保存します。</span><span class="sxs-lookup"><span data-stu-id="6365b-146">Append the following line to the file and save it.</span></span>

    ```azurecli
    window.authEnabled = true
    ```

1. <span data-ttu-id="6365b-147">ファイルが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6365b-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="6365b-148">ファイルを Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="6365b-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-application"></a><span data-ttu-id="6365b-149">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="6365b-149">Test the application</span></span>

1. <span data-ttu-id="6365b-150">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="6365b-150">Open the application in a browser.</span></span> <span data-ttu-id="6365b-151">**[ログイン]** をクリックしてログインします。</span><span class="sxs-lookup"><span data-stu-id="6365b-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="6365b-152">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="6365b-152">Select an image file and upload it.</span></span>

    ![サインイン ページ](../media/6-aad-auth.png)

## <a name="summary"></a><span data-ttu-id="6365b-154">まとめ</span><span class="sxs-lookup"><span data-stu-id="6365b-154">Summary</span></span>

<span data-ttu-id="6365b-155">このユニットでは、Azure App Service 認証を使用してアプリケーションに認証を追加する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="6365b-155">In this unit, you learned how to add authentication to the application using Azure App Service authentication.</span></span>