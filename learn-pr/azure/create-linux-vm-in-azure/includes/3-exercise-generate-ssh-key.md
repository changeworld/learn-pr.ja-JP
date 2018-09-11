Azure で Linux 仮想マシンを作成する前に、リモート アクセスについて考慮する必要があります。 ここでは、Linux Web サーバーにサインインしてソフトウェアを構成し、メンテナンスを実行できるようにします。 Azure でホストされている Linux VM を管理するための既定の方法は SSH です。

## <a name="what-is-ssh"></a>SSH とは何ですか?

Secure Shell (SSH) は、セキュリティで保護されていない接続においてセキュリティで保護されたサインインを可能にする、暗号化された接続プロトコルです。 SSH を使用すると、ネットワーク接続を使用して遠隔地からターミナル シェルに接続することができます。

SSH 接続の認証に使用できる方法は 2 つあります。**ユーザー名とパスワード**、または **SSH キー ペア**です。 

> [!TIP]
> SSH は暗号化された接続を提供しますが、SSH 接続でパスワードを使用すると、VM はブルートフォース攻撃やパスワードの推測に対して脆弱になります。 SSH を使用して Linux VM に接続するためのより安全で推奨される方法は、公開キーと秘密キーのペア (SSH キーとも呼ばれます) を使用する方法です。

SSH キー ペアを使用すると、パスワードを使用せずに Linux ベースの Azure 仮想マシンにサインインすることができます。 サインインに使用するコンピューターが数台のみの予定の場合は、この方法の方が安全です。 さまざまな場所から Linux VM にアクセスできるようにする必要がある場合、ユーザー名とパスワードの組み合わせの方が適している可能性があります。 SSH キー ペアには、公開キーと秘密キーという 2 つの部分があります。

* 公開キーは、Linux VM か、公開キー暗号化で使用する他のサービスに配置します。 これは誰とでも共有できます。

* 秘密キーは、SSH 接続するときに自分の身元を証明するために渡すキーです。 これは機密情報として扱い、パスワードやその他のプライベート データと同様に保護します。

同じ単一の公開キーと秘密キーのペアを使用して、複数の Azure VM とサービスにアクセスできます。

## <a name="create-the-ssh-key-pair"></a>SSH キー ペアを作成する

Linux、Windows 10、および macOS では、組み込みの `ssh-keygen` コマンドを使用して、SSH の公開キー ファイルと秘密キー ファイルを生成できます。 

> [!TIP]
> Windows 10 の **Fall Creators Update** には、SSH クライアントが含まれています。 以前のバージョンの Windows では、SSH を使用するために追加のソフトウェアが必要です。[詳細についてはドキュメントを参照してください](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)。または、Windows 用の Linux サブシステムをインストールして、同様の機能を実現することもできます。

> [!NOTE]
> ここでは、生成されたキーを Azure のプライベート ストレージ アカウントに保存する Azure Cloud Shell を使用します。 これらのコマンドは、必要に応じてローカルのシェルに直接入力することもできます。 この方法を採用する場合は、ローカルのセッションを反映するようにこのモジュール全体の手順を調整する必要があります。

Azure VM のキー ペア生成に必要な最小限のコマンドを次に示します。 これにより、2,048 ビット長 (最短) の SSH プロトコル 2 (SSH-2) RSA 公開キーと秘密キーのペアが作成されます。 

Cloud Shell に次のコマンドを入力します。

```bash
ssh-keygen -t rsa -b 2048
```

このツールから、ファイル名と省略可能なパスフレーズの入力が求められます。 既定値のままにします。 `~/.ssh` ディレクトリに、`id_rsa` と `id_rsa.pub` という 2 つの新しいファイルが作成されます。 ファイルが存在する場合は上書きされます。 プロンプトが表示されないようにファイル名やパスフレーズを指定するには、さまざまな方法を利用できます。

### <a name="private-key-passphrase"></a>秘密キーのパスフレーズ

必要に応じて、秘密キーを生成するときにパスフレーズを指定することもできます。 これは、キーの使用時に入力する必要があるパスワードです。 このパスフレーズは、秘密 SSH キー ファイルへのアクセスに使用されます。ユーザー アカウントのパスワードではありません。 

SSH キーにパスフレーズを追加すると、128 ビット AES を使用して秘密キーが暗号化されるため、暗号化解除するパスフレーズなしでは秘密キーを使用できなくなります。 

> [!IMPORTANT]
> パスフレーズを追加することが**強く**推奨されます。 攻撃者によって盗まれた秘密キーにパスフレーズがないと、その秘密キーが使用され、対応する公開キーを持つ任意のサーバーにログインされてしまいます。 パスフレーズによって秘密キーが保護されている場合、攻撃者は秘密キーを使用できないため、Azure 上のインフラストラクチャにセキュリティ レイヤーが追加されたことになります。

パスフレーズを設定する方法の例を次に示します。 このコマンドを実行する必要はありません (ただし、必要な場合は実行できます)。

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
| `-C` | 識別に使用できる公開キーに追加する省略可能なコメント。 通常、これはメール アドレスですが、シンプルなテキストなので、任意の識別方法を使用できます。 |
| `-f` | 秘密キー ファイルの場所とファイル名。 **.pub** を付加された対応する公開キー ファイルが、同じディレクトリに生成されます。 ディレクトリは存在している必要があります。 |
| `-N` | 秘密キーの暗号化に使用されるパスフレーズ。 |

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a>Azure Linux VM で SSH キー ペアを使用する

キー ペアを生成したら、それを Azure 内の Linux VM に使用できます。 VM の作成時に公開キーを指定するか、VM の作成後に追加することができます。 

Azure Cloud Shell でファイルの内容を表示するには、次のコマンドを使用します。

```bash
cat ~/.ssh/id_rsa.pub
```

内容は次のように 1 行です。

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

この値をコピーして、次の演習で使用できるようにします。

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a>Linux VM の作成時に SSH キーを使用する

新しい Linux VM の作成時に SSH キーを適用するには、公開キーの内容をコピーして、Azure portal で指定するか、_または_ Azure CLI または Azure PowerShell コマンドに公開キー ファイルを指定する必要があります。 この方法は、Linux VM を作成するときに使用します。

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a>既存の Linux VM に SSH キーを追加する

既に VM を作成済みの場合は、`ssh-copy-id` コマンドを使用して Linux VM に公開キーをインストールできます。 SSH 用のキーが承認されると、パスワードなしでのサーバーに対するアクセスが許可されます。

それに公開キー ファイルとユーザー名を渡して、キーと関連付けます。

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```
公開キーを用意できたら、Azure portal に切り替えて Linux VM を作成しましょう。

