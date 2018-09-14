イベント ハブを作成して構成したら、イベント データ ストリームの送受信ができるようにアプリケーションを構成する必要があります。

たとえば、支払い処理ソリューションでは、顧客のクレジット カードのデータを収集するために何らかの形式の送信側アプリケーションが使用され、そのクレジット カードが有効であることを確認するために受信側アプリケーションが使用されます。

.NET アプリケーションの場合と比較して、Java アプリケーションの構成方法には違いがありますが、アプリケーションをイベント ハブに接続し、アプリケーションによってメッセージの送受信を正常に行えるようにするには一般的な原則に従います。 そのため、Java 構成テキスト ファイルの編集プロセスは、Visual Studio を使用して .NET アプリケーションを準備するのとは異なりますが、原則は同じです。

## <a name="what-are-the-minimum-event-hub-application-requirements"></a>イベント ハブ アプリケーションの最小要件とは?

イベント ハブにメッセージを送信できるようにアプリケーションを構成するには、次の情報を指定して、アプリケーションによって接続の資格情報が作成されるようにする必要があります。

- イベント ハブ名前空間の名前
- イベント ハブ名
- 共有アクセス ポリシー名
- プライマリ共有アクセス キー

イベント ハブからメッセージを受信できるようにアプリケーションを構成するには、次の情報を指定して、アプリケーションによって接続の資格情報が作成されるようにします。

- イベント ハブ名前空間の名前
- イベント ハブ名
- 共有アクセス ポリシー名
- プライマリ共有アクセス キー
- ストレージ アカウント名
- ストレージ アカウントの接続文字列
- ストレージ アカウントのコンテナー名

Azure Blob Storage 内にメッセージを格納する受信側アプリケーションがある場合は、最初にストレージ アカウントも構成する必要があります。

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>汎用の Standard ストレージ アカウントを作成するための Azure CLI コマンド

1. リソース グループ、およびリソース グループを作成するときに使用した同じ Azure データ センターの場所では、汎用 V2 ストレージ アカウントを作成します。

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```

1. このストレージにアクセスするには、ストレージ アカウントのアクセス キーが必要です。 ストレージ アカウントに関連付けられているアクセス キーを表示します。

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

1. **key1** に関連付けられている値を保存します。

1. また、ストレージ アカウントに関する接続の詳細も必要になります。 ストレージ アカウント用の接続文字列を表示します。

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

1. **connectionString** に関連付けられている値を保存します。

1. ご利用のストレージ アカウント内のコンテナーにメッセージが格納されるようになります。 前の手順からの接続文字列を含む `<connection string>` を使用して、ストレージ アカウント内でコンテナーを作成します。

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a>アプリケーションの GitHub リポジトリを複製するためのシェル コマンド

Git は分散型バージョン管理モデルを使用するコラボレーション ツールであり、ソフトウェア プロジェクトやドキュメント プロジェクトでの共同作業向けに設計されています。 Windows を含む複数のプラットフォームで Git クライアントを利用することができ、Git コマンドラインは Azure Bash Cloud Shell に含まれています。 GitHub は Git リポジトリに対する Web ベースのホスティング サービスです。 

GitHub 内でプロジェクトとしてホストされているアプリケーションがある場合は、**git clone** コマンドを使用してプロジェクトのリポジトリを複製することにより、プロジェクトのローカル コピーを作成することができます。

1. ホーム ディレクトリにリポジトリを複製します。

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a>アプリケーションを準備するためのシェル コマンド

1. **nano** などのエディターを使用して、アプリケーションを編集し、ご利用のイベント ハブ名前空間、イベント ハブ名、共有アクセス ポリシー名、およびプライマリ キーを追加できるようになりました。 Nano は Linux や他の Unix 系のオペレーティング システムで使用できるシンプルなテキスト エディターです。Azure Bash Cloud Shell 内で使用することもできます。 たとえば、**nano** エディターでは次のコマンドを使用して Java アプリケーション ファイルを開くことができます。

    ```azurecli
    nano <application>.java
    ```

1. アプリケーションの開発方法によっては、イベント ハブの構成情報を追加した後でアプリケーションをコンパイルまたはビルドすることが必要な場合があります。 たとえば、次のユニットで使用する Java アプリケーションについては、Apache **Maven** などのツールを使用してビルドする必要があります。 Maven では、.java ファイルが .class ファイルにコンパイルされるか、または .java ファイルが .jar ファイルにパッケージ化されます。 Maven は Bash Cloud Shell から利用することができます。Java アプリケーションをビルドするには、**mvn** コマンドを使用します。 Java アプリケーションをビルドするには、次の mvn コマンドを使用します。

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a>まとめ

イベント ハブ環境に関する特定の情報を使用して、送信側アプリケーションと受信側アプリケーションを構成する必要があります。 ご利用の受信側アプリケーションによってメッセージを Blob Storage に格納する場合は、ストレージ アカウントを作成します。 ご利用のアプリケーションが GitHub 上でホストされている場合は、それをローカル ディレクトリに複製する必要があります。 アプリケーションに名前空間を追加するには、**nano** のようなテキスト エディターを使用します。