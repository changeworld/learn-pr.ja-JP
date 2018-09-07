Azure CLI は、Azure に接続して Azure リソース上で管理コマンドを実行することができるコマンドライン プログラムです。 これは Linux、macOS、および Windows 上で実行されます。それにより、管理者および開発者は Web ブラウザーではなく、ターミナルまたはコマンド ライン プロンプト (またはスクリプト) を通してコマンドを実行できるようになります。 たとえば、仮想マシン (VM) を再起動するには、次のようなコマンドを使用します。

 ```bash
 az vm restart -g MyResourceGroup -n MyVm
 ```

Azure CLI は、Azure リソースを管理するためのクロスプラットフォームのコマンドライン ツールであり、Linux、Mac、または Windows コンピューター上でローカルにインストールできます。 Azure CLI は、Azure Cloud Shell を使用してブラウザーから利用することもできます。 いずれの場合も、対話形式またはスクリプト形式で使用できます。 対話形式で使用するには、まずシェル (Windows 上では dmd.exe、Linux または macOS 上では Bash など) を起動し、シェルのプロンプトでコマンドを発行します。 反復的なタスクを自動化するには、選択したシェルのスクリプト構文を使用して、複数の CLI コマンドを 1 つのシェル スクリプトにまとめて、スクリプトを実行します。

## <a name="how-to-install-azure-cli"></a>Azure CLI のインストール方法

Linux および macOS では、パッケージ マネージャーを使用して PowerShell Core をインストールします。 推奨されるパッケージ マネージャーは、OS とディストリビューションによって異なります。
- Linux の場合: **apt-get** (Ubuntu)、**yum** (Red Hat)、**zypper** (OpenSUSE)
- Mac の場合: **Homebrew**

Azure CLI は Microsoft リポジトリ内で利用できるので、まずパッケージ マネージャーにそのリポジトリを追加する必要があります。

Windows の場合、Azure CLI をインストールするには、MSI ファイルをダウンロードして実行します。

## <a name="using-the-azure-cli-in-scripts"></a>Azure CLI をスクリプトで使用する

スクリプトで Azure CLI コマンドを使用する場合は、"シェル" や、スクリプトを実行するために使用する環境に関する問題を認識している必要があります。 たとえば、bash シェルでは、変数を設定するときに次の構文を使用します。

 ```bash
 variable="value"
 variable=integer
 ```

PowerShell 環境を使用して Azure CLI スクリプトを実行する場合は、変数にはこの構文を使用する必要があります。

 ```powershell
 $variable="value"
 $variable=integer
 ```

## <a name="summary"></a>まとめ

ローカル コンピューターで Azure CLI を使用して Azure リソースを管理するには、その前に Azure CLI をインストールする必要があります。 Windows、Linux、macOS のそれぞれでインストール手順は異なりますが、インストールした後には、コマンドはプラットフォーム間で共通です。 
