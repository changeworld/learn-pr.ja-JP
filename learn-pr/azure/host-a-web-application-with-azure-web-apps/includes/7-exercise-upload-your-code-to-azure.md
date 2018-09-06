<span data-ttu-id="176c0-101">この演習では、ASP.NET Core アプリケーションを Azure にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="176c0-101">In this exercise, you'll upload your ASP.NET Core application to Azure.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="176c0-102">ステージング デプロイ スロットを作成する</span><span class="sxs-lookup"><span data-stu-id="176c0-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="176c0-103">左側のナビゲーションで **[デプロイ スロット]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-103">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="176c0-104">次に示すように **[デプロイ スロット]** ブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="176c0-104">Azure opens the **Deployment slots** blade as shown below.</span></span>

    ![デプロイ スロット](../media-draft/7-deployment-slot-blade.png)

1. <span data-ttu-id="176c0-106">[デプロイ スロット] ブレードの上部ナビゲーション バーで **[+ スロットの追加]** ボタンを見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-106">Locate and click the **+ Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="176c0-107">次に示すような **[スロットの追加]** ブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="176c0-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    ![スロットの追加](../media-draft/7-new-deployment-slot-blade.png)

    <span data-ttu-id="176c0-109">デプロイ スロットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="176c0-109">Give your deployment slot a name.</span></span> <span data-ttu-id="176c0-110">この例では、**Staging** という名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="176c0-110">In this case, use **Staging**.</span></span>

    <span data-ttu-id="176c0-111">**[構成のソース]** を選択するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="176c0-111">To choose a **Configuration Source**, you have 2 options.</span></span>

    1. <span data-ttu-id="176c0-112">他のスロットから、または Azure で作成された Web アプリから、構成要素を選択して複製します。</span><span class="sxs-lookup"><span data-stu-id="176c0-112">Select to clone the configuration elements from any other slot or web app created on Azure.</span></span>

    1. <span data-ttu-id="176c0-113">複製しない構成要素を選択できます。</span><span class="sxs-lookup"><span data-stu-id="176c0-113">You can choose not to clone any configuration elements.</span></span> <span data-ttu-id="176c0-114">この例では、**[既存のスロットから構成を複製しない]** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-114">In this case, you select the option **Don't clone configuration from an existing slot**.</span></span>

    <span data-ttu-id="176c0-115">2 番目のオプション **[既存のスロットから構成を複製しない]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-115">Choose the second option, **Don't clone configuration from an existing slot**.</span></span>

1. <span data-ttu-id="176c0-116">ブレードの下部にある **[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-116">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="176c0-117">デプロイ スロットが正常に作成されると、Web アプリの **[デプロイ スロット]** ブレードに表示が戻ります。</span><span class="sxs-lookup"><span data-stu-id="176c0-117">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="176c0-118">ここで、先ほど作成した新しいデプロイ スロットを確認できます。</span><span class="sxs-lookup"><span data-stu-id="176c0-118">Now, you can see the new deployment slot that you have just created.</span></span>

    ![作成されたデプロイ スロット](../media-draft/7-deployment-slot-created.png)

1. <span data-ttu-id="176c0-120">新しいデプロイ スロットを探してクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-120">Locate and click the new deployment slot.</span></span>

1. <span data-ttu-id="176c0-121">新しく作成されたデプロイ スロットの **[概要]** ページに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="176c0-121">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![ステージング デプロイ スロット](../media-draft/7-deployment-slot-staging.png)

    <span data-ttu-id="176c0-123">ステージング デプロイ スロットの **URL** に注意してください。</span><span class="sxs-lookup"><span data-stu-id="176c0-123">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="176c0-124">このユニットを始めるときに見たものとまったく同じ URL です。</span><span class="sxs-lookup"><span data-stu-id="176c0-124">It is the exact same URL you saw previously at the beginning of this unit.</span></span>

    <span data-ttu-id="176c0-125">デプロイ スロットは、Azure 内の Web アプリとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="176c0-125">A deployment slot is treated as a web app inside Azure.</span></span> <span data-ttu-id="176c0-126">ただし、元の Web アプリの子という特別な種類であり、元の Web アプリとスワップすることができます。</span><span class="sxs-lookup"><span data-stu-id="176c0-126">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="176c0-127">**URL** をクリックすると、Azure portal で初めて作成した Web アプリに対して Azure で作成されたものと同じ既定のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-127">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="176c0-128">ステージング デプロイ スロットが正常に作成されたので、次に、**デプロイ資格情報**を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-128">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="176c0-129">デプロイ資格情報を作成する</span><span class="sxs-lookup"><span data-stu-id="176c0-129">Create deployment credentials</span></span>

<span data-ttu-id="176c0-130">Azure では、実際のデプロイ プロセスを始める前に、デプロイ資格情報を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-130">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="176c0-131">そのため、独自のデプロイ資格情報を作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="176c0-131">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="176c0-132">左側のナビゲーションで **[デプロイ資格情報]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-132">Locate and click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="176c0-133">次に示すような **[デプロイ資格情報]** ブレードに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="176c0-133">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    ![デプロイ資格情報](../media-draft/7-deployment-credentials.png)

    <span data-ttu-id="176c0-135">適切な**ユーザー名**と**パスワード**を入力し、パスワードをもう一度入力して確認します。</span><span class="sxs-lookup"><span data-stu-id="176c0-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > <span data-ttu-id="176c0-136">忘れないように、ユーザー名とパスワードを書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="176c0-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="176c0-137">後で Azure へのコードのアップロードとデプロイを始めるときに必要になります。</span><span class="sxs-lookup"><span data-stu-id="176c0-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    <span data-ttu-id="176c0-138">ユーザー名とパスワードを決めた後、**[デプロイ資格情報]** ブレードの上部にある **[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-138">Once you decide on the username and password, locate and click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="176c0-139">デプロイ資格情報が正常に作成されたので、次に、**デプロイ オプション**を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-139">Now that the deployment credentials are created successfully, you need to configure **deployment options**.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="176c0-140">デプロイ オプションとしてローカル Git リポジトリを使用する</span><span class="sxs-lookup"><span data-stu-id="176c0-140">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="176c0-141">ここでは、コードのアップロード プロセスを開始できるように、Azure にローカル Git リポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="176c0-141">Now, it is time to create a local Git repository in Azure so that you can start the process of uploading your code.</span></span>

1. <span data-ttu-id="176c0-142">左側のナビゲーションで **[デプロイ オプション]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-142">Locate and click the **Deployment options** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="176c0-143">次に示すような **[デプロイ オプション]** ブレードに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="176c0-143">The Azure portal navigates to the **Deployment options** blade as shown below.</span></span>

    ![デプロイ オプション](../media-draft/7-deployment-options.png)

1. <span data-ttu-id="176c0-145">**[ソースの選択] > [必要な設定の構成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-145">Click on **Choose source / Configure required settings**.</span></span>

    ![デプロイ資格情報](../media-draft/7-deployment-sources.png)

1. <span data-ttu-id="176c0-147">構成して使用できるオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-147">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="176c0-148">この例では、**[ローカル Git リポジトリ]** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-148">In our case, choose the **Local Git Repository** option.</span></span>

    ![ローカル Git リポジトリ](../media-draft/7-local-git-repo.png)

1. <span data-ttu-id="176c0-150">ブレードの下部にある **[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-150">Locate and click the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="176c0-151">左側のナビゲーションで **[概要]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-151">Locate and click the **Overview** menu item on the left-side navigation.</span></span>

    ![Git が構成されたデプロイ スロット](../media-draft/7-staging-after-setting-git.png)

    <span data-ttu-id="176c0-153">2 つの重要な情報に注意してください。</span><span class="sxs-lookup"><span data-stu-id="176c0-153">Notice two important pieces of information:</span></span>
    - <span data-ttu-id="176c0-154">**[Git/デプロイメント ユーザー名]**: 後で Azure 上のローカル Git リポジトリに接続するときに使用する資格情報です。</span><span class="sxs-lookup"><span data-stu-id="176c0-154">**Git/Deployment username**: These are the credentials that you will use later on to connect to the local Git repository on Azure.</span></span>

    - <span data-ttu-id="176c0-155">**[Git クローン URL]**: ローカル Web アプリケーション リポジトリに対する**リモート**として使用するローカル Git リポジトリの URL です。</span><span class="sxs-lookup"><span data-stu-id="176c0-155">**Git clone url**: This is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="176c0-156">ここで、ステージング デプロイ スロットへのコードのアップロードを始めます。</span><span class="sxs-lookup"><span data-stu-id="176c0-156">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="176c0-157">コンピューターに Git をインストールする</span><span class="sxs-lookup"><span data-stu-id="176c0-157">Install Git on your machine</span></span>

<span data-ttu-id="176c0-158">Ubuntu 18.04 コンピューターに Git をインストールします。</span><span class="sxs-lookup"><span data-stu-id="176c0-158">I will be installing Git on my Ubuntu 18.04 machine.</span></span>

1. <span data-ttu-id="176c0-159">新しい**ターミナル** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="176c0-159">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="176c0-160">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-160">Type the following command.</span></span> <span data-ttu-id="176c0-161">Ubuntu のユーザー パスワードの入力を求めるプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-161">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="176c0-162">更新が成功したら、次のコマンドを入力して Git をローカルにインストールします。</span><span class="sxs-lookup"><span data-stu-id="176c0-162">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="176c0-163">コンピューターへの Git のインストールに同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="176c0-163">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="176c0-164">Git がインストールされたことを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-164">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="176c0-165">インストールが成功した場合、次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-165">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="176c0-166">ユーザーの名前とメール アドレスを提供することで、Git の設定を構成するのが常によい方法です。</span><span class="sxs-lookup"><span data-stu-id="176c0-166">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="176c0-167">そのためには、次のコマンドを発行する必要があります。名前とメール アドレスのプレースホルダーを置き換えます。中かっこ (`{}`) は省きます。</span><span class="sxs-lookup"><span data-stu-id="176c0-167">For that, you need to issue the following commands, replacing the placeholders for name and email, without the curly braces (`{}`):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="176c0-168">Git によってユーザーの情報が記録されたことを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-168">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="176c0-169">次のように、名前とメール アドレスが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="176c0-169">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="176c0-170">Web アプリ用にローカル Git リポジトリを初期化する</span><span class="sxs-lookup"><span data-stu-id="176c0-170">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="176c0-171">Git を使い始めるには、Web アプリ用にローカル Git リポジトリを初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-171">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="176c0-172">新しい**ターミナル** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="176c0-172">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="176c0-173">Web アプリのコンテンツ ルート フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="176c0-173">Navigate to the content root folder of your web app.</span></span> <span data-ttu-id="176c0-174">次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-174">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

    ![Web アプリのコンテンツ ルート フォルダー](../media-draft/7-web-app-content-root-folder.png)

1. <span data-ttu-id="176c0-176">次のコマンドを発行して、新しい Git リポジトリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="176c0-176">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="176c0-177">コマンドが成功すると、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-177">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/your-user/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="176c0-178">すべての Web アプリ ファイルを Git にステージングします。</span><span class="sxs-lookup"><span data-stu-id="176c0-178">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="176c0-179">次に、Git に Web アプリ ファイルを認識させます。</span><span class="sxs-lookup"><span data-stu-id="176c0-179">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="176c0-180">そのためには、Git によって**ステージング**されるように、作業ディレクトリのすべてのファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="176c0-180">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="176c0-181">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-181">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="176c0-182">上のコマンドでは、"." によって表されるすべてのファイルが、Git のステージング状態に追加されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-182">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="176c0-183">次に、Git に変更をコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-183">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="176c0-184">Git にファイルをステージングした後は、ローカル コンピューター上の **Git コミット履歴**にファイルをコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-184">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="176c0-185">そのためには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-185">You do that by typing the following command:</span></span>

   `git commit -m "Initial create"`

   <span data-ttu-id="176c0-186">`commit` コマンドでは、作成しているコミットに関するメッセージを含めるための `-m` 引数が受け付けられます。</span><span class="sxs-lookup"><span data-stu-id="176c0-186">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="176c0-187">後で、コードを Azure にプッシュするときに、この特定のコミットに格納されている同じメッセージを見ることができます。</span><span class="sxs-lookup"><span data-stu-id="176c0-187">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="176c0-188">ローカル Git リポジトリに対するリモートを追加する</span><span class="sxs-lookup"><span data-stu-id="176c0-188">Add a remote for the local Git repository</span></span>

<span data-ttu-id="176c0-189">この時点では、新しいローカル Git リポジトリが正常に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="176c0-189">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="176c0-190">さらに、すべての Web アプリ ファイルを Git にコミットしてあります。</span><span class="sxs-lookup"><span data-stu-id="176c0-190">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="176c0-191">残っているのは、ローカル Git リポジトリを Azure でホストされているリポジトリに接続するための**リモート**を追加することです。</span><span class="sxs-lookup"><span data-stu-id="176c0-191">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="176c0-192">そのためには、以下のことを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="176c0-192">To do so, you need to:</span></span>

1. <span data-ttu-id="176c0-193">前の手順で表示された **Git クローン URL** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="176c0-193">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="176c0-194">コピーした後、**ターミナル** ウィンドウに戻り、次の Git コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="176c0-194">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="176c0-195">上の Git コマンドでは、ローカル Git リポジトリが Azure でホストされているリポジトリにフックされます。</span><span class="sxs-lookup"><span data-stu-id="176c0-195">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="176c0-196">これで、ローカル Git リポジトリとリモート Git リポジトリの間でプッシュとプルを始めることができます。</span><span class="sxs-lookup"><span data-stu-id="176c0-196">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="176c0-197">上記のコマンドを確認するには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-197">To verify the above command, type the following Git command:</span></span>

    ```
    git remote -v
    ```

    <span data-ttu-id="176c0-198">上のコマンドでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-198">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="176c0-199">コードを Azure にプッシュする</span><span class="sxs-lookup"><span data-stu-id="176c0-199">Push your code to Azure</span></span>

<span data-ttu-id="176c0-200">ローカル Git リポジトリを Azure 上のリモート Git リポジトリにフックしたので、アプリを開発してビルドした後、Azure に Web アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="176c0-200">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="176c0-201">**master** ブランチを Azure 上のリモート Git リポジトリにプッシュするには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="176c0-201">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="176c0-202">前に **[デプロイ資格情報]** セクションで構成したパスワードの入力を求められます。</span><span class="sxs-lookup"><span data-stu-id="176c0-202">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="176c0-203">パスワードを入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="176c0-203">Enter your password and hit Enter.</span></span> <span data-ttu-id="176c0-204">Git で、ステージング デプロイ スロットに構成されている Azure リモート Git リポジトリへのコミットされたファイルのアップロードが開始されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-204">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="176c0-205">Web アプリが Azure にアップロードされたことを確認する</span><span class="sxs-lookup"><span data-stu-id="176c0-205">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="176c0-206">Azure portal にログインします。</span><span class="sxs-lookup"><span data-stu-id="176c0-206">Log in to the Azure portal.</span></span>

1. <span data-ttu-id="176c0-207">左側のナビゲーションで **[すべてのリソース]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-207">Locate and click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="176c0-208">それまでに Azure で作成されたすべてのリソースの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-208">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="176c0-209">上で作成したステージング スロットを見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-209">Locate and click on the staging slot created above.</span></span> <span data-ttu-id="176c0-210">デプロイ スロットは Web アプリと見なされることを思い出してください。そのため、**[すべてのリソース]** では Web アプリ リソースとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-210">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="176c0-211">ステージング デプロイ スロットのブレードが表示されたら、**[デプロイ オプション]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="176c0-211">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    ![Web アプリをアップロードした後のステージング デプロイ スロット](../media-draft/7-staging-deployment-slot-after-uploading-files.png)

    <span data-ttu-id="176c0-213">コンピューター上にローカルに存在していた最初のコミットが、Azure portal にアップロードされていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="176c0-213">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal!</span></span>

    <span data-ttu-id="176c0-214">Azure 上でホストされているリモート Git リポジトリにコードをローカルにプッシュすると、Azure によってこの操作が記録されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-214">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="176c0-215">Azure にコードをプッシュするたびに、新しいレコードと、コンピューター上でローカルに変更をコミットするときに入力したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-215">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

1. <span data-ttu-id="176c0-216">**ステージング スロット**の URL にアクセスしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="176c0-216">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="176c0-217">URL については上で説明しましたが、その URL を忘れた場合は、いつでも、ステージング デプロイ スロットの **[概要]** ページに移動して、URL を選択できます。</span><span class="sxs-lookup"><span data-stu-id="176c0-217">The URL was mentioned above, however, if your forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="176c0-218">ブラウザーのアドレス バーに次の URL を入力します。[https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/)</span><span class="sxs-lookup"><span data-stu-id="176c0-218">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![オンラインでホストされているステージング スロット](../media-draft/7-staging-slot-hosted-online.png)

<span data-ttu-id="176c0-220">ローカル Web アプリのファイルを Azure 上のステージング デプロイ スロットに正しくアップロードできました。</span><span class="sxs-lookup"><span data-stu-id="176c0-220">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="176c0-221">ステージング デプロイ スロットと運用デプロイ スロットのスワップ</span><span class="sxs-lookup"><span data-stu-id="176c0-221">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="176c0-222">Azure でホストされているステージング デプロイ スロットでアプリケーションが起動されて実行されるようになったので、次にこのスロットを運用スロットとスワップします。</span><span class="sxs-lookup"><span data-stu-id="176c0-222">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="176c0-223">これを行うには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="176c0-223">To do so, follow these steps:</span></span>

1. <span data-ttu-id="176c0-224">ユニット 2 で作成した元の Web アプリ ブレードを探してそこに移動します。</span><span class="sxs-lookup"><span data-stu-id="176c0-224">Locate and navigate to the original web app blade, created in Unit #2.</span></span>

1. <span data-ttu-id="176c0-225">左側のナビゲーションで **[デプロイ スロット]** メニュー項目を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-225">Locate and click the **Deployment slots** menu item on the left-side navigation.</span></span>

    ![Web アプリ デプロイ スロット ブレード](../media-draft/7-web-app-slots.png)

1. <span data-ttu-id="176c0-227">ブレードの上部にある **[スワップ]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-227">Locate and click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="176c0-228">Azure portal の表示が **[スワップ]** ブレードに変わります。</span><span class="sxs-lookup"><span data-stu-id="176c0-228">The Azure portal navigates you to the **Swap** blade.</span></span>

    ![[スワップ] ブレード](../media-draft/7-swap-blade.png)

1. <span data-ttu-id="176c0-230">**[スワップ]** フィールドで、**[スワップ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-230">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="176c0-231">**[ソース]** フィールドで、**[ステージング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-231">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="176c0-232">**[ターゲット]** フィールドで、**[運用]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="176c0-232">For the **Destination** field, select **Production**.</span></span>

1. <span data-ttu-id="176c0-233">ブレードの下部にある **[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="176c0-233">Locate and click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="176c0-234">Azure でスワップ プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="176c0-234">Azure starts the swapping process.</span></span> <span data-ttu-id="176c0-235">スワップする Web アプリのサイズにもよりますが、通常、この操作には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="176c0-235">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="176c0-236">操作が終了したら、Web アプリの URL [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="176c0-236">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![Web アプリ ページ](../media-draft/7-web-app-page.png)

    <span data-ttu-id="176c0-238">スワップ操作が正常に完了しました。</span><span class="sxs-lookup"><span data-stu-id="176c0-238">The swapping operation has been successful!</span></span> <span data-ttu-id="176c0-239">ステージング デプロイ スロットにアップロードされ、運用スロットでもホストされるようになった、コードを確認できます。</span><span class="sxs-lookup"><span data-stu-id="176c0-239">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot!</span></span>

1. <span data-ttu-id="176c0-240">次に、ステージング スロットの URL [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="176c0-240">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![ステージング Web アプリ](../media-draft/7-staging-after-swapping.png)

    <span data-ttu-id="176c0-242">これで、ステージング デプロイ スロットには、以前は運用スロットでホストされていた元の既定の Web アプリケーション ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="176c0-242">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="176c0-243">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="176c0-243">Congratulations!</span></span> <span data-ttu-id="176c0-244">Azure に Web アプリが正常にアップロードされました。</span><span class="sxs-lookup"><span data-stu-id="176c0-244">You have successfully uploaded your web app to Azure!</span></span>
