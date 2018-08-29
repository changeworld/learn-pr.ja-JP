## <a name="motivation"></a><span data-ttu-id="60468-101">目的</span><span class="sxs-lookup"><span data-stu-id="60468-101">Motivation</span></span>
<span data-ttu-id="60468-102">たとえば、あなたの会社が Azure リソースを管理するためのコマンドライン ソリューションとして、Azure CLI を選択したとします。</span><span class="sxs-lookup"><span data-stu-id="60468-102">Suppose your company has chosen the Azure CLI as your command-line solution for managing Azure resources.</span></span> <span data-ttu-id="60468-103">管理者と開発者は、コマンドとスクリプトをブラウザー経由ではなくローカルで実行することを好みます。</span><span class="sxs-lookup"><span data-stu-id="60468-103">Your administrators and developers prefer to run their commands and scripts locally rather than through a browser.</span></span> <span data-ttu-id="60468-104">チームは、Linux、macOS、および Windows を実行するマシンを使用しています。</span><span class="sxs-lookup"><span data-stu-id="60468-104">The team uses machines that run Linux, macOS, and Windows.</span></span> <span data-ttu-id="60468-105">これらのすべてのデバイスで Azure CLI が実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="60468-105">You need to get the Azure CLI working on all their devices.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="60468-106">Azure CLI とは</span><span class="sxs-lookup"><span data-stu-id="60468-106">What is the Azure CLI?</span></span>
<span data-ttu-id="60468-107">Azure CLI は、Azure に接続して Azure リソース上で管理コマンドを実行することができるコマンドライン プログラムです。</span><span class="sxs-lookup"><span data-stu-id="60468-107">The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources.</span></span> <span data-ttu-id="60468-108">たとえば、仮想マシン (VM) を再起動するには、次のようなコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="60468-108">For example, to restart a Virtual Machine (VM), you would use a command like the following:</span></span>

 ```bash
 az vm restart -g MyResourceGroup -n MyVm
 ```

<span data-ttu-id="60468-109">Azure CLI は、Azure リソースを管理するためのクロスプラットフォームのコマンドライン ツールであり、Linux、Mac、または Windows コンピューター上でローカルにインストールできます。</span><span class="sxs-lookup"><span data-stu-id="60468-109">The Azure CLI provides cross-platform command-line tools for managing Azure resources, and can be installed locally on Linux, Mac, or Windows computers.</span></span> <span data-ttu-id="60468-110">Azure CLI は、Azure Cloud Shell を使用してブラウザーから利用することもできます。</span><span class="sxs-lookup"><span data-stu-id="60468-110">The Azure CLI can also be used from a browser through the Azure Cloud Shell.</span></span> <span data-ttu-id="60468-111">いずれの場合も、対話形式またはスクリプト形式で使用できます。</span><span class="sxs-lookup"><span data-stu-id="60468-111">In both cases, it can be used interactively or scripted.</span></span> <span data-ttu-id="60468-112">対話形式で使用するには、まずシェル (Windows 上では dmd.exe、Linux または macOS 上では Bash など) を起動し、シェルのプロンプトでコマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="60468-112">For interactive use, you first launch a shell such as cmd.exe on Windows or Bash on Linux or macOS and then issue the command at the shell prompt.</span></span> <span data-ttu-id="60468-113">反復的なタスクを自動化するには、選択したシェルのスクリプト構文を使用して、複数の CLI コマンドを 1 つのシェル スクリプトにまとめて、スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="60468-113">To automate repetitive tasks, you assemble the CLI commands into a shell script using the script syntax of your chosen shell and then execute the script.</span></span>

## <a name="how-to-install-azure-cli"></a><span data-ttu-id="60468-114">Azure CLI のインストール方法</span><span class="sxs-lookup"><span data-stu-id="60468-114">How to install Azure CLI</span></span>
<span data-ttu-id="60468-115">Linux および macOS では、パッケージ マネージャーを使用して PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="60468-115">On both Linux and macOS, you use a package manager to install PowerShell Core.</span></span> <span data-ttu-id="60468-116">推奨されるパッケージ マネージャーは、OS とディストリビューションによって異なります。</span><span class="sxs-lookup"><span data-stu-id="60468-116">The recommended package manager differs by OS and distribution:</span></span>
- <span data-ttu-id="60468-117">Linux の場合: **apt-get** (Ubuntu)、**yum** (Red Hat)、**zypper** (OpenSUSE)</span><span class="sxs-lookup"><span data-stu-id="60468-117">Linux: **apt-get** on Ubuntu, **yum** on Red Hat, and **zypper** on OpenSUSE</span></span>
- <span data-ttu-id="60468-118">Mac の場合: **Homebrew**</span><span class="sxs-lookup"><span data-stu-id="60468-118">Mac: **Homebrew**</span></span>

<span data-ttu-id="60468-119">Azure CLI は Microsoft リポジトリ内で利用できるので、まずパッケージ マネージャーにそのリポジトリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60468-119">The Azure CLI is available in the Microsoft repository, so you'll first need to add that repository to your package manager.</span></span>

<span data-ttu-id="60468-120">Windows の場合、Azure CLI をインストールするには、MSI ファイルをダウンロードして実行します。</span><span class="sxs-lookup"><span data-stu-id="60468-120">On Windows, you install the Azure CLI by downloading and running an MSI file.</span></span>

## <a name="using-the-azure-cli-in-scripts"></a><span data-ttu-id="60468-121">Azure CLI をスクリプトで使用する</span><span class="sxs-lookup"><span data-stu-id="60468-121">Using the Azure CLI in scripts</span></span>
<span data-ttu-id="60468-122">スクリプトで Azure CLI コマンドを使用する場合は、"シェル" や、スクリプトを実行するために使用する環境に関する問題を認識している必要があります。</span><span class="sxs-lookup"><span data-stu-id="60468-122">If you want to use the Azure CLI commands in scripts, you need to be aware of any issues around the "shell" or environment used for running the script.</span></span> <span data-ttu-id="60468-123">たとえば、bash シェルでは、変数を設定するときに次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="60468-123">For example, in a bash shell, the following syntax is used when setting variables:</span></span>

 ```bash
 variable="string"
 variable=integer
 ```

<span data-ttu-id="60468-124">PowerShell 環境を使用して Azure CLI スクリプトを実行する場合は、変数にはこの構文を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60468-124">If you use a PowerShell environment for running Azure CLI scripts, you'll need to use this syntax for variables:</span></span>

 ```powershell
 $variable="string"
 $variable=integer
 ```

## <a name="summary"></a><span data-ttu-id="60468-125">まとめ</span><span class="sxs-lookup"><span data-stu-id="60468-125">Summary</span></span>
<span data-ttu-id="60468-126">ローカル コンピューターで Azure CLI を使用して Azure リソースを管理するには、その前に Azure CLI をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60468-126">The Azure CLI must be installed before it can be used to manage Azure resources from a local computer.</span></span> <span data-ttu-id="60468-127">Windows、Linux、macOS のそれぞれでインストール手順は異なりますが、インストールした後には、コマンドはプラットフォーム間で共通です。</span><span class="sxs-lookup"><span data-stu-id="60468-127">The installation steps vary for Windows, Linux, and macOS, but once installed, the commands are common across platforms.</span></span> 
