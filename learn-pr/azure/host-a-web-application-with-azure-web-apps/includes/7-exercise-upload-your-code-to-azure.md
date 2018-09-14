<span data-ttu-id="1a3a7-101">この演習では、ASP.NET Core アプリケーションを Azure Web アプリにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-101">In this exercise, you'll upload your ASP.NET Core application to an Azure Web App.</span></span>

## <a name="create-a-staging-deployment-slot"></a><span data-ttu-id="1a3a7-102">ステージング デプロイ スロットを作成する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-102">Create a staging deployment slot</span></span>

1. <span data-ttu-id="1a3a7-103">以前に作成した Web アプリ リソースを開きます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-103">Open the Web App resource you created previously.</span></span> <span data-ttu-id="1a3a7-104">リソース ブレードを閉じた場合は、**[すべてのリソース]** でアプリを検索するか、**[リソース グループ]** でアプリが属するリソース グループを検索することでもう一度見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-104">If you have closed its resource blade, you can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.</span></span>

1. <span data-ttu-id="1a3a7-105">左側のナビゲーションで **[デプロイ スロット]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-105">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1a3a7-106">**[デプロイ スロット]** ブレードで、上部ナビゲーション バーの **[スロットの追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-106">Inside the **Deployment slots** blade, click the **Add Slot** button on the top navigation bar of the deployment slots blade.</span></span>

1. <span data-ttu-id="1a3a7-107">次に示すような **[スロットの追加]** ブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-107">The Azure portal opens the **Add a slot** blade as shown below.</span></span>

    1. <span data-ttu-id="1a3a7-108">デプロイ スロットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-108">Give your deployment slot a name.</span></span> <span data-ttu-id="1a3a7-109">例では `staging` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-109">In this case, use `staging`.</span></span>

    2. <span data-ttu-id="1a3a7-110">**[構成のソース]** を選択するときは、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-110">To choose a **Configuration Source**, you have two options.</span></span>

        * <span data-ttu-id="1a3a7-111">既存のデプロイ スロット、または Azure で作成された Web アプリから構成要素を複製します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-111">You can choose to clone the configuration elements from any existing deployment slot or Web App created on Azure.</span></span>
        * <span data-ttu-id="1a3a7-112">または、構成要素を複製しないことを選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-112">Or you can choose not to clone any configuration elements.</span></span> <span data-ttu-id="1a3a7-113">**[既存のスロットから構成を複製しない]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-113">Select the option **Don't clone configuration from an existing slot**.</span></span>

        <span data-ttu-id="1a3a7-114">このデプロイ スロットでは、2 番目のオプションの **[既存のスロットから構成を複製しない]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-114">For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**.</span></span> <span data-ttu-id="1a3a7-115">スロットを直接構成します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-115">You will configure it directly.</span></span>

    ![新しいステージング デプロイ スロットの構成を示す Azure portal のスクリーンショット。](../media/7-new-deployment-slot-blade.png)

1. <span data-ttu-id="1a3a7-117">ブレードの下部にある **[OK]** をクリックして、新しいデプロイ スロットを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-117">Click the **OK** button at the bottom of the blade to create your new deployment slot.</span></span>

1. <span data-ttu-id="1a3a7-118">デプロイ スロットが正常に作成されると、Web アプリの **[デプロイ スロット]** ブレードに表示が戻ります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-118">Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.</span></span>

    <span data-ttu-id="1a3a7-119">ここで、先ほど作成した新しいデプロイ スロットを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-119">Now, you can see the new deployment slot that you have just created.</span></span>

    ![作成した新しいスロットが含まれた [デプロイ スロット] ブレードを示す Azure portal のスクリーンショット。](../media/7-deployment-slot-created.png)

1. <span data-ttu-id="1a3a7-121">新しいデプロイ スロットを選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-121">Select the new deployment slot.</span></span>

1. <span data-ttu-id="1a3a7-122">新しく作成されたデプロイ スロットの **[概要]** ページに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-122">The Azure portal navigates to the **Overview** page of the newly created deployment slot.</span></span>

    ![ステージング デプロイ スロット](../media/7-deployment-slot-staging.png)

    <span data-ttu-id="1a3a7-124">ステージング デプロイ スロットの **URL** に注意してください。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-124">Notice the **URL** of the staging deployment slot.</span></span> <span data-ttu-id="1a3a7-125">スロット名が追加され、以前に表示されていたものとは異なる URL になっています。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-125">It is a different URL from what you saw previously, with the slot name appended.</span></span>

    <span data-ttu-id="1a3a7-126">デプロイ スロットは、Azure 内で完全な Web アプリとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-126">A deployment slot is treated as a full Web App inside Azure.</span></span> <span data-ttu-id="1a3a7-127">ただし、元の Web アプリの子という特別な種類であり、元の Web アプリとスワップすることができます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-127">However, it is a special type that is a child of the original web app and can be swapped with the original web app.</span></span>

    <span data-ttu-id="1a3a7-128">**URL** をクリックすると、Azure portal で初めて作成した Web アプリに対して Azure で作成されたものと同じ既定のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-128">If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.</span></span>

<span data-ttu-id="1a3a7-129">ステージング デプロイ スロットが正常に作成されたので、次に、**デプロイ資格情報**を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-129">Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.</span></span>

## <a name="create-deployment-credentials"></a><span data-ttu-id="1a3a7-130">デプロイ資格情報を作成する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-130">Create deployment credentials</span></span>

<span data-ttu-id="1a3a7-131">Azure では、実際のデプロイ プロセスを始める前に、デプロイ資格情報を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-131">Azure requires deployment credentials to be set up before you can start the actual deployment process.</span></span> <span data-ttu-id="1a3a7-132">そのため、独自のデプロイ資格情報を作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-132">For that reason, you will learn how to create your own deployment credentials.</span></span>

1. <span data-ttu-id="1a3a7-133">左側のナビゲーションで、**[デプロイ資格情報]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-133">Click the **Deployment credentials** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1a3a7-134">次に示すような **[デプロイ資格情報]** ブレードに表示が変わります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-134">The Azure portal navigates to the **Deployment credentials** blade as shown below.</span></span>

    <span data-ttu-id="1a3a7-135">適切な**ユーザー名**と**パスワード**を入力し、パスワードをもう一度入力して確認します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-135">Enter a **username** and **password** of your choice, and then confirm your password once again.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a3a7-136">忘れないように、ユーザー名とパスワードを書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-136">Make sure you write down your username and password so that you don't forget them!</span></span> <span data-ttu-id="1a3a7-137">後で Azure へのコードのアップロードとデプロイを始めるときに必要になります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-137">You will need them later when we start uploading and deploying our code to Azure.</span></span>

    ![必須フィールドに資格情報の例が入力された、ステージング スロットの [デプロイ資格情報] ブレードを示す Azure portal のスクリーンショット。](../media/7-deployment-credentials.png)

1. <span data-ttu-id="1a3a7-139">**[デプロイ資格情報]** ブレードの上部にある **[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-139">Click the **Save** button at the top of the **Deployment credentials** blade.</span></span>

<span data-ttu-id="1a3a7-140">デプロイ資格情報が正常に作成されたので、次に、他のデプロイ オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-140">Now that the deployment credentials are created successfully, you need to configure other deployment options.</span></span>

## <a name="use-a-local-git-repository-as-your-deployment-option"></a><span data-ttu-id="1a3a7-141">デプロイ オプションとしてローカル Git リポジトリを使用する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-141">Use a local Git repository as your deployment option</span></span>

<span data-ttu-id="1a3a7-142">次に、コードのアップロードを開始できるように、Azure でローカル Git リポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-142">Next, we'll create a local Git repository in Azure so you can start uploading your code.</span></span>

1. <span data-ttu-id="1a3a7-143">**staging** デプロイ スロット Web アプリ内で、左側のナビゲーションの **[デプロイ オプション]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-143">Within the **staging** deployment slot Web App, click the **Deployment options** menu item on the left-hand navigation.</span></span>

1. <span data-ttu-id="1a3a7-144">**[デプロイ オプション]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-144">The Azure portal navigates to the **Deployment options** blade.</span></span>

1. <span data-ttu-id="1a3a7-145">**[ソースの選択]** をクリックして必要な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-145">Click on the **Choose Source** to configure the required settings.</span></span>

1. <span data-ttu-id="1a3a7-146">構成して使用できるオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-146">The Azure portal displays the available options that you can configure and use.</span></span> <span data-ttu-id="1a3a7-147">この例では、**[ローカル Git リポジトリ]** オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-147">In our case, choose the **Local Git Repository** option.</span></span>

1. <span data-ttu-id="1a3a7-148">自動的に **[デプロイ オプション]** ブレードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-148">You will be returned to the **Deployment option** blade.</span></span> <span data-ttu-id="1a3a7-149">ブレードの下部にある **[OK]** をクリックしてデプロイ ソースを設定します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-149">Click the **OK** button at the bottom of the blade to set up the deployment source.</span></span>

1. <span data-ttu-id="1a3a7-150">次に、左側のナビゲーションの **[デプロイ センター (プレビュー)]** セクションに移動して、新しいデプロイの詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-150">Now, navigate to the **Deployment Center (Preview)** section on the left-side navigation to view the new deployment details.</span></span>

    ![スロットの Git Clone URI が強調表示された、デプロイ スロットの [デプロイ センター] ブレードを示す Azure portal のスクリーンショット。](../media/7-staging-after-setting-git.png)

    <span data-ttu-id="1a3a7-152">ここで注意すべき重要な情報は **Git Clone URI** です。これは、ローカル Web アプリケーション リポジトリで**リモート**として使用するローカル Git リポジトリの URL です。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-152">The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local web application repository.</span></span>

<span data-ttu-id="1a3a7-153">ここで、ステージング デプロイ スロットへのコードのアップロードを始めます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-153">It is time to start uploading your code to the staging deployment slot.</span></span>

## <a name="install-git-on-your-machine"></a><span data-ttu-id="1a3a7-154">コンピューターに Git をインストールする</span><span class="sxs-lookup"><span data-stu-id="1a3a7-154">Install Git on your machine</span></span>

<span data-ttu-id="1a3a7-155">Linux マシンに Git をインストールします (まだインストールしていない場合)。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-155">If you don't already have it, install Git on your Linux machine.</span></span>

> [!NOTE]
> <span data-ttu-id="1a3a7-156">以下の手順は Ubuntu 18.04 向けです。Git をインストールする手順は、ディストリビューションとバージョンによって異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-156">These instructions are for Ubuntu 18.04, so the steps to install Git may differ for your distribution and version.</span></span> <span data-ttu-id="1a3a7-157">適切な手順については、[Linux Git のインストール手順](https://git-scm.com/download/linux)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-157">Check out the [Linux Git installation instructions](https://git-scm.com/download/linux) for the appropriate steps.</span></span>

1. <span data-ttu-id="1a3a7-158">新しい**ターミナル** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-158">Open a new **Terminal** window</span></span>

1. <span data-ttu-id="1a3a7-159">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-159">Type the following command.</span></span> <span data-ttu-id="1a3a7-160">Ubuntu のユーザー パスワードの入力を求めるプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-160">You will be prompted to enter your Ubuntu user password.</span></span>

    ```console
    sudo apt-get update
    ```

1. <span data-ttu-id="1a3a7-161">更新が成功したら、次のコマンドを入力して Git をローカルにインストールします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-161">Once the update is successful, type the following command to install Git locally.</span></span> <span data-ttu-id="1a3a7-162">コンピューターへの Git のインストールに同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-162">You will be prompted to accept the installation of Git on your machine.</span></span>

    ```console
    sudo apt-get install git-core
    ```

1. <span data-ttu-id="1a3a7-163">Git がインストールされたことを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-163">To verify that Git is now installed, type the following command:</span></span>

    ```console
    git --version
    ```

   <span data-ttu-id="1a3a7-164">インストールが成功した場合、次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-164">If the installation is successful, you will see the following output:</span></span>

    ```console
    git version 2.17.1
    ```

1. <span data-ttu-id="1a3a7-165">ユーザーの名前とメール アドレスを提供することで、Git の設定を構成するのが常によい方法です。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-165">It is always a good practice to configure Git settings by providing your name and email.</span></span> <span data-ttu-id="1a3a7-166">そのためには、次のコマンドを発行する必要があります。プレースホルダー `{your name}` と `{your email}` を、自分の名前とメール アドレスに置き換えます (中かっこ (`{}`) は不要です)。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-166">For that, you need to issue the following commands, replacing the `{your name}` and `{your email}` placeholders with your own name and email (without the `{}` curly braces):</span></span>

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. <span data-ttu-id="1a3a7-167">Git によってユーザーの情報が記録されたことを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-167">To verify that your information has been recorded by Git, type the following command:</span></span>

    ```console
    cat ~/.gitconfig
    ```

   <span data-ttu-id="1a3a7-168">次のように、名前とメール アドレスが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-168">You should be seeing the following, with your name and email shown:</span></span>

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## <a name="initialize-a-local-git-repository-for-your-web-app"></a><span data-ttu-id="1a3a7-169">Web アプリ用にローカル Git リポジトリを初期化する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-169">Initialize a local Git repository for your web app</span></span>

<span data-ttu-id="1a3a7-170">Git を使い始めるには、Web アプリ用にローカル Git リポジトリを初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-170">To start using Git, you need to initialize a local Git repository for the web app.</span></span>

1. <span data-ttu-id="1a3a7-171">新しい**ターミナル** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-171">Open a new **Terminal** window.</span></span>

1. <span data-ttu-id="1a3a7-172">以前に作成した Web アプリのコンテンツ ルート フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-172">Navigate to the content root folder of the web app you created previously.</span></span> <span data-ttu-id="1a3a7-173">次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-173">You can type the following:</span></span>

    ```console
    cd ~/Documents/BestBikeApp/
    ```

1. <span data-ttu-id="1a3a7-174">次のコマンドを発行して、新しい Git リポジトリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-174">Initialize a new Git repository by issuing the following command:</span></span>

    ```console
    git init
    ```

    <span data-ttu-id="1a3a7-175">コマンドが成功すると、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-175">If the command is successful, you receive a message like the following:</span></span>

    ```console
    Initialized empty Git repository in /home/{your-user}/Documents/BestBikeApp/.git/
    ```

1. <span data-ttu-id="1a3a7-176">すべての Web アプリ ファイルを Git にステージングします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-176">Stage all the web app files to Git.</span></span>

   <span data-ttu-id="1a3a7-177">次に、Git に Web アプリ ファイルを認識させます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-177">The next step is to let Git know about your web app files.</span></span> <span data-ttu-id="1a3a7-178">そのためには、Git によって**ステージング**されるように、作業ディレクトリのすべてのファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-178">Do that by adding all the files of the working directory so that they get **staged** by Git.</span></span> <span data-ttu-id="1a3a7-179">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-179">Type the following command:</span></span>

    ```console
    git add .
    ```

    <span data-ttu-id="1a3a7-180">上のコマンドでは、"." によって表されるすべてのファイルが、Git のステージング状態に追加されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-180">The command above adds all files, represented by the ".", to the staging state of Git.</span></span>

1. <span data-ttu-id="1a3a7-181">次に、Git に変更をコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-181">Now, you need to commit your changes to Git.</span></span>

   <span data-ttu-id="1a3a7-182">Git にファイルをステージングした後は、ローカル コンピューター上の **Git コミット履歴**にファイルをコミットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-182">Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine.</span></span> <span data-ttu-id="1a3a7-183">そのためには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-183">You do that by typing the following command:</span></span>

    ```console
   git commit -m "Initial create"
    ```

   <span data-ttu-id="1a3a7-184">`commit` コマンドでは、作成しているコミットに関するメッセージを含めるための `-m` 引数が受け付けられます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-184">The `commit` command accepts  `-m` argument to include a message with the commit you are creating.</span></span> <span data-ttu-id="1a3a7-185">後で、コードを Azure にプッシュするときに、この特定のコミットに格納されている同じメッセージを見ることができます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-185">Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.</span></span>

## <a name="add-a-remote-for-the-local-git-repository"></a><span data-ttu-id="1a3a7-186">ローカル Git リポジトリに対するリモートを追加する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-186">Add a remote for the local Git repository</span></span>

<span data-ttu-id="1a3a7-187">この時点では、新しいローカル Git リポジトリが正常に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-187">At this point, you have successfully initialized a new local Git repository.</span></span> <span data-ttu-id="1a3a7-188">さらに、すべての Web アプリ ファイルを Git にコミットしてあります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-188">In addition, you've committed all of your web app files to Git.</span></span> <span data-ttu-id="1a3a7-189">残っているのは、ローカル Git リポジトリを Azure でホストされているリポジトリに接続するための**リモート**を追加することです。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-189">What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.</span></span>

<span data-ttu-id="1a3a7-190">そのためには、以下のことを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-190">To do so, you need to:</span></span>

1. <span data-ttu-id="1a3a7-191">前の手順で表示された **Git クローン URL** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-191">Copy the **Git clone url** that you saw above.</span></span>

1. <span data-ttu-id="1a3a7-192">コピーした後、**ターミナル** ウィンドウに戻り、次の Git コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-192">Once copied, you go back to the **Terminal** window and issue the following Git command:</span></span>

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    <span data-ttu-id="1a3a7-193">上の Git コマンドでは、ローカル Git リポジトリが Azure でホストされているリポジトリにフックされます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-193">The above Git command hooks your local Git repository to the one hosted on Azure.</span></span> <span data-ttu-id="1a3a7-194">これで、ローカル Git リポジトリとリモート Git リポジトリの間でプッシュとプルを始めることができます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-194">Now, you can start pushing and pulling between the local and remote Git repositories!</span></span>

1. <span data-ttu-id="1a3a7-195">上記のコマンドを確認するには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-195">To verify the above command, type the following Git command:</span></span>

    ```console
    git remote -v
    ```

    <span data-ttu-id="1a3a7-196">上のコマンドでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-196">The command above generates the following output:</span></span>

    ```console
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin  https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## <a name="push-your-code-to-azure"></a><span data-ttu-id="1a3a7-197">コードを Azure にプッシュする</span><span class="sxs-lookup"><span data-stu-id="1a3a7-197">Push your code to Azure</span></span>

<span data-ttu-id="1a3a7-198">ローカル Git リポジトリを Azure 上のリモート Git リポジトリにフックしたので、アプリを開発してビルドした後、Azure に Web アプリをプッシュします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-198">Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.</span></span>

1. <span data-ttu-id="1a3a7-199">**master** ブランチを Azure 上のリモート Git リポジトリにプッシュするには、次の Git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-199">Type the following Git command to push your **master** branch to the remote Git repository on Azure:</span></span>

    ```console
    git push origin master
    ```

1. <span data-ttu-id="1a3a7-200">前に **[デプロイ資格情報]** セクションで構成したパスワードの入力を求められます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-200">You will be prompted to enter the password that you have configured in the **Deployment credentials** section above.</span></span> <span data-ttu-id="1a3a7-201">パスワードを入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-201">Enter your password and hit Enter.</span></span> <span data-ttu-id="1a3a7-202">Git で、ステージング デプロイ スロットに構成されている Azure リモート Git リポジトリへのコミットされたファイルのアップロードが開始されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-202">Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.</span></span>

## <a name="verify-the-web-app-is-uploaded-to-azure"></a><span data-ttu-id="1a3a7-203">Web アプリが Azure にアップロードされたことを確認する</span><span class="sxs-lookup"><span data-stu-id="1a3a7-203">Verify the web app is uploaded to Azure</span></span>

1. <span data-ttu-id="1a3a7-204">Azure portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-204">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="1a3a7-205">左側のナビゲーションで、**[すべてのリソース]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-205">Click on the **All Resources** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1a3a7-206">それまでに Azure で作成されたすべてのリソースの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-206">The Azure portal navigates you to the list of all resources created on Azure so far.</span></span>

1. <span data-ttu-id="1a3a7-207">上記で作成したステージング スロットをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-207">Click on the staging slot created above.</span></span> <span data-ttu-id="1a3a7-208">デプロイ スロットは Web アプリと見なされることを思い出してください。そのため、**[すべてのリソース]** では Web アプリ リソースとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-208">Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.</span></span>

1. <span data-ttu-id="1a3a7-209">ステージング デプロイ スロットのブレードが表示されたら、**[デプロイ オプション]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-209">Once you arrive to the staging deployment slot blade, go to **Deployment options**.</span></span>

    <span data-ttu-id="1a3a7-210">コンピューター上にローカルに存在する最初のコミットが、Azure portal にアップロードされていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-210">You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.</span></span>

    <span data-ttu-id="1a3a7-211">Azure 上でホストされているリモート Git リポジトリにコードをローカルにプッシュすると、Azure によってこの操作が記録されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-211">By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.</span></span>

    <span data-ttu-id="1a3a7-212">Azure にコードをプッシュするたびに、新しいレコードと、コンピューター上でローカルに変更をコミットするときに入力したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-212">Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.</span></span>

    ![[デプロイ オプション] ブレードの最近の Git リポジトリ デプロイを示す Azure portal のスクリーンショット。](../media/7-staging-deployment-slot-after-uploading-files.png)

1. <span data-ttu-id="1a3a7-214">**ステージング スロット**の URL にアクセスしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-214">Let's visit the **staging slot** URL.</span></span> <span data-ttu-id="1a3a7-215">URL については既に説明しましたが、その URL を忘れた場合は、いつでも、ステージング デプロイ スロットの **[概要]** ページに移動して、URL を選択できます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-215">The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.</span></span>

1. <span data-ttu-id="1a3a7-216">ブラウザーのアドレス バーに次の URL を入力します。[https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/)</span><span class="sxs-lookup"><span data-stu-id="1a3a7-216">Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).</span></span>

    ![ステージング デプロイ スロット Web サイトの Web ブラウザー ビューを示すスクリーンショット。](../media/7-staging-slot-hosted-online.png)

<span data-ttu-id="1a3a7-218">ローカル Web アプリのファイルを Azure 上のステージング デプロイ スロットに正しくアップロードできました。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-218">You have successfully uploaded your local web app files to the staging deployment slot on Azure.</span></span>

## <a name="swapping-the-staging-and-production-deployment-slots"></a><span data-ttu-id="1a3a7-219">ステージング デプロイ スロットと運用デプロイ スロットのスワップ</span><span class="sxs-lookup"><span data-stu-id="1a3a7-219">Swapping the staging and production deployment slots</span></span>

<span data-ttu-id="1a3a7-220">Azure でホストされているステージング デプロイ スロットでアプリケーションが起動されて実行されるようになったので、次にこのスロットを運用スロットとスワップします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-220">Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one.</span></span> <span data-ttu-id="1a3a7-221">これを行うには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-221">To do so, follow these steps:</span></span>

1. <span data-ttu-id="1a3a7-222">以前に作成した元の Web アプリ ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-222">Navigate to the original Web App blade created early.</span></span> <span data-ttu-id="1a3a7-223">元の Web アプリは、**[すべてのリソース]** ブレードで見つかります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-223">You can find the original Web App from the **All resources** blade.</span></span>

1. <span data-ttu-id="1a3a7-224">左側のナビゲーションで **[デプロイ スロット]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-224">Click the **Deployment slots** menu item on the left-side navigation.</span></span>

1. <span data-ttu-id="1a3a7-225">ブレードの上部にある **[スワップ]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-225">Click on the **Swap** button at the top of the blade.</span></span>

1. <span data-ttu-id="1a3a7-226">Azure portal の表示が **[スワップ]** ブレードに変わります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-226">The Azure portal navigates you to the **Swap** blade.</span></span>

1. <span data-ttu-id="1a3a7-227">**[スワップ]** フィールドで、**[スワップ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-227">For the **Swap** field, select **Swap**.</span></span>

1. <span data-ttu-id="1a3a7-228">**[ソース]** フィールドで、**[ステージング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-228">For the **Source** field, select **Staging**.</span></span>

1. <span data-ttu-id="1a3a7-229">**[ターゲット]** フィールドで、**[運用]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-229">For the **Destination** field, select **Production**.</span></span>

    ![デプロイ スロットのスワップ ブレードを示す Azure portal のスクリーンショット。](../media/7-swap-blade.png)

1. <span data-ttu-id="1a3a7-231">ブレードの下部にある **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-231">Click on the **OK** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="1a3a7-232">Azure でスワップ プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-232">Azure starts the swapping process.</span></span> <span data-ttu-id="1a3a7-233">スワップする Web アプリのサイズにもよりますが、通常、この操作には数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-233">Usually, this operation takes a few seconds, depending on the size of the web app being swapped.</span></span>

1. <span data-ttu-id="1a3a7-234">操作が終了したら、Web アプリの URL [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-234">Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).</span></span>

    ![プライマリ Web アプリとしてホストされるようになった以前のステージング デプロイ スロットの Web ブラウザー ビューを示すスクリーンショット。](../media/7-web-app-page.png)

    <span data-ttu-id="1a3a7-236">スワップ操作が正常に完了しました。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-236">The swapping operation has been successful!</span></span> <span data-ttu-id="1a3a7-237">ステージング デプロイ スロットにアップロードされ、運用スロットでホストされるようになったコードを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-237">You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.</span></span>

1. <span data-ttu-id="1a3a7-238">次に、ステージング スロットの URL [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-238">Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).</span></span>

    ![ステージング デプロイ スロット Web アプリとしてホストされるようになった以前のプライマリ デプロイ スロットの Web ブラウザー ビューを示すスクリーンショット。](../media/7-staging-after-swapping.png)

    <span data-ttu-id="1a3a7-240">これで、ステージング デプロイ スロットには、以前は運用スロットでホストされていた元の既定の Web アプリケーション ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-240">The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.</span></span>

<span data-ttu-id="1a3a7-241">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-241">Congratulations!</span></span> <span data-ttu-id="1a3a7-242">Azure に Web アプリが正常にアップロードされ、デプロイ スロットがスワップされました。</span><span class="sxs-lookup"><span data-stu-id="1a3a7-242">You have successfully uploaded your web app to Azure and swapped deployment slots.</span></span>
