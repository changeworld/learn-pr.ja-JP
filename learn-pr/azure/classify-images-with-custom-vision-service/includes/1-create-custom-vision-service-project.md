<span data-ttu-id="92f3c-101">Custom Vision Service で画像分類モデルを構築するための最初のステップは、プロジェクトを作成することです。</span><span class="sxs-lookup"><span data-stu-id="92f3c-101">The first step in building an image-classification model with the Custom Vision Service is to create a project.</span></span> <span data-ttu-id="92f3c-102">このユニットでは、Custom Vision Service ポータルを使用して Custom Vision Service プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="92f3c-102">In this unit, you will use the Custom Vision Service portal to create a Custom Vision Service project.</span></span>

1. <span data-ttu-id="92f3c-103">ブラウザーで [Custom Vision Service ポータル](https://www.customvision.ai/)を開きます。</span><span class="sxs-lookup"><span data-stu-id="92f3c-103">Open the [Custom Vision Service portal](https://www.customvision.ai/) in your browser.</span></span> <span data-ttu-id="92f3c-104">**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="92f3c-104">Then click **Sign In**.</span></span>

    ![Custom Vision Service ポータルへのサインイン](../media-draft/1-portal-sign-in.png)

1. <span data-ttu-id="92f3c-106">サインインを求められたら、Microsoft アカウントの資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="92f3c-106">If you are asked to sign in, do so using the credentials for your Microsoft account.</span></span> <span data-ttu-id="92f3c-107">このアプリにユーザー情報へのアクセスを許可するように求められたら、**[はい]** をクリックし、プロンプトが表示されたらサービス使用条件に同意します。</span><span class="sxs-lookup"><span data-stu-id="92f3c-107">If you are asked to let this app access your info, click **Yes**, and if prompted, agree to the terms of service.</span></span>

1. <span data-ttu-id="92f3c-108">**[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="92f3c-108">Click **New Project** to create a new project.</span></span>
  
    ![Custom Vision Service プロジェクトの作成](../media-draft/1-portal-click-new-project.png)

1. <span data-ttu-id="92f3c-110">[新しいプロジェクト] ダイアログで、プロジェクトに "Artworks" と名前を付け、ドメインとして **[全般]** が選択されていることを確認して、**[プロジェクトの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="92f3c-110">In the "New project" dialog, name the project "Artworks," ensure that **General** is selected as the domain, and click **Create project**.</span></span>

    > <span data-ttu-id="92f3c-111">ドメインは、指定した種類の画像に合わせてモデルを最適化しています。</span><span class="sxs-lookup"><span data-stu-id="92f3c-111">A domain optimizes a model for specific types of images.</span></span> <span data-ttu-id="92f3c-112">たとえば、含まれている食品の種類別または料理の民族性別に食品の画像を分類することが目標の場合は、[食料] ドメインを選択すると便利です。</span><span class="sxs-lookup"><span data-stu-id="92f3c-112">For example, if your goal is to classify food images by the types of food they contain or the ethnicity of the dishes, then it might be helpful to select the Food domain.</span></span> <span data-ttu-id="92f3c-113">提供されているどのドメインとも一致しないシナリオの場合、または選択するドメインがわからない場合は、[全般] ドメインを選択します。</span><span class="sxs-lookup"><span data-stu-id="92f3c-113">For scenarios that don't match any of the offered domains, or if you are unsure of which domain to choose, select the General domain.</span></span>

   ![Custom Vision Service プロジェクトの作成](../media-draft/1-portal-create-project.png)

<span data-ttu-id="92f3c-115">次の手順は、プロジェクトに画像をアップロードし、それらの画像にタグを割り当てて分類することです。</span><span class="sxs-lookup"><span data-stu-id="92f3c-115">The next step is to upload images to the project and assign tags to those images to classify them.</span></span>