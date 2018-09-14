Azure で Linux 仮想マシンを作成する前に、リモート アクセスについて考慮する必要があります。 ソフトウェアを構成してメンテナンスを実行する Linux web サーバーにサインインできるようにします。 Azure でホストされている Linux VM を管理するための既定の方法は SSH です。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-ssh"></a>SSH とは何ですか?

Secure Shell (SSH) は、セキュリティで保護されていない接続においてセキュリティで保護されたサインインを可能にする、暗号化された接続プロトコルです。 SSH を使用すると、ネットワーク接続を使用して遠隔地からターミナル シェルに接続することができます。

2 つの方法、SSH 接続の認証に使用できます:**ユーザー名とパスワード**、または**SSH キー ペア**します。

> [!TIP]
> SSH では、暗号化された接続は、SSH 接続にパスワードを使用してまま、VM がブルート フォース パスワード攻撃に対して脆弱になります。 SSH を使用した Linux VM への接続より安全かつ推奨される方法は、公開/秘密キー ペアの場合は、その SSH キーとも呼ばれます。

SSH キー ペアを使用すると、パスワードを使用せずに Linux ベースの Azure 仮想マシンにサインインすることができます。 これは、いくつかのコンピューターから、VM にサインインする場合より安全な手法です。 さまざまな場所から Linux VM にアクセスできるようにする必要がある場合、ユーザー名とパスワードの組み合わせの方が適している可能性があります。 SSH キー ペアを 2 つの部分: 公開キーと秘密キー。

* 公開キーは、Linux VM か、公開キー暗号化で使用する他のサービスに配置します。 これは誰とでも共有できます。

* **秘密キー**本人 Linux VM に SSH 接続を作成するときに提示するものです。 これは機密情報として扱い、パスワードやその他のプライベート データと同様に保護します。

同じ単一の公開キーと秘密キーのペアを使用して、複数の Azure VM とサービスにアクセスできます。

## <a name="create-the-ssh-key-pair"></a>SSH キー ペアを作成する

Linux、Windows 10、および macOS では、組み込みの `ssh-keygen` コマンドを使用して、SSH の公開キー ファイルと秘密キー ファイルを生成できます。

> [!TIP]
> Windows 10 の **Fall Creators Update** には、SSH クライアントが含まれています。 以前のバージョンの Windows には、SSH; を使用して、追加のソフトウェアが必要です。[完全な詳細情報のマニュアルを参照](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)します。 また、Windows 用の Linux サブシステムをインストールして、同様の機能を実現することもできます。

> [!NOTE]
> Azure で、プライベート ストレージ アカウントで生成されたキーを格納する、Azure Cloud Shell を使用します。 これらのコマンドは、必要に応じてローカルのシェルに直接入力することもできます。 この方法を採用する場合は、ローカルのセッションを反映するようにこのモジュール全体の手順を調整する必要があります。

[!include[](../../../includes/azure-sandbox-activate.md)]

Azure VM のキー ペア生成に必要な最小限のコマンドを次に示します。 2048 ビット長 (最小の長さ) を持つ、SSH プロトコル 2 (ssh-2) RSA の公開/秘密キー ペアが作成されます。

Cloud Shell には、このコマンドを入力します。

```bash
ssh-keygen -t rsa -b 2048
```

このツールは、ファイルの名前とオプションのパスフレーズに求められます。 既定値のままにします。 `~/.ssh` ディレクトリに、`id_rsa` と `id_rsa.pub` という 2 つの新しいファイルが作成されます。 存在する場合、ファイルが上書きされます。 ファイル名プロンプトを回避するためにパスフレーズを提供するために使用できるさまざまなオプションがあります。

### <a name="private-key-passphrase"></a>秘密キーのパスフレーズ

必要に応じて、秘密キーを生成するときにパスフレーズを指定することもできます。 これは、キーの使用時に入力する必要があるパスワードです。 このパスフレーズは、秘密 SSH キー ファイルへのアクセスに使用されます。ユーザー アカウントのパスワードではありません。

SSH キーにパスフレーズを追加するときに、秘密キーが暗号化を解除するパスフレーズ役に立たないように、128 ビット AES を使用して秘密キーを暗号化します。

> [!IMPORTANT]
> パスフレーズを追加することが**強く**推奨されます。 攻撃者によって盗まれた秘密キーにパスフレーズがないと、その秘密キーが使用され、対応する公開キーを持つ任意のサーバーにログインされてしまいます。 パスフレーズが秘密キーを保護する場合は、その攻撃者によって使用できません。 これは、Azure 上のインフラストラクチャに追加のセキュリティ層を提供します。

パスフレーズを設定する方法の例を次に示します。 (ただしできますをする場合) は、このコマンドを実行する必要はありません。

```bash
ssh-keygen -t rsa -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N someReallySecurePhraseYouWillRemember
```

| パラメーター | 実行内容 |
|-----------|--------------|
| `-t` | 作成するキーの種類です。 **rsa** にする必要があります。 |
| `-b` | キーのビット数。 最短は 2,048、最長は 4,096 です。 |
| `-C` | 識別に使用できる公開キーに追加する省略可能なコメント。 通常、これは、電子メール アドレスが単純なテキストになります。 その結果、必要に応じて任意の識別方法を使用できます。 |
| `-f` | 秘密キー ファイルの場所とファイル名。 **.pub** を付加された対応する公開キー ファイルが、同じディレクトリに生成されます。 ディレクトリは存在している必要があります。 |
| `-N` | 秘密キーの暗号化に使用されるパスフレーズ。 |

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a>Azure Linux VM で SSH キー ペアを使用する

キー ペアを生成したら、それを Azure 内の Linux VM に使用できます。 VM の作成時に、公開キーを指定したり、VM が作成された後で追加できます。

次のコマンドを使用して、Azure Cloud Shell でファイルの内容を表示できます。

```bash
cat ~/.ssh/id_rsa.pub
```

内容は次のように 1 行です。

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

次の手順で使用できるように、この値をコピーします。

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a>Linux VM の作成時に SSH キーを使用する

新しい Linux VM の作成時に SSH キーを適用するには、公開キーの内容をコピーして、Azure portal で指定するか、"_または_"、Azure CLI または Azure PowerShell コマンドに公開キー ファイルを指定する必要があります。 この方法は、Linux VM を作成するときに使用します。

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a>既存の Linux VM に SSH キーを追加する

既に VM を作成済みの場合は、`ssh-copy-id` コマンドを使用して Linux VM に公開キーをインストールできます。 SSH 用のキーが承認されると、パスワードなしでのサーバーに対するアクセスが許可されます。

公開キー ファイルとをキーに関連付けるユーザー名に渡します。

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

あるので、公開キーは、Azure portal に切り替えてし、Linux VM を作成しましょう。
