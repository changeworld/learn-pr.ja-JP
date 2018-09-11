### <a name="exercise-1-create-a-custom-vision-service-project"></a><span data-ttu-id="549f4-101">演習 1: Custom Vision Service プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="549f4-101">Exercise 1: Create a Custom Vision Service project</span></span>

<span data-ttu-id="549f4-102">Custom Vision Service で画像分類モデルを構築するための最初のステップは、プロジェクトを作成することです。</span><span class="sxs-lookup"><span data-stu-id="549f4-102">The first step in building an image-classification model with the Custom Vision Service is to create a project.</span></span> <span data-ttu-id="549f4-103">この演習では、Custom Vision Service ポータルを使用して Custom Vision Service プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="549f4-103">In this exercise, you will use the Custom Vision Service portal to create a Custom Vision Service project.</span></span>

1. <span data-ttu-id="549f4-104">ブラウザーで [Custom Vision Service ポータル](https://www.customvision.ai/)を開きます。</span><span class="sxs-lookup"><span data-stu-id="549f4-104">Open the [Custom Vision Service portal](https://www.customvision.ai/) in your browser.</span></span> <span data-ttu-id="549f4-105">**[サインイン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="549f4-105">Then click **Sign In**.</span></span> 
 
    ![Custom Vision Service ポータルにサインインする](../images/portal-sign-in.png)

    <span data-ttu-id="549f4-107">"Custom Vision Service ポータルにサインインする"__</span><span class="sxs-lookup"><span data-stu-id="549f4-107">_Signing in to the Custom Vision Service portal_</span></span>

1. <span data-ttu-id="549f4-108">サインインを求められたら、Microsoft アカウントの資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="549f4-108">If you are asked to sign in, do so using the credentials for your Microsoft account.</span></span> <span data-ttu-id="549f4-109">このアプリにユーザー情報へのアクセスを許可するように求められたら、**[はい]** をクリックし、プロンプトが表示されたらサービス使用条件に同意します。</span><span class="sxs-lookup"><span data-stu-id="549f4-109">If you are asked to let this app access your info, click **Yes**, and if prompted, agree to the terms of service.</span></span>

1. <span data-ttu-id="549f4-110">**[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="549f4-110">Click **New Project** to create a new project.</span></span>
  
    ![Custom Vision Service プロジェクトを作成する](../images/portal-click-new-project.png)

    <span data-ttu-id="549f4-112">"Custom Vision Service プロジェクトを作成する"__</span><span class="sxs-lookup"><span data-stu-id="549f4-112">_Creating a Custom Vision Service project_</span></span>

1. <span data-ttu-id="549f4-113">[新しいプロジェクト] ダイアログで、プロジェクトに "Artworks" と名前を付け、ドメインとして **[全般]** が選択されていることを確認して、**[プロジェクトの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="549f4-113">In the "New project" dialog, name the project "Artworks," ensure that **General** is selected as the domain, and click **Create project**.</span></span>

    > <span data-ttu-id="549f4-114">ドメインは、指定した種類の画像に合わせてモデルを最適化しています。</span><span class="sxs-lookup"><span data-stu-id="549f4-114">A domain optimizes a model for specific types of images.</span></span> <span data-ttu-id="549f4-115">たとえば、含まれている食品の種類別または料理の民族性別に食品の画像を分類することが目標の場合は、[食料] ドメインを選択すると便利です。</span><span class="sxs-lookup"><span data-stu-id="549f4-115">For example, if your goal is to classify food images by the types of food they contain or the ethnicity of the dishes, then it might be helpful to select the Food domain.</span></span> <span data-ttu-id="549f4-116">提供されているどのドメインとも一致しないシナリオの場合、または選択するドメインがわからない場合は、[全般] ドメインを選択します。</span><span class="sxs-lookup"><span data-stu-id="549f4-116">For scenarios that don't match any of the offered domains, or if you are unsure of which domain to choose, select the General domain.</span></span>

    ![Custom Vision Service プロジェクトを作成する](../images/portal-create-project.png)

    <span data-ttu-id="549f4-118">"Custom Vision Service プロジェクトを作成する"__</span><span class="sxs-lookup"><span data-stu-id="549f4-118">_Creating a Custom Vision Service project_</span></span>

<span data-ttu-id="549f4-119">次の手順は、プロジェクトに画像をアップロードし、それらの画像にタグを割り当てて分類することです。</span><span class="sxs-lookup"><span data-stu-id="549f4-119">The next step is to upload images to the project and assign tags to those images to classify them.</span></span>