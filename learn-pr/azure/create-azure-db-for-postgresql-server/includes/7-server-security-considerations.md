あなたはオンプレミスの PostgreSQL データベースを使っているものとしましょう。 セキュリティのあらゆる面を管理していて、標準的な PostgreSQL サーバー レベルのファイアウォール規則を使用してサーバーへのすべてのアクセスをロック ダウンしました。 今度は、Azure で同じサーバー レベルのファイアウォール規則を構成できることを確認しようと考えています。

## <a name="server-security-considerations-and-connection-methods"></a>サーバーのセキュリティに関する考慮事項と接続方法

Azure Database for PostgreSQL サーバーとデータベースへのアクセスを制限するには、多くのオプションがあります。 ネットワーク アクセスは、ネットワーク、サーバー、またはデータベースのレベルで制限できます。 次のオプションをどれでも使用できます。

- データベース アクセスを制限するためのユーザー アカウント
- ネットワーク アクセスを制限するための仮想ネットワーク
- サーバー アクセスを制限するためのファイアウォール規則

### <a name="authentication-and-authorization"></a>認証と認可

Azure Database for PostgreSQL サーバーは、ネイティブ PostgreSQL 認証をサポートしています。 サーバーの管理者ログインを使用してサーバーに接続し、認証を行うことができます。 また、特定のデータベースに接続するためのユーザーを作成して、アクセスを制限することもあります。

### <a name="what-is-a-virtual-network"></a>仮想ネットワークとは

仮想ネットワークは、Azure ネットワーク内で作成された論理的に分離されたネットワークです。 仮想ネットワークを使用して、他のリソースに接続できる Azure リソースを制御できます。

データベースに接続する Web アプリケーションを実行しているとします。 サブネットを使用して、ネットワークのさまざまな部分を分離します。 サブネットは、IP アドレスの範囲に基づくネットワークの一部です。

これらのサブネットを構成するには、仮想ネットワークを作成して、ネットワークをサブネットに再分割します。 Web アプリケーションはあるサブネット上で動作し、データベースは別のサブネット上で動作します。 各サブネットには、他のネットワークと通信するための独自の規則があります。 これらの規則によって、データベースから Web アプリケーションへのアクセスを制限できます。

### <a name="what-is-a-firewall"></a>ファイアウォールとは

ファイアウォールとは、各要求の送信元 IP アドレスに基づいてサーバーへのアクセスを許可するサービスです。 IP アドレスの範囲を指定するファイアウォール規則を作成します。 これらの許可された IP アドレスからのクライアントのみが、サーバーへのアクセスを許可されます。 一般に、ファイアウォール規則には特定のネットワーク プロトコルとポート情報も含まれます。 たとえば、既定では、PostgreSQL サーバーはポート 5432 で TCP 要求をリッスンします。

### <a name="azure-database-for-postgresql-server-firewall"></a>Azure Database for PostgreSQL サーバーのファイアウォール

Azure Database for PostgreSQL サーバーのファイアウォールは、ユーザーがアクセス許可を持つコンピューターを指定するまで、データベース サーバーへのすべてのアクセスを禁止します。 ファイアウォールの構成を使用して、サーバーへの接続を許可する IP アドレスの範囲を指定できます。 サーバーは常に、PostgreSQL の既定の接続情報を使用します。

![受信したすべての要求の IP アドレスをスキャンする Azure Database for PostgreSQL サーバーのファイアウォールを示す図。 事前定義された一連の有効な IP アドレスからの要求のみがデータベースに転送されます](../media/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a>Azure Database for PostgreSQL サーバーの SSL 接続

Azure Database for PostgreSQL では、クライアント アプリケーションが PostgreSQL サービスに接続するときに、Secure Sockets Layer (SSL) を使用することが推奨されます。 データベース サーバーとクライアント アプリケーション間に SSL 接続を適用すると、サーバーとクライアント間のデータが暗号化されて、"中間者攻撃" などの攻撃から保護されます。 SSL を有効にすると、クライアントとサーバー間の接続が機能するためには、キーの交換と厳密な認証が必要になります。 SSL の使用については、この学習モジュールでは詳しく説明しません。

## <a name="configure-connection-security"></a>接続のセキュリティを構成する

Azure Database for PostgreSQL サーバーのファイアウォールを構成するときの決定事項と手順を見ていきましょう。 前に作成したサーバーに接続する方法も確認します。

サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。 ファイアウォール規則を作成する対象のサーバー リソースに移動します。

次に、**[接続のセキュリティ]** オプションを選択して、右側に接続のセキュリティ ブレードを開きます。

![PostgreSQL データベース リソース ブレードの [接続のセキュリティ] セクションを示す Azure portal のスクリーン ショット](../media/7-db-security-settings.png)

この画面には、いくつかのオプションがあります。 次のことが行えます。

- **[クライアント IP の追加]** ボタンをクリックして、ポータルへのアクセスに使用する IP アドレスをファイアウォールのエントリとして追加します。
- Azure サービスへのアクセスを許可します。 既定では、すべての Azure サービスは PostgreSQL サーバーにアクセス**できません**。
- IP アドレスの範囲を入力して、ファイアウォール規則を追加します。
- SSL 接続を適用します。 このオプションを指定すると、クライアントは SSL 証明書を使用してサーバーに接続することを強制されます。

変更を行ったら、忘れずに入力フィールドの上の **[保存]** アイコンをクリックして更新された構成を保存してください。

### <a name="allow-access-to-azure-services"></a>Azure サービスへのアクセスを許可する

Azure Cloud Shell を使用してサーバーにアクセスしたりサーバーを構成したりするには、**[Azure サービスへのアクセスを許可]** を有効にします。 この手順では、Cloud Shell からのアクセスを許可するように、サーバーの構成にファイアウォール規則を追加します。 この規則は、ユーザーが追加するカスタム規則の 1 つとして表示されることはありません。

**[SSL 接続を強制する]** を無効にする必要もあります。 クライアント接続に SSL が必要な場合、PowerShell はサーバーに接続できません。

これらのオプションはどちらも、正しく構成しないと、コマンド ラインにエラー メッセージが表示されます。

たとえば、Azure サービスへのアクセスを許可しないで、SSL 接続の強制を有効にした場合、ファイアウォールがアクセスをブロックすると次のようなエラーが表示されます。

```output
psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL:  SSL connection is required. Please specify SSL options and retry.
```

### <a name="create-a-firewall-rule-using-the-portal"></a>ポータルを使用してファイアウォール規則を作成する

たとえば、任意の IP アドレスからのアクセスを許可するファイアウォール規則を作成するとします。

> [!WARNING]
> このファイアウォール規則を作成すると、インターネット上のすべての IP アドレスがサーバーへの接続を試みることができます。 クライアントがユーザー名とパスワードなしではサーバーにアクセスできない場合でも、セキュリティへの影響を確実に理解したうえで、慎重にこの規則を有効にします。

それぞれのラベルの付いたフィールドに次のデータを入力して、新しいファイアウォール規則を作成します。

- 規則名: `AllowAll`
- 開始 IP: `0.0.0.0`
- 終了 IP: `255.255.255.255`

ファイアウォール規則を削除するには、削除するルールの末尾にある省略記号 (...) をクリックします。 **[削除]** ボタンをクリックして、規則を削除します。

入力フィールドの上にある **[保存]** アイコンをクリックして、規則の削除をコミットします。

### <a name="create-a-firewall-rule-using-the-azure-cli"></a>Azure CLI を使用してファイアウォール規則を作成する

Azure CLI で `az postgres server firewall-rule create` コマンドを使用して、ファイアウォール規則をサーバーに追加できます。 上記から規則を作成する例を次に示します。

```azurecli
az postgres server firewall-rule create \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --server <server-name> \
  --name AllowAll \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 255.255.255.255
```

サーバーからファイアウォール規則を削除するには、`az postgres server firewall-rule delete` コマンドを使用します。 次に例を示します。

```azurecli
az postgres server firewall-rule delete \
  --name AllowAll \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --server-name <server-name>
```

## <a name="connecting-to-your-server"></a>サーバーへの接続

他の最新のデータベースと同じように、PostgreSQL でも最適なパフォーマンスを実現するには通常のサーバー管理が必要です。 Azure Database for PostgreSQL サーバーに接続して管理するには、多くのオプションがあります。 ここでは、`psql` を使用してサーバーに接続します。

### <a name="what-is-psql"></a>psql とは

`psql` という名前のコマンド ライン ツールは、PostgreSQL サーバーとデータベースを操作するための PostgreSQL の分散対話型ターミナルです。 `psql` は他の PostgreSQL 実装と同じように Azure Database for PostgreSQL で動作し、Azure Cloud Shell に付属します。 `psql` ツールでは、データベースを管理できるだけでなく、これらのデータベースに対してクエリを実行することができます。

`psql` を使うには、PostgreSQL サーバーに正常に接続する必要があります。 `psql` では多数のコマンド ライン パラメーターを使用できます。

- `--host` - 接続先のホスト。
- `--username` - 接続するためのユーザー名/ID。
- `--dbname` - 接続先のデータベースの名前。

> [!TIP]
> サーバー アクセスとデータベース構成を管理するときは、通常、`postgres` 管理データベースに接続します。

完全なコマンドを次に示します。

```bash
psql --host=<server-name>.postgres.database.azure.com
      --username=<admin-user>@<server-name>
      --dbname=<database>
```

接続されると、コマンド プロンプトが表示され、サーバーとデータベースに対してコマンドを実行できます。

Azure Database for PostgreSQL のセキュリティ設定を構成する手順を見てきました。 次のユニットでは、Azure Database for PostgreSQL のセキュリティ設定を構成します。 また、Cloud Shell を使用してサーバーに接続します。
