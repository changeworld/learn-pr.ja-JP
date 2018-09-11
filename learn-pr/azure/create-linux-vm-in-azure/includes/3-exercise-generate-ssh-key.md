<span data-ttu-id="1f53c-101">Azure で Linux 仮想マシンを作成する前に、リモート アクセスについて考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-101">Before we can create a Linux virtual machine in Azure, we will need to think about remote access.</span></span> <span data-ttu-id="1f53c-102">ここでは、Linux Web サーバーにサインインしてソフトウェアを構成し、メンテナンスを実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1f53c-102">We want to be able to sign into our Linux web server to configure the software and perform maintenance.</span></span> <span data-ttu-id="1f53c-103">Azure でホストされている Linux VM を管理するための既定の方法は SSH です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-103">The default approach to administering Linux VMs hosted in Azure is SSH.</span></span>

## <a name="what-is-ssh"></a><span data-ttu-id="1f53c-104">SSH とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="1f53c-104">What is SSH?</span></span>

<span data-ttu-id="1f53c-105">Secure Shell (SSH) は、セキュリティで保護されていない接続においてセキュリティで保護されたサインインを可能にする、暗号化された接続プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="1f53c-105">Secure Shell (SSH) is an encrypted connection protocol that allows secure sign-ins over unsecured connections.</span></span> <span data-ttu-id="1f53c-106">SSH を使用すると、ネットワーク接続を使用して遠隔地からターミナル シェルに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-106">SSH allows you to connect to a terminal shell from a remote location using a network connection.</span></span>

<span data-ttu-id="1f53c-107">SSH 接続の認証に使用できる方法は 2 つあります。**ユーザー名とパスワード**、または **SSH キー ペア**です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-107">There are two approaches we can use to authenticate an SSH connection: \*\* username/password\*\*, or a **SSH key pair**.</span></span> 

> [!TIP]
> <span data-ttu-id="1f53c-108">SSH は暗号化された接続を提供しますが、SSH 接続でパスワードを使用すると、VM はブルートフォース攻撃やパスワードの推測に対して脆弱になります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-108">Although SSH provides an encrypted connection, using passwords with SSH connections leaves the VM vulnerable to brute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="1f53c-109">SSH を使用して Linux VM に接続するためのより安全で推奨される方法は、公開キーと秘密キーのペア (SSH キーとも呼ばれます) を使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-109">A more secure and preferred method of connecting to a Linux VM with SSH is with a public-private key pair, also known as SSH keys.</span></span>

<span data-ttu-id="1f53c-110">SSH キー ペアを使用すると、パスワードを使用せずに Linux ベースの Azure 仮想マシンにサインインすることができます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-110">With an SSH key pair, you can sign in to Linux-based Azure virtual machines without a password.</span></span> <span data-ttu-id="1f53c-111">サインインに使用するコンピューターが数台のみの予定の場合は、この方法の方が安全です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-111">This is a more secure approach if you only plan to sign into the from a few computers.</span></span> <span data-ttu-id="1f53c-112">さまざまな場所から Linux VM にアクセスできるようにする必要がある場合、ユーザー名とパスワードの組み合わせの方が適している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-112">If you need to be able to access the Linux VM from a variety of locations, a username and password combination might be a better approach.</span></span> <span data-ttu-id="1f53c-113">SSH キー ペアには、公開キーと秘密キーという 2 つの部分があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-113">There are two parts to an SSH key pair: a public key, and a private key.</span></span>

* <span data-ttu-id="1f53c-114">公開キーは、Linux VM か、公開キー暗号化で使用する他のサービスに配置します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-114">The public key is placed on your Linux VM or any other service that you wish to use with public-key cryptography.</span></span> <span data-ttu-id="1f53c-115">これは誰とでも共有できます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-115">This can be shared with anyone.</span></span>

* <span data-ttu-id="1f53c-116">秘密キーは、SSH 接続するときに自分の身元を証明するために渡すキーです。</span><span class="sxs-lookup"><span data-stu-id="1f53c-116">The private key is what you present to your Linux VM when you make an SSH connection, to verify your identity.</span></span> <span data-ttu-id="1f53c-117">これは機密情報として扱い、パスワードやその他のプライベート データと同様に保護します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-117">Consider this confidential information and protect this like you would a password or any other private data.</span></span>

<span data-ttu-id="1f53c-118">同じ単一の公開キーと秘密キーのペアを使用して、複数の Azure VM とサービスにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-118">You can use the same single public-private key pair to access multiple Azure VMs and services.</span></span>

## <a name="create-the-ssh-key-pair"></a><span data-ttu-id="1f53c-119">SSH キー ペアを作成する</span><span class="sxs-lookup"><span data-stu-id="1f53c-119">Create the SSH key pair</span></span>

<span data-ttu-id="1f53c-120">Linux、Windows 10、macOS では、組み込みの `ssh-keygen` コマンドを使用して、SSH の公開キー ファイルと秘密キー ファイルを生成できます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-120">On Linux, Windows 10, and MacOS, you can use the built-in `ssh-keygen` command to generate the SSH public and private key files.</span></span> 

> [!TIP]
> <span data-ttu-id="1f53c-121">Windows 10 の Fall Creators Update には、SSH クライアントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1f53c-121">Windows 10 includes an SSH client with the Fall Creators Update.</span></span> <span data-ttu-id="1f53c-122">以前のバージョンの Windows では、追加のソフトウェアが必要です。Windows で SSH を使用する予定の場合、[詳細についてはドキュメントを参照してください](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)。</span><span class="sxs-lookup"><span data-stu-id="1f53c-122">Earlier versions of Windows require additional software, [check the documentation for full details](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) if you plan to use Windows with SSH.</span></span> <span data-ttu-id="1f53c-123">また、Windows 用の Linux サブシステムをインストールして、同様の機能を実現することもできます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-123">Alternatively, you can install the Linux subsystem for Windows and get the same functionality.</span></span>

> [!NOTE]
> <span data-ttu-id="1f53c-124">ここでは、生成されたキーを Azure のプライベート ストレージ アカウントに保存する Azure Cloud Shell を使用します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-124">We will use the Azure Cloud Shell which will store the generated keys in Azure in your private storage account.</span></span> <span data-ttu-id="1f53c-125">これらのコマンドは、必要に応じてローカルのシェルに直接入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-125">You can also type these commands directly into your local shell if you prefer.</span></span> <span data-ttu-id="1f53c-126">この方法を採用する場合は、ローカルのセッションを反映するようにこのモジュール全体の手順を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-126">You will need to adjust the instructions throughout this module to reflect a local session if you take this approach.</span></span>

<span data-ttu-id="1f53c-127">Azure VM のキー ペア生成に必要な最小限のコマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-127">Here is the minimum command necessary to generate the key pair for an Azure VM.</span></span> <span data-ttu-id="1f53c-128">これにより、2,048 ビット長 (最短) の SSH プロトコル 2 (SSH-2) RSA 公開キーと秘密キーのペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-128">This will create an SSH protocol 2 (SSH-2) RSA public-private key pair with a 2048 bit length (the minimum length).</span></span> 

<span data-ttu-id="1f53c-129">Cloud Shell に次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-129">Type this command into the Cloud Shell.</span></span>

```bash
ssh-keygen -t rsa -b 2048
```

<span data-ttu-id="1f53c-130">このツールから、ファイル名と省略可能なパスフレーズの入力が求められます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-130">The tool will prompt for filenames and an optional passphrase.</span></span> <span data-ttu-id="1f53c-131">既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="1f53c-131">Just take the defaults.</span></span> <span data-ttu-id="1f53c-132">`~/.ssh` ディレクトリに、`id_rsa` と `id_rsa.pub` という 2 つの新しいファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-132">It will create two files: `id_rsa` and `id_rsa.pub` in the `~/.ssh` directory.</span></span> <span data-ttu-id="1f53c-133">ファイルが存在する場合は上書きされます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-133">The files are overwritten if they exist.</span></span> <span data-ttu-id="1f53c-134">プロンプトが表示されないようにファイル名やパスフレーズを指定するには、さまざまな方法を利用できます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-134">There are various options you can use to provide the filename or a passphrase to avoid the prompt.</span></span>

### <a name="private-key-passphrase"></a><span data-ttu-id="1f53c-135">秘密キーのパスフレーズ</span><span class="sxs-lookup"><span data-stu-id="1f53c-135">Private key passphrase</span></span>

<span data-ttu-id="1f53c-136">必要に応じて、秘密キーを生成するときにパスフレーズを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-136">You can optionally provide a passphrase while generating your private key.</span></span> <span data-ttu-id="1f53c-137">これは、キーの使用時に入力する必要があるパスワードです。</span><span class="sxs-lookup"><span data-stu-id="1f53c-137">This is a password you must enter when you use the key.</span></span> <span data-ttu-id="1f53c-138">このパスフレーズは、秘密 SSH キー ファイルへのアクセスに使用されます。ユーザー アカウントのパスワードではありません。</span><span class="sxs-lookup"><span data-stu-id="1f53c-138">This passphrase is used to access the private SSH key file and is not the user account password.</span></span> 

<span data-ttu-id="1f53c-139">SSH キーにパスフレーズを追加すると、128 ビット AES を使用して秘密キーが暗号化されるため、暗号化解除するパスフレーズなしでは秘密キーを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-139">When you add a passphrase to your SSH key, it encrypts the private key using 128-bit AES, so that the private key is useless without the passphrase to decrypt it.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="1f53c-140">パスフレーズを追加することが**強く**推奨されます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-140">It is **strongly** recommended that you add a passphrase.</span></span> <span data-ttu-id="1f53c-141">攻撃者によって盗まれた秘密キーにパスフレーズがないと、その秘密キーが使用され、対応する公開キーを持つ任意のサーバーにログインされてしまいます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-141">If an attacker stole your private key and that key did not have a passphrase, they would be able to use that private key to log in to any servers that have the corresponding public key.</span></span> <span data-ttu-id="1f53c-142">パスフレーズによって秘密キーが保護されている場合、攻撃者は秘密キーを使用できないため、Azure 上のインフラストラクチャにセキュリティ レイヤーが追加されたことになります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-142">If a passphrase protects a private key, it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="1f53c-143">パスフレーズを設定する方法の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-143">Here is an example showing how to set the passphrase.</span></span> <span data-ttu-id="1f53c-144">このコマンドを実行する必要はありません (ただし、必要な場合は実行できます)。</span><span class="sxs-lookup"><span data-stu-id="1f53c-144">You don't need to execute this command (although you can if you want).</span></span>

```bash
ssh-keygen -t rsa -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N someReallySecurePhraseYouWillRemember
```

| <span data-ttu-id="1f53c-145">パラメーター</span><span class="sxs-lookup"><span data-stu-id="1f53c-145">Parameter</span></span> | <span data-ttu-id="1f53c-146">実行内容</span><span class="sxs-lookup"><span data-stu-id="1f53c-146">What it does</span></span> |
|-----------|--------------|
| `-t` | <span data-ttu-id="1f53c-147">作成するキーの種類です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-147">Type of key to create.</span></span> <span data-ttu-id="1f53c-148">**rsa** にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-148">Must be **rsa**.</span></span> |
| `-b` | <span data-ttu-id="1f53c-149">キーのビット数。</span><span class="sxs-lookup"><span data-stu-id="1f53c-149">Number of bits in the key.</span></span> <span data-ttu-id="1f53c-150">最短は 2,048、最長は 4,096 です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-150">Minimum length is 2048; maximum is 4096.</span></span> |
| `-C` | <span data-ttu-id="1f53c-151">識別に使用できる公開キーに追加する省略可能なコメント。</span><span class="sxs-lookup"><span data-stu-id="1f53c-151">An optional comment to append to the public key that can be used to identify it.</span></span> <span data-ttu-id="1f53c-152">通常、これはメール アドレスですが、シンプルなテキストなので、任意の識別方法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-152">Normally this is an email address, but it's simple text, and you can use whatever identification method you prefer.</span></span> |
| `-f` | <span data-ttu-id="1f53c-153">秘密キー ファイルの場所とファイル名。</span><span class="sxs-lookup"><span data-stu-id="1f53c-153">The location and filename of the private key file.</span></span> <span data-ttu-id="1f53c-154">**.pub** を付加された対応する公開キー ファイルが、同じディレクトリに生成されます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-154">A corresponding public key file appended with **.pub** is generated in the same directory.</span></span> <span data-ttu-id="1f53c-155">ディレクトリは存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-155">The directory must exist.</span></span> |
| `-N` | <span data-ttu-id="1f53c-156">秘密キーの暗号化に使用されるパスフレーズ。</span><span class="sxs-lookup"><span data-stu-id="1f53c-156">The passphrase used to encrypt the private key.</span></span> |

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a><span data-ttu-id="1f53c-157">Azure Linux VM で SSH キー ペアを使用する</span><span class="sxs-lookup"><span data-stu-id="1f53c-157">Use the SSH key pair with an Azure Linux VM</span></span>

<span data-ttu-id="1f53c-158">キー ペアを生成したら、それを Azure 内の Linux VM に使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-158">Once you have the key pair generated, you can use it with a Linux VM in Azure.</span></span> <span data-ttu-id="1f53c-159">VM の作成時に公開キーを指定するか、VM の作成後に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-159">You can supply the public key during the VM creation, or add it after the VM has been created.</span></span> 

<span data-ttu-id="1f53c-160">Azure Cloud Shell でファイルの内容を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-160">You can view the contents of the file in the Azure Cloud Shell with the following command.</span></span>

```bash
cat ~/.ssh/id_rsa.pub
```

<span data-ttu-id="1f53c-161">内容は次のように 1 行です。</span><span class="sxs-lookup"><span data-stu-id="1f53c-161">It will be a single line and look something like:</span></span>

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

<span data-ttu-id="1f53c-162">この値をコピーして、次の演習で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1f53c-162">Copy this value off so you can use it in the next exercise.</span></span>

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a><span data-ttu-id="1f53c-163">Linux VM の作成時に SSH キーを使用する</span><span class="sxs-lookup"><span data-stu-id="1f53c-163">Use the SSH key when creating a Linux VM</span></span>

<span data-ttu-id="1f53c-164">新しい Linux VM の作成時に SSH キーを適用するには、公開キーの内容をコピーして、Azure portal で指定するか、_または_ Azure CLI または Azure PowerShell コマンドに公開キー ファイルを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f53c-164">To apply the SSH key while creating a new Linux VM, you will need to copy the contents of the public key and supply it to the Azure portal, _or_ supply the public key file to the Azure CLI or Azure PowerShell command.</span></span> <span data-ttu-id="1f53c-165">この方法は、Linux VM を作成するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="1f53c-165">We'll use this approach when we create our Linux VM.</span></span>

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a><span data-ttu-id="1f53c-166">既存の Linux VM に SSH キーを追加する</span><span class="sxs-lookup"><span data-stu-id="1f53c-166">Add the SSH key to an existing Linux VM</span></span>

<span data-ttu-id="1f53c-167">既に VM を作成済みの場合は、`ssh-copy-id` コマンドを使用して Linux VM に公開キーをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-167">If you have already created a VM, you can install the public key onto your Linux VM with the `ssh-copy-id` command.</span></span> <span data-ttu-id="1f53c-168">SSH 用のキーが承認されると、パスワードなしでのサーバーに対するアクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-168">Once the key has been authorized for SSH, it grants access to the server without a password.</span></span>

<span data-ttu-id="1f53c-169">それに公開キー ファイルとユーザー名を渡して、キーと関連付けます。</span><span class="sxs-lookup"><span data-stu-id="1f53c-169">Pass it the public key file and the username to associate with the key.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```
<span data-ttu-id="1f53c-170">公開キーを用意できたら、Azure portal に切り替えて Linux VM を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="1f53c-170">Now that we have our public key let's switch to the Azure portal and create a Linux VM.</span></span>

