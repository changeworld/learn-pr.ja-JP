<span data-ttu-id="21522-101">セキュリティに関する最大の課題の 1 つが、保護する必要がある領域すべてを確認し、ハッカーより先に脆弱性を見つけられるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="21522-101">One of the biggest problems with security is being able to see all the areas you need to protect and to find vulnerabilities before hackers do.</span></span> <span data-ttu-id="21522-102">Azure には、この作業をはるかかに容易にする、Azure Security Center というサービスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="21522-102">Azure provides a service which makes this much easier called Azure Security Center.</span></span>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="21522-103">Azure Security Center とは</span><span class="sxs-lookup"><span data-stu-id="21522-103">What is Azure Security Center?</span></span>

<span data-ttu-id="21522-104">Azure Security Center (ASC) は、Azure とオンプレミスの両方において、すべてのサービスに対して脅威からの保護を提供する監視サービスです。</span><span class="sxs-lookup"><span data-stu-id="21522-104">Azure Security Center (ASC) is a monitoring service that provides threat protection across all of your services both in Azure, and on-premises.</span></span> <span data-ttu-id="21522-105">次のことが可能です。</span><span class="sxs-lookup"><span data-stu-id="21522-105">It can:</span></span>

- <span data-ttu-id="21522-106">構成、リソース、およびネットワークに基づいて、セキュリティに関するレコメンデーションを提供する。</span><span class="sxs-lookup"><span data-stu-id="21522-106">Provide security recommendations based on your configurations, resources, and networks.</span></span>
- <span data-ttu-id="21522-107">オンプレミスおよびクラウドのワークロード全体のセキュリティ設定を監視し、新しいサービスがオンラインになると必要なセキュリティを自動的に適用する。</span><span class="sxs-lookup"><span data-stu-id="21522-107">Monitor security settings across on-premises and cloud workloads and automatically apply required security to new services as they come online.</span></span>
- <span data-ttu-id="21522-108">すべてのサービスを継続的に監視し、自動的なセキュリティ評価を実行して、潜在的脆弱性を、悪用される前に特定する。</span><span class="sxs-lookup"><span data-stu-id="21522-108">Continuously monitor all your services and perform automatic security assessments to identify potential vulnerabilities before they can be exploited.</span></span>
- <span data-ttu-id="21522-109">機械学習を使用して、マルウェアがサービスや仮想マシンにインストールされる前に検出してブロックする。</span><span class="sxs-lookup"><span data-stu-id="21522-109">Use machine learning to detect and block malware from being installed in your services and virtual machines.</span></span> <span data-ttu-id="21522-110">また、アプリケーションをホワイトリストに登録して、検証対象のアプリのみが実行を許可されるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="21522-110">You can also white-list applications to ensure that only the apps you validate are allowed to execute.</span></span>
- <span data-ttu-id="21522-111">受信攻撃の可能性を分析および特定し、脅威および発生した可能性がある侵害後のアクティビティの調査を支援する。</span><span class="sxs-lookup"><span data-stu-id="21522-111">Analyze and identify potential inbound attacks and help to investigate threats and any post-breach activity which might have occurred.</span></span>
- <span data-ttu-id="21522-112">ポートに対する Just-In-Time アクセスの制御。必要なトラフィックのみがネットワークで許可されるようにして、攻撃対象の領域を減らします。</span><span class="sxs-lookup"><span data-stu-id="21522-112">Just-In-Time access control for ports, reducing your attack surface by ensuring the network only allows traffic you require.</span></span>

<span data-ttu-id="21522-113">ASC は [Center for Internet Security](https://www.cisecurity.org/cis-benchmarks/) (CIS) の推奨事項に含まれています。</span><span class="sxs-lookup"><span data-stu-id="21522-113">ASC is part of the [Center for Internet Security](https://www.cisecurity.org/cis-benchmarks/) (CIS) recommendations.</span></span>

## <a name="activating-azure-security-center"></a><span data-ttu-id="21522-114">Azure Security Center をアクティブ化する</span><span class="sxs-lookup"><span data-stu-id="21522-114">Activating Azure Security Center</span></span>

<span data-ttu-id="21522-115">ASC の利点を考慮し、あなたの会社のセキュリティ チームは、オフィスのすべてのサブスクリプションに対して ASC を有効にすることにしました。</span><span class="sxs-lookup"><span data-stu-id="21522-115">Given the benefits of ASC, the security team at your company has decided that it be turned on for all subscriptions at your office.</span></span> <span data-ttu-id="21522-116">今朝、お使いのアプリケーションに対して ASC を有効にすることを通知するメールが届きました。そこで、その方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="21522-116">You got an email this morning to turn it on for your applications - so let's look at how to do that.</span></span>

1. <span data-ttu-id="21522-117">[Azure portal](https://portal.azure.com?azure-portal=true) を開いて、左側のメニューから **[Azure Security Center]** を選択します。表示されていない場合は、以下に示すように **[すべてのサービス]** を選択し、セキュリティ セクションで **[Security Center]** を見つけます。</span><span class="sxs-lookup"><span data-stu-id="21522-117">Open the [Azure portal](https://portal.azure.com?azure-portal=true) and select **Azure Security Center** from the left hand menu, if you don't see it there, you can select **All services** and find **Security Center** in the security section as shown below.</span></span>

![Azure Security Center を開く](../media-draft/ASC-Menu.png)

2. <span data-ttu-id="21522-119">これまで ASC を開いたことがない場合は、ブレードは **[Getting started]\(作業の開始\)** エントリで開始され、ここでご自身のサブスクリプションをアップグレードするように求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="21522-119">If you have never opened ASC, the blade will start on the **Getting started** entry which might ask you to upgrade your subscription.</span></span> <span data-ttu-id="21522-120">ここではそれを無視し、ページの下部にある **[スキップ]** を選択して、**[概要]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="21522-120">Ignore that for now, select **Skip** at the bottom of the page, and then select **Overview**.</span></span>
    - <span data-ttu-id="21522-121">これにより、お使いのサブスクリプションで使用可能な、すべての要素にわたる "セキュリティの全体像" が表示されます。</span><span class="sxs-lookup"><span data-stu-id="21522-121">This will display the "big security picture" across all the elements available in your subscription.</span></span>
    - <span data-ttu-id="21522-122">これには確認可能な有用な情報が大量に含まれています。</span><span class="sxs-lookup"><span data-stu-id="21522-122">This has a ton of great information you can explore.</span></span>

3. <span data-ttu-id="21522-123">次に、[Policy and Compliance]\(ポリシーとコンプライアンス\) で **[カバレッジ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="21522-123">Next, select **Coverage**, under "Policy and Compliance".</span></span> <span data-ttu-id="21522-124">これにより、ACS の対象となっている (または対象となっていない) サブスクリプション要素が表示されます。</span><span class="sxs-lookup"><span data-stu-id="21522-124">This will display what subscription elements are being covered (or not covered) by ACS.</span></span> <span data-ttu-id="21522-125">ここで、アクセスできるすべてのサブスクリプションに対して ACS を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="21522-125">Here you can turn on ACS for any subscription you have access to.</span></span> <span data-ttu-id="21522-126">[未カバー]、[基本カバレッジ]、[Standard カバレッジ] の 3 つのカバレッジ領域の間で切り替えてみてください。</span><span class="sxs-lookup"><span data-stu-id="21522-126">Try switching between the three coverage areas: "Not covered", "Basic coverage" and "Standard coverage".</span></span>

4. <span data-ttu-id="21522-127">未カバーのサブスクリプションでは、ACS のアクティブ化を求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="21522-127">Subscriptions that are not covered will have a prompt to activate ACS.</span></span> <span data-ttu-id="21522-128">[今すぐアップグレード] ボタンを押すと、サブスクリプション内のすべてのリソースに対して ACS が有効になります。</span><span class="sxs-lookup"><span data-stu-id="21522-128">You can press the "Upgrade Now" button to enable ACS for all the resources in the subscription.</span></span>

![カバレッジをアップグレードする](../media-draft/Upgrade-Now.png)

### <a name="free-vs-standard-pricing-tier"></a><span data-ttu-id="21522-130">Free およびStandard 価格レベル</span><span class="sxs-lookup"><span data-stu-id="21522-130">Free vs. Standard pricing tier</span></span>

<span data-ttu-id="21522-131">ASC は無料の Azure サブスクリプション レベルでも使用できますが、Azure リソースの評価と推奨のみに制限されています。</span><span class="sxs-lookup"><span data-stu-id="21522-131">While you can use a free Azure subscription tier with ASC, it is limited to assessments and recommendations of Azure resources only.</span></span> <span data-ttu-id="21522-132">ASC を本当に活用するには、上記のように、Standard レベルのサブスクリプションにアップグレードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="21522-132">To really leverage ASC, you will need to upgrade to a Standard tier subscription as shown above.</span></span> <span data-ttu-id="21522-133">上記のように、**[カバレッジ]** ブレードの [今すぐアップグレード] ボタンを使用して、お使いのサブスクリプションをアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="21522-133">You can upgrade your subscription through the "Upgrade Now" button in the **Coverage** blade as noted above.</span></span> <span data-ttu-id="21522-134">ASC メニューで、**[Getting Started]\(作業の開始\)** ブレードに切り替えることもできます。ASC メニューでは、サブスクリプション レベルを順を追って変更できます。</span><span class="sxs-lookup"><span data-stu-id="21522-134">You can also switch to the **Getting Started** blade in the ASC menu which will walk you through changing your subscription level.</span></span> <span data-ttu-id="21522-135">価格と機能はリージョンによって異なります。全体的な概要については、[価格のページ](https://azure.microsoft.com/en-us/pricing/details/security-center/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="21522-135">The pricing and features may change based on the region, you can get a full overview on the [pricing page](https://azure.microsoft.com/en-us/pricing/details/security-center/).</span></span> 

> [!NOTE]
> <span data-ttu-id="21522-136">サブスクリプションを Standard レベルにアップグレードするには、サブスクリプションの所有者、サブスクリプションの共同作成者、またはセキュリティ管理者のロールが割り当てられている必要があります。</span><span class="sxs-lookup"><span data-stu-id="21522-136">To upgrade a subscription to the Standard tier, you must be assigned the role of Subscription Owner, Subscription Contributor, or Security Admin.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21522-137">60 日間の試用期間が終了したら、ASC では **1 か月あたり 15 ドル/ノード**が課金され、ご自身のアカウントに請求されます。</span><span class="sxs-lookup"><span data-stu-id="21522-137">After the 60-day trial period is over, ASC is priced at **$15/node per month** and will be billed to your account.</span></span>

## <a name="turning-off-azure-security-center"></a><span data-ttu-id="21522-138">Azure Security Center を無効化する</span><span class="sxs-lookup"><span data-stu-id="21522-138">Turning off Azure Security Center</span></span>

<span data-ttu-id="21522-139">運用システムについては、お使いのリソースすべてで脅威を監視できるように、Azure Security Center を必ず常に有効にしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="21522-139">For production systems, you will definitely want to keep Azure Security Center turned on so it can monitor all your resources for threats.</span></span> <span data-ttu-id="21522-140">ただし、ASC を試す目的で有効にした場合は、課金されないように、無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="21522-140">However, if you are just playing with ASC and turned it on, you will likely want to disable it to ensure you are not charged.</span></span> <span data-ttu-id="21522-141">ここでは、その方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="21522-141">Let's do that now.</span></span>

1. <span data-ttu-id="21522-142">[Azure portal](https://portal.azure.com?azure-portal=true) を開いて、左側のメニューから **[Azure Security Center]** を選択します。表示されていない場合は、以下に示すように **[すべてのサービス]** を選択し、セキュリティ セクションで **[Security Center]** を見つけます。</span><span class="sxs-lookup"><span data-stu-id="21522-142">Open the [Azure portal](https://portal.azure.com?azure-portal=true) and select **Azure Security Center** from the left hand menu, if you don't see it there, you can select **All services** and find **Security Center** in the security section as shown below.</span></span>

![Azure Security Center を開く](../media-draft/ASC-Menu.png)

2. <span data-ttu-id="21522-144">左側のメニューで **[セキュリティ ポリシー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="21522-144">Select **Security Policy** from the left hand menu.</span></span>

3. <span data-ttu-id="21522-145">次に、ASC をダウングレードするサブスクリプションの横にある **[設定の編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="21522-145">Next, select **Edit settings >**, next to the subscription for which you want to downgrade ASC.</span></span>

4. <span data-ttu-id="21522-146">次の画面で、左側のメニューから [価格レベル] を選択します。</span><span class="sxs-lookup"><span data-stu-id="21522-146">On the next screen select "Pricing Tier" from the left hand menu.</span></span>

5. <span data-ttu-id="21522-147">次の図のような新しいページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="21522-147">A new page will appear that looks like the image below.</span></span> <span data-ttu-id="21522-148">"無料 (Azure リソースのみ)" と表示されている左側のボックスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="21522-148">Click on the box on the left that says "Free (for Azure resources only)".</span></span>

![価格レベル](../media-draft/Pricing-Tier.png)

6. <span data-ttu-id="21522-150">画面上部にある [保存] を押します。</span><span class="sxs-lookup"><span data-stu-id="21522-150">Press the "save" button at the top of the screen.</span></span>

<span data-ttu-id="21522-151">これで、ご自身のサブスクリプションが、Azure Security Center の Free レベルにダウングレードされました。</span><span class="sxs-lookup"><span data-stu-id="21522-151">You have now downgraded your subscription to the free tier of Azure Security Center.</span></span>

## <a name="knowledge-check"></a><span data-ttu-id="21522-152">知識チェック</span><span class="sxs-lookup"><span data-stu-id="21522-152">Knowledge Check</span></span>
<span data-ttu-id="21522-153"><!-- TODO: move into yaml --> ASC 機能の横にはすべてチェックマークを付けてください。</span><span class="sxs-lookup"><span data-stu-id="21522-153"><!-- TODO: move into yaml --> Put a checkmark next to all ASC features.</span></span>

* <span data-ttu-id="21522-154">推奨事項 (正)</span><span class="sxs-lookup"><span data-stu-id="21522-154">Recommendations (correct)</span></span>
* <span data-ttu-id="21522-155">軽減策 (誤)</span><span class="sxs-lookup"><span data-stu-id="21522-155">Mitigations (false)</span></span>
* <span data-ttu-id="21522-156">防御 (誤)</span><span class="sxs-lookup"><span data-stu-id="21522-156">Defenses (false)</span></span>
* <span data-ttu-id="21522-157">Just In Time (正)</span><span class="sxs-lookup"><span data-stu-id="21522-157">Just In Time (True)</span></span>
* <span data-ttu-id="21522-158">脅威の検出 (正)</span><span class="sxs-lookup"><span data-stu-id="21522-158">Threat Detection (true)</span></span>

## <a name="summary"></a><span data-ttu-id="21522-159">まとめ</span><span class="sxs-lookup"><span data-stu-id="21522-159">Summary</span></span>

<span data-ttu-id="21522-160"><!-- TODO: need link to module --> これで、アプリケーション、データ、およびネットワークをセキュリティで保護するための最初の (最も重要な) 手順が完了しました。</span><span class="sxs-lookup"><span data-stu-id="21522-160"><!-- TODO: need link to module --> Congratulations, you have taken your first (and most important) step to securing your application, data and network!</span></span> <!--If you want to learn more about Azure Security Center, you can go through the **Protect your resources with Azure Security Center** learning module.-->
