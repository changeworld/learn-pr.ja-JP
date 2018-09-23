<span data-ttu-id="5cc13-101">このユニットでは、ASP.NET Core アプリケーションを Azure App Service にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-101">In this unit, you'll upload your ASP.NET Core application to Azure App Service.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="5cc13-102">ステージング デプロイ スロットを作成する</span><span class="sxs-lookup"><span data-stu-id="5cc13-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="5cc13-103">[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) に戻ります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-103">Switch back to the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="5cc13-104">以前に作成した App Service リソース (Web アプリ) を開きます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-104">Open the App Service resource (the web app) you created previously.</span></span> <span data-ttu-id="5cc13-105">**[すべてのリソース]** でアプリを検索するか、**[リソース グループ]** でアプリが属するリソース グループを検索することでもう一度見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-105">You can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.</span></span>

1. <span data-ttu-id="5cc13-106">左側のナビゲーションで **[デプロイ スロット]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-106">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="5cc13-107">**[デプロイ スロット]** ページで、上部ナビゲーション バーの **[スロットの追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-107">Inside the **Deployment slots** page, click the **Add Slot** button on the top navigation bar of the deployment slots page.</span></span>

1. <span data-ttu-id="5cc13-108">次に示すような **[スロットの追加]** ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-108">The Azure portal opens the **Add a slot** page as shown below.</span></span>

    1. <span data-ttu-id="5cc13-109">デプロイ スロットに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-109">Give your deployment slot a name.</span></span> <span data-ttu-id="5cc13-110">ここでは `staging` を使用します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-110">In this case, use `staging`.</span></span>

    2. <span data-ttu-id="5cc13-111">**[構成のソース]** を選択するときは、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-111">To choose a **Configuration Source**, you have two options.</span></span>

        * <span data-ttu-id="5cc13-112">既存のデプロイ スロットまたは App Service アプリから構成要素を複製します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-112">You can choose to clone the configuration elements from any existing deployment slot or App Service app.</span></span>
        * <span data-ttu-id="5cc13-113">または、構成要素を複製しないことを選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-113">Or you can choose not to clone any configuration elements.</span></span> <span data-ttu-id="5cc13-114">**[既存のスロットから構成を複製しない]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-114">Select the option **Don't clone configuration from an existing slot**.</span></span>

        <span data-ttu-id="5cc13-115">このデプロイ スロットでは、2 番目のオプションの **[既存のスロットから構成を複製しない]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-115">For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**.</span></span> <span data-ttu-id="5cc13-116">スロットを直接構成します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-116">You will configure it directly.</span></span>

    ![新しいステージング デプロイ スロットの構成を示す Azure portal のスクリーンショット。](../media/7-new-deployment-slot-blade.png)

1. <span data-ttu-id="5cc13-118">ページの下部にある **[OK]** をクリックして、新しいデプロイ スロットを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-118">Click the **OK** button at the bottom of the page to create your new deployment slot.</span></span>

1. <span data-ttu-id="5cc13-119">デプロイ スロットが正常に作成されると、Web アプリの **[デプロイ スロット]** ページに表示が戻ります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-119">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** page of your web app.</span></span>

    <span data-ttu-id="5cc13-120">ここで、先ほど作成した新しいデプロイ スロットを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-120">Now, you can see the new deployment slot that you have just created.</span></span>

    ![作成した新しいスロットが含まれた [デプロイ スロット] ページを示す Azure portal のスクリーンショット。](../media/7-deployment-slot-created.png)

1. <span data-ttu-id="5cc13-122">新しいデプロイ スロットを選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-122">Select the new deployment slot.</span></span>

1. <span data-ttu-id="5cc13-123">新しく作成されたデプロイ スロットの **[概要]** ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-123">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![ステージング デプロイ スロット](../media/7-deployment-slot-staging.png)

    <span data-ttu-id="5cc13-125">ステージング デプロイ スロットの **URL** に注意してください。</span><span class="sxs-lookup"><span data-stu-id="5cc13-125">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="5cc13-126">スロット名が追加され、以前に表示されていたものとは異なる URL になっています。</span><span class="sxs-lookup"><span data-stu-id="5cc13-126">It is a different URL from what you saw previously, with the slot name appended.</span></span>

    <span data-ttu-id="5cc13-127">デプロイ スロットは、Azure 内で完全な App Service アプリとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-127">A deployment slot is treated as a full App Service app inside Azure.</span></span> <span data-ttu-id="5cc13-128">ただし、元のアプリの子という特別な種類であり、元のアプリとスワップすることができます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-128">However, it is a special type that is a child of the original app and can be swapped with the original app.</span></span>

    <span data-ttu-id="5cc13-129">**URL** をクリックすると、Azure portal で初めて作成したデプロイ スロット "アプリ" に対して Azure で作成されたものと同じ既定のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-129">If you click the **URL**, you will see the same default page that Azure created for the deployment slot "app" the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="5cc13-130">ステージング デプロイ スロットが正常に作成されたので、次に、**デプロイ資格情報**を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-130">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="5cc13-131">デプロイ資格情報を作成する</span><span class="sxs-lookup"><span data-stu-id="5cc13-131">Create deployment credentials</span></span>

<span data-ttu-id="5cc13-132">Azure では、実際のデプロイ プロセスを始める前に、デプロイ資格情報を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-132">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="5cc13-133">そのため、独自のデプロイ資格情報を作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-133">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="5cc13-134">左側のナビゲーションで、**[デプロイ資格情報]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-134">Click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="5cc13-135">次に示すような **[デプロイ資格情報]** ページに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-135">The Azure portal navigates to the **Deployment credentials** page as shown below.</span></span>

    <span data-ttu-id="5cc13-136">任意の**ユーザー名**と**パスワード**を入力し、そのパスワードをもう一度入力して確認します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-136">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5cc13-137">ユーザー名とパスワードは決して忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="5cc13-137">Make sure you don't forget your username and password!</span></span> <span data-ttu-id="5cc13-138">後で Azure へのコードのアップロードとデプロイを始めるときに必要になります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-138">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    ![必須フィールドに資格情報の例が入力された、ステージング スロットの [デプロイ資格情報] ページを示す Azure portal のスクリーンショット。](../media/7-deployment-credentials.png)

1. <span data-ttu-id="5cc13-140">**[デプロイ資格情報]** ページの上部にある **[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-140">Click the **Save** button at the top of the **Deployment credentials** page.</span></span>

<span data-ttu-id="5cc13-141">デプロイ資格情報が正常に作成されたので、次に、他のデプロイ オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-141">Now that the deployment credentials are created successfully, you need to configure other deployment options.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="5cc13-142">デプロイ オプションとしてローカル Git リポジトリを使用する</span><span class="sxs-lookup"><span data-stu-id="5cc13-142">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="5cc13-143">次に、コードのアップロードを開始できるように、Azure でローカル Git リポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-143">Next, we'll create a local Git repository in Azure, so you can start uploading your code.</span></span>

1. <span data-ttu-id="5cc13-144">**staging** デプロイ スロット "アプリ" 内で、左側のナビゲーションの **[デプロイ オプション]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-144">Within the **staging** deployment slot "app", click the **Deployment options** menu item on the left-hand navigation.</span></span>

1. <span data-ttu-id="5cc13-145">**[デプロイ オプション]** ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-145">The Azure portal navigates to the **Deployment options** page.</span></span>

1. <span data-ttu-id="5cc13-146">**[ソースの選択]** をクリックして必要な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-146">Click on the **Choose Source** to configure the required settings.</span></span>

1. <span data-ttu-id="5cc13-147">構成して使用できるオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-147">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="5cc13-148">この例では、**[ローカル Git リポジトリ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-148">In our case, choose the **Local Git Repository** option.</span></span>

1. <span data-ttu-id="5cc13-149">自動的に **[デプロイ オプション]** ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-149">You will be returned to the **Deployment option** page.</span></span> <span data-ttu-id="5cc13-150">ページの下部にある **[OK]** をクリックしてデプロイ ソースを設定します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-150">Click the **OK** button at the bottom of the page to set up the deployment source.</span></span>

1. <span data-ttu-id="5cc13-151">次に、左側のナビゲーションにある **[概要]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-151">Now, navigate to the **Overview** section on the left-side navigation.</span></span>

    <span data-ttu-id="5cc13-152">ここで注意すべき重要な情報は **Git Clone URI** です。これは、ご利用のローカル アプリケーション コード リポジトリで**リモート**として使用するローカル Git リポジトリの URL です。</span><span class="sxs-lookup"><span data-stu-id="5cc13-152">The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local application code repository.</span></span>

<span data-ttu-id="5cc13-153">次に、ステージング デプロイ スロットへのご自分のコードのアップロードを開始します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-153">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="set-up-git-on-cloud-shell"></a><span data-ttu-id="5cc13-154">Cloud Shell 上で git を設定する</span><span class="sxs-lookup"><span data-stu-id="5cc13-154">Set up git on Cloud Shell</span></span>

<span data-ttu-id="5cc13-155">Git は既にインストールされている Azure Cloud Shell ですが、ご自分の Cloud Shell アカウントのユーザー名と電子メール アドレスを設定することが必要になります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-155">Git is already installed Azure Cloud Shell but you'll want to set your username and email for your cloud shell account.</span></span>

1. <span data-ttu-id="5cc13-156">右側の Cloud Shell で、次のコマンドを入力し、プレースホルダー `[your name]` と `[your email]` を、自分の名前とメール アドレスに置き換えます (かっこは不要です)。</span><span class="sxs-lookup"><span data-stu-id="5cc13-156">In the Cloud Shell on the right, type the following commands, replacing the `[your name]` and `[your email]` placeholders with your own name and email (without the braces):</span></span>

    ```bash
    git config --global user.name "[your name]"
    git config --global user.email "[your email]"
    ```

1. <span data-ttu-id="5cc13-157">Git によって情報が記録されたことを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-157">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```bash
    cat ~/.gitconfig
    ```

   <span data-ttu-id="5cc13-158">次のように、名前とメール アドレスが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="5cc13-158">You should be seeing the following, with your name and email shown:</span></span>

    ```output
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-code"></a><span data-ttu-id="5cc13-159">コード用にローカル Git リポジトリを初期化する</span><span class="sxs-lookup"><span data-stu-id="5cc13-159">Initialize a local Git repository for your code</span></span>

<span data-ttu-id="5cc13-160">Git を使い始めるには、ご自分の .NET Core アプリケーション コード用にローカル Git リポジトリを初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-160">To start using Git, you need to initialize a local Git repository for your .NET Core application code.</span></span>

1. <span data-ttu-id="5cc13-161">先ほど作成したプロジェクト フォルダーが選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-161">Make sure you are in the project folder you created earlier.</span></span>

    ```bash
    cd ~/BestBikeApp/
    ```

1. <span data-ttu-id="5cc13-162">次のコマンドを発行して、新しい Git リポジトリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-162">Initialize a new Git repository by issuing the following command:</span></span>

    ```bash
    git init
    ```

    <span data-ttu-id="5cc13-163">コマンドが成功すると、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-163">If the command is successful, you receive a message like the following:</span></span>

    ```output
    Initialized empty Git repository in /home/{your-user}/BestBikeApp/.git/
    ```

1. <span data-ttu-id="5cc13-164">すべてのアプリケーション ファイルを Git にステージングします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-164">Stage all the application files to Git.</span></span>

   <span data-ttu-id="5cc13-165">次に、Git にアプリケーション ファイルを認識させます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-165">The next step is to let Git know about your application files.</span></span> <span data-ttu-id="5cc13-166">そのためには、Git によって**ステージング**されるように、作業ディレクトリのすべてのファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-166">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="5cc13-167">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-167">Type the following command:</span></span>

    ```bash
    git add .
    ```

    <span data-ttu-id="5cc13-168">上のコマンドでは、"." によって表されるすべてのファイルが、Git のステージング状態に追加されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-168">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="5cc13-169">次に、Git に変更をコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-169">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="5cc13-170">Git にファイルをステージングした後は、ローカル コンピューター上の **Git コミット履歴**にファイルをコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-170">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="5cc13-171">そのためには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-171">You do that by typing the following command:</span></span>

    ```bash
   git commit -m "Initial create"
    ```

   <span data-ttu-id="5cc13-172">`commit` コマンドでは、作成しているコミットに関するメッセージを含めるための `-m` 引数が受け付けられます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-172">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="5cc13-173">後で、コードを Azure にプッシュするときに、この特定のコミットに格納されている同じメッセージを見ることができます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-173">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="5cc13-174">ローカル Git リポジトリに対するリモートを追加する</span><span class="sxs-lookup"><span data-stu-id="5cc13-174">Add a remote for the local Git repository</span></span>

<span data-ttu-id="5cc13-175">この時点では、新しいローカル Git リポジトリが正常に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="5cc13-175">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="5cc13-176">さらに、すべてのアプリケーション ファイルを Git にコミットしてあります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-176">In addition, you've committed all of your application files to Git.</span></span> <span data-ttu-id="5cc13-177">残っているのは、ローカル Git リポジトリを Azure でホストされているリポジトリに接続するための**リモート**を追加することです。</span><span class="sxs-lookup"><span data-stu-id="5cc13-177">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="5cc13-178">そのためには、以下のことを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-178">To do so, you need to:</span></span>

1. <span data-ttu-id="5cc13-179">前の手順で表示された **Git クローン URL** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-179">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="5cc13-180">コピーした後、**ターミナル** ウィンドウに戻り、URL を含む次の Git コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-180">Once copied, you go back to the **Terminal** window and issue the following Git command with your url:</span></span>

    ```bash
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="5cc13-181">上の Git コマンドでは、ローカル Git リポジトリが Azure でホストされているリポジトリにフックされます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-181">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="5cc13-182">これで、ローカル Git リポジトリとリモート Git リポジトリの間でプッシュとプルを始めることができます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-182">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="5cc13-183">上記のコマンドを確認するには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-183">To verify the above command, type the following Git command:</span></span>

    ```bash
    git remote -v
    ```

    <span data-ttu-id="5cc13-184">上のコマンドでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-184">The command above generates the following output:</span></span>

    ```output
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="5cc13-185">コードを Azure にプッシュする</span><span class="sxs-lookup"><span data-stu-id="5cc13-185">Push your code to Azure</span></span>

<span data-ttu-id="5cc13-186">ローカル Git リポジトリを Azure 上のリモート Git リポジトリにフックしたので、アプリを開発してビルドした後、Azure にアプリケーション コードをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-186">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your application code to Azure.</span></span>

1. <span data-ttu-id="5cc13-187">**master** ブランチを Azure 上のリモート Git リポジトリにプッシュするには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-187">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```bash
    git push origin master
    ```

1. <span data-ttu-id="5cc13-188">前に **[デプロイ資格情報]** セクションで構成したパスワードの入力を求められます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-188">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="5cc13-189">パスワードを入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-189">Enter your password and hit Enter.</span></span> <span data-ttu-id="5cc13-190">Git で、ステージング デプロイ スロットに構成されている Azure リモート Git リポジトリへのコミットされたファイルのアップロードが開始されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-190">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-code-is-uploaded-to-azure"></a><span data-ttu-id="5cc13-191">コードが Azure にアップロードされたことを確認する</span><span class="sxs-lookup"><span data-stu-id="5cc13-191">Verify the code is uploaded to Azure</span></span>

1. <span data-ttu-id="5cc13-192">Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-192">Switch back to the Azure portal.</span></span>

1. <span data-ttu-id="5cc13-193">左側のナビゲーションで、**[すべてのリソース]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-193">Click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="5cc13-194">これまでに Azure で作成されたすべてのリソースの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-194">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="5cc13-195">上記で作成したステージング スロットをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-195">Click on the staging slot created above.</span></span> <span data-ttu-id="5cc13-196">デプロイ スロットはアプリと見なされるため、**[すべてのリソース]** では App Service リソースとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-196">Remember, a deployment slot is considered as an app, and hence, it will appear as an App Service resource under **All Resources**.</span></span>

1. <span data-ttu-id="5cc13-197">ステージング デプロイ スロットのページが表示されたら、**[デプロイ オプション]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-197">Once you arrive to the staging deployment slot page, go to **Deployment options**.</span></span>

    <span data-ttu-id="5cc13-198">コンピューター上にローカルに存在する最初のコミットが、Azure portal にアップロードされていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-198">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.</span></span>

    <span data-ttu-id="5cc13-199">App Service 内のリモート Git リポジトリにコードをローカルにプッシュすると、Azure によってこの操作が記録されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-199">When you push your code locally to the remote Git repository in App Service, Azure records this operation.</span></span>

    <span data-ttu-id="5cc13-200">Azure にコードをプッシュするたびに、新しいレコードと、コンピューター上でローカルに変更をコミットするときに入力したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-200">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

    ![[デプロイ オプション] ページの最近の Git リポジトリ デプロイを示す Azure portal のスクリーンショット。](../media/7-staging-deployment-slot-after-uploading-files.png)

1. <span data-ttu-id="5cc13-202">**ステージング スロット**の URL にアクセスしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5cc13-202">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="5cc13-203">URL については既に説明しましたが、その URL を忘れた場合は、いつでも、ステージング デプロイ スロットの **[概要]** ページに移動して、URL を選択できます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-203">The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="5cc13-204">ブラウザーのアドレス バーに、URL として [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/) を入力します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-204">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![ステージング デプロイ スロット Web サイトの Web ブラウザー ビューを示すスクリーンショット。](../media/7-staging-slot-hosted-online.png)

<span data-ttu-id="5cc13-206">ローカル アプリケーションのファイルを Azure 上のステージング デプロイ スロットに正しくアップロードできました。</span><span class="sxs-lookup"><span data-stu-id="5cc13-206">You have successfully uploaded your local application files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="5cc13-207">ステージング デプロイ スロットと運用デプロイ スロットのスワップ</span><span class="sxs-lookup"><span data-stu-id="5cc13-207">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="5cc13-208">Azure でホストされているステージング デプロイ スロットでアプリケーションが起動されて実行されるようになったので、次にこのスロットを運用スロットとスワップします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-208">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="5cc13-209">これを行うには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="5cc13-209">To do so, follow these steps:</span></span>

1. <span data-ttu-id="5cc13-210">以前に作成した元のアプリ ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-210">Navigate to the original app page created earlier.</span></span> <span data-ttu-id="5cc13-211">元の Web アプリは、**[すべてのリソース]** ページで見つかります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-211">You can find the original web app from the **All resources** page.</span></span>

1. <span data-ttu-id="5cc13-212">左側のナビゲーションで **[デプロイ スロット]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-212">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="5cc13-213">ページの上部にある **[スワップ]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-213">Click on the **Swap** button at the top of the page.</span></span>

1. <span data-ttu-id="5cc13-214">Azure portal の表示が **[スワップ]** ページに変わります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-214">The Azure portal navigates you to the **Swap** page.</span></span>

1. <span data-ttu-id="5cc13-215">**[スワップ]** フィールドで、**[スワップ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-215">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="5cc13-216">**[ソース]** フィールドで、**[ステージング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-216">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="5cc13-217">**[ターゲット]** フィールドで、**[運用]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5cc13-217">For the **Destination** field, select **Production**.</span></span>

    ![デプロイ スロットのスワップ ページを示す Azure portal のスクリーンショット。](../media/7-swap-blade.png)

1. <span data-ttu-id="5cc13-219">ページの下部にある **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-219">Click on the **OK** button at the bottom of the page.</span></span>

1. <span data-ttu-id="5cc13-220">Azure でスワップ プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-220">Azure starts the swapping process.</span></span> <span data-ttu-id="5cc13-221">スワップする Web アプリのサイズにもよりますが、通常、この操作には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="5cc13-221">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="5cc13-222">操作が終了したら、ウェブ アプリの URL にアクセスします。これは、ポータル内のアプリ サービスの [概要] ページにあります: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/)。</span><span class="sxs-lookup"><span data-stu-id="5cc13-222">Once the operation ends, visit your web app URL; you can find it in the Overview page for your app service in the portal: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![プライマリ Web アプリとしてホストされるようになった以前のステージング デプロイ スロットの Web ブラウザー ビューを示すスクリーンショット。](../media/7-web-app-page.png)

    <span data-ttu-id="5cc13-224">スワップ操作が正常に完了しました。</span><span class="sxs-lookup"><span data-stu-id="5cc13-224">The swapping operation has been successful!</span></span> <span data-ttu-id="5cc13-225">ステージング デプロイ スロットにアップロードされ、運用スロットでホストされるようになったコードを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5cc13-225">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.</span></span>

1. <span data-ttu-id="5cc13-226">次に、ステージング スロットの URL ([https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/)) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5cc13-226">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![ステージング デプロイ スロット Web アプリとしてホストされるようになった以前のプライマリ デプロイ スロットの Web ブラウザー ビューを示すスクリーンショット。](../media/7-staging-after-swapping.png)

    <span data-ttu-id="5cc13-228">これで、ステージング デプロイ スロットは、以前は運用スロットで処理されていた元の既定の HTML ファイルを処理するようになりました。</span><span class="sxs-lookup"><span data-stu-id="5cc13-228">The staging deployment slot now serves the original, default HTML files that were previously served from the production slot.</span></span>

<span data-ttu-id="5cc13-229">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="5cc13-229">Congratulations!</span></span> <span data-ttu-id="5cc13-230">Azure にアプリケーション コードが正常にアップロードされ、デプロイ スロットがスワップされました。</span><span class="sxs-lookup"><span data-stu-id="5cc13-230">You have successfully uploaded your application code to Azure and swapped deployment slots.</span></span>
