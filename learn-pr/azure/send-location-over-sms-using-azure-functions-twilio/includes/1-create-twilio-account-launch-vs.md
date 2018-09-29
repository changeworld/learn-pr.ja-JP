> [!TIP]
> <span data-ttu-id="6c481-101">VM にサインインする際に必要なユーザー名とパスワードは **[リソース]** タブにあります。</span><span class="sxs-lookup"><span data-stu-id="6c481-101">The username and password you need to sign in to the VM are located on the **Resources** tab.</span></span>

> [!NOTE]
> <span data-ttu-id="6c481-102">Mac ユーザーであれば場合によっては、VM の起動後、ツール バーでライトニング アイコンを使用するか、指示の隣にある **[リソース]** タブから **Ctrl + Alt + Delete** オプションを使用し、VM のロックを解除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6c481-102">If you are using a Mac, after launching the VM you may need to use either the lightning icon on the toolbar, or the **Ctrl+Alt+Delete** option from the **Resources** tab next to the instructions to unlock the VM.</span></span>


<span data-ttu-id="6c481-103">このモジュールでは、サーバーレス バックエンドでクロスプラットフォームの Xamarin.Forms アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c481-103">In this module, you'll create a cross-platform Xamarin.Forms app with a serverless back end.</span></span> <span data-ttu-id="6c481-104">このアプリではデバイスからユーザーの場所を取得し、電話番号の一覧とともに Azure 関数に送信します。</span><span class="sxs-lookup"><span data-stu-id="6c481-104">This app will get the user's location from their device and send it with a list of phone numbers to an Azure function.</span></span> <span data-ttu-id="6c481-105">その後、関数ではサード パーティ サービス (Twilio) へのバインドを使用して、提供されるすべての電話番号に SMS メッセージとして場所を送信します。</span><span class="sxs-lookup"><span data-stu-id="6c481-105">The function will then use a binding to a third-party service (Twilio) to send your location as an SMS message to all the phone numbers provided.</span></span>

<span data-ttu-id="6c481-106">この処理では、以下の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="6c481-106">This process involves the following steps:</span></span>

1. <span data-ttu-id="6c481-107">アプリでは、デバイス固有の場所 API の抽象化として Xamarin.Essentials を使用して場所をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="6c481-107">The app captures your location using Xamarin.Essentials as an abstraction over device-specific location APIs.</span></span>

1. <span data-ttu-id="6c481-108">場所と電話番号は JSON ペイロードにパッケージ化され、Azure 関数に送信されます。</span><span class="sxs-lookup"><span data-stu-id="6c481-108">The location and phone numbers are packaged up into a JSON payload and sent to an Azure function.</span></span>

1. <span data-ttu-id="6c481-109">Azure 関数では JSON ペイロードをデコードし、SMS メッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c481-109">The Azure function decodes the JSON payload and creates SMS messages.</span></span>

1. <span data-ttu-id="6c481-110">SMS メッセージは [Twilio](https://www.twilio.com/?azure-portal=true) を介して送信されます。</span><span class="sxs-lookup"><span data-stu-id="6c481-110">The SMS messages are sent via [Twilio](https://www.twilio.com/?azure-portal=true).</span></span>

<span data-ttu-id="6c481-111">次の図で、この処理の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="6c481-111">The following illustration shows an overview of this process.</span></span>

![テキスト メッセージを通じて、ロケーションを共有する処理の高水準アーキテクチャを示す図。](../media/1-architecture.png)

## <a name="create-a-twilio-account"></a><span data-ttu-id="6c481-113">Twilio アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="6c481-113">Create a Twilio account</span></span>

<span data-ttu-id="6c481-114">Azure 関数から SMS メッセージを送信できるようにするには、Twilio アカウントが必要になります。</span><span class="sxs-lookup"><span data-stu-id="6c481-114">To be able to send SMS messages from an Azure function, you'll need a Twilio account.</span></span> <span data-ttu-id="6c481-115">作業を開始するには無料アカウントで十分です。</span><span class="sxs-lookup"><span data-stu-id="6c481-115">The free account is more than enough to get started.</span></span>

1. <span data-ttu-id="6c481-116">[twilio.com](https://www.twilio.com?azure-portal=true) に移動します。</span><span class="sxs-lookup"><span data-stu-id="6c481-116">Head to [twilio.com](https://www.twilio.com?azure-portal=true).</span></span>

1. <span data-ttu-id="6c481-117">右上隅にある **[サインアップ]** という赤いボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c481-117">Click the red **Sign Up** button in the top-right corner.</span></span>

1. <span data-ttu-id="6c481-118">詳細を入力し、**[作業開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c481-118">Fill in your details and click **Get Started**.</span></span>

1. <span data-ttu-id="6c481-119">ご自分の電話番号を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6c481-119">You'll need to verify your phone number.</span></span> <span data-ttu-id="6c481-120">Twilio の無料アカウントでは、スパムに使用されないように確認済みの電話番号にのみメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="6c481-120">Twilio free accounts let you send messages only to verified phone numbers to stop them being used for spam.</span></span> <span data-ttu-id="6c481-121">Twilio によって、電話番号を確認するために入力する必要がある確認コードが送信されます。</span><span class="sxs-lookup"><span data-stu-id="6c481-121">Twilio will send you a verification code that you need to enter to verify your phone.</span></span>

1. <span data-ttu-id="6c481-122">**[製品]** タブを選択し、**[プログラム可能 SMS]** をクリックし、**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c481-122">Select the **Products** tab, and click **Programmable SMS**, then click **Continue**.</span></span>

1. <span data-ttu-id="6c481-123">"I'm here" など、最初のプロジェクト用の名前を指定し、**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c481-123">Provide a name for your first project, such as "I'm here", then click **Continue**.</span></span>

1. <span data-ttu-id="6c481-124">チーム メイトを招待する手順はスキップします。</span><span class="sxs-lookup"><span data-stu-id="6c481-124">Skip the step to invite a team mate.</span></span>

1. <span data-ttu-id="6c481-125">Twilio メッセージング ダッシュボードから、**[プロジェクト情報]** パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="6c481-125">From the Twilio messaging dashboard, expand the **Project Info** panel.</span></span>

1. <span data-ttu-id="6c481-126">自分の **[アカウント SID]** と **[認証トークン]** をメモしておいてください。これらの値は後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="6c481-126">Note your **ACCOUNT SID** and **AUTH TOKEN** because you will need these values later.</span></span>

    <span data-ttu-id="6c481-127">Twilio アカウントを作成すると、メッセージの送信元として使用する電話番号が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="6c481-127">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="6c481-128">この電話番号は、Twilio の **[Phone Numbers]\(電話番号\)** ダッシュ ボードで確認できます。</span><span class="sxs-lookup"><span data-stu-id="6c481-128">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span>

1. <span data-ttu-id="6c481-129">Twilio サイトで、左側のメニューの下部にある省略記号を選択します。</span><span class="sxs-lookup"><span data-stu-id="6c481-129">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="6c481-130">次に、*[SUPER NETWORK]\(スーパー ネットワーク\) -> [Phone Numbers]\(電話番号\)* の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6c481-130">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="6c481-131">ピン アイコンを使用して、左側のメニューにこのダッシュ ボードをピン留めすることができます。</span><span class="sxs-lookup"><span data-stu-id="6c481-131">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="6c481-132">*[Manage Numbers]\(番号の管理\) -> [Active Numbers]\(有効な番号\)* の下に、Twilio 番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c481-132">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span>

    ![Twilio 番号を検索する](../media/7-twilio-find-number.png)

    > [!TIP]
    > <span data-ttu-id="6c481-134">有効な番号がまだ与えられていない場合、[Active Numbers]\(有効な番号\) ページの **[開始]** を選択し、番号作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="6c481-134">If you don't have an active number yet, select **Get Started** in the Active Numbers page to begin the process of creating a number.</span></span>

1. <span data-ttu-id="6c481-135">有効な電話番号をメモしておいてください。</span><span class="sxs-lookup"><span data-stu-id="6c481-135">Note your active phone number.</span></span> <span data-ttu-id="6c481-136">このモジュールの後半で使用します。</span><span class="sxs-lookup"><span data-stu-id="6c481-136">It will be used later in this module.</span></span>


> [!NOTE]
> <span data-ttu-id="6c481-137">サインアップすると、SMS メッセージの送信に使用される Twilio 電話番号が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="6c481-137">When you sign up, you will be assigned a Twilio phone number that will be used to send SMS messages.</span></span> <span data-ttu-id="6c481-138">一部の国では、これらの番号では SMS メッセージを送信できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="6c481-138">In some countries, these numbers may not be able to send SMS messages.</span></span> <span data-ttu-id="6c481-139">Twilio のドキュメントに、[制限がある国](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true)の一覧と、[国際番号または英数字の送信者 ID](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true) を使用して SMS メッセージを送信する方法が示されています。</span><span class="sxs-lookup"><span data-stu-id="6c481-139">The Twilio documentation lists [which countries have restrictions](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true), and shows ways to send SMS messages using an [international number or AlphaNumeric sender Id](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).</span></span>

## <a name="launch-visual-studio"></a><span data-ttu-id="6c481-140">Visual Studio を起動する</span><span class="sxs-lookup"><span data-stu-id="6c481-140">Launch Visual Studio</span></span>

<span data-ttu-id="6c481-141">このモジュールでは、仮想マシンを介して利用可能な、Visual Studio 2017 を使用してモバイル アプリと Azure Functions アプリを開発します。</span><span class="sxs-lookup"><span data-stu-id="6c481-141">For this module, you'll develop the mobile app and Azure Functions app using Visual Studio 2017, available via a virtual machine.</span></span> <span data-ttu-id="6c481-142">Xamarin.Forms アプリをビルドして iOS、Android、ユニバーサル Windows プラットフォーム (UWP) を実行できますが、このモジュールでは UWP のみに焦点を合わせ、ラボの仮想マシン内で動作するようにします。</span><span class="sxs-lookup"><span data-stu-id="6c481-142">Although Xamarin.Forms apps can be built to run on iOS, Android, and Universal Windows Platform (UWP), this module will just focus on UWP so that it works inside the lab virtual machine.</span></span>

<span data-ttu-id="6c481-143">VM の [スタート] メニューまたはデスクトップのショートカットから Visual Studio 2017 を起動します。</span><span class="sxs-lookup"><span data-stu-id="6c481-143">Launch Visual Studio 2017 from the VM's Start Menu, or from the desktop shortcut.</span></span>

## <a name="summary"></a><span data-ttu-id="6c481-144">まとめ</span><span class="sxs-lookup"><span data-stu-id="6c481-144">Summary</span></span>

<span data-ttu-id="6c481-145">このユニットでは、SMS メッセージを送信するために使用する Twilio アカウントを作成し、Visual Studio を起動しました。</span><span class="sxs-lookup"><span data-stu-id="6c481-145">In this unit, you created a Twilio account to use to send SMS messages and launched Visual Studio.</span></span> <span data-ttu-id="6c481-146">次は、Xamarin.Forms アプリを作成し、Xamarin.Essentials NuGet パッケージを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6c481-146">Next, you learn how to create a Xamarin.Forms app and add the Xamarin.Essentials NuGet package.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c481-147">このモジュールで集めた Twilio の **アカウント SID**、**認証トークン**、**有効な電話番号**のメモを保存してください。この値は後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="6c481-147">Keep note of the Twilio  **ACCOUNT SID** and **AUTH TOKEN** and **Active Phone Number** values that you gathered in this unit, because you'll need these values later.</span></span>
