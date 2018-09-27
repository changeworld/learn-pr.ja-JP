アプリと Azure 関数が完成し、ローカルで実行されています。 このユニットでは、関数を Azure に発行してクラウドで実行します。

> [!Note]
> 関数を Visual Studio から発行します。 これは概念実証、プロトタイプ、学習を始める方法として優れていますが、運用品質のアプリにはこの手法を利用**しないで**ください。 何らかの形式の CI ベースのデプロイを利用してください。 この方法の詳細については、[Azure Functions のデプロイに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true)を参照してください。

## <a name="publishing-your-app-to-azure"></a>アプリを Azure に発行する

Azure 関数は Visual Studio 内から Azure に発行できます。

1. ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。

1. ソリューション エクスプローラーで `ImHere.Functions` アプリを右クリックし、*[発行]* を選択します。

    ![Functions アプリで右クリック発行する](../media/8-right-click-publish.png)

1. **[発行先の選択]** ダイアログから *[Azure 関数アプリ]* を選択し、**[Azure App Service]** には *[新規作成]* を選択します。 **[発行]** をクリックします。

    ![発行先の Azure App Service を新しく作成する](../media/8-pick-publish-target.png)

1. これらの手順の **[リソース]** タブで、ユーザー名とパスワードを使用して Azure にサインインします。 VM を使用するのでなくローカルでこのアプリを構築する場合は、ご利用の Azure アカウントを使用してログインし、ダイアログ上のリンクを使用する必要がある場合は、新しい Azure アカウントを作成します。

1. 値はすべて既定値のままとします。そうすることで、ご自分の Functions アプリを実行するのに必要なインフラストラクチャ全体が作成されるからです。

1. **[作成]** をクリックし、Azure にすべてのリソースをプロビジョニングし、Azure Functions アプリを発行します。

    ![App Service を作成する](../media/8-create-app-service.png)

1. Azure 上で Functions のバージョンを更新するかどうかを質問される場合があります。 次のダイアログ ボックスが表示されたら、**[はい]** を選択して、ご利用の関数アプリが最新の Azure Functions ランタイム バージョンで確実に発行されるようにします。
    ![Azure Functions の更新ダイアログ](../media/8-update-functions-on-azure.png)

プロビジョニングが完了するまで数分かかります。 次のリソースがプロビジョニングされます。

- Azure Functions アプリに必要なファイルを格納するためのストレージ アカウント
- Azure Functions アプリに必要なコンピューティング リソースを管理する App Service プラン
- Azure 関数を実行する App Service

関数が発行され、**https://\<your-app-name\>.azurewebsites.net/api/SendLocation** で呼び出せるようになります。

## <a name="configuring-your-app"></a>アプリの構成

Azure 関数がローカルで実行されていたとき、`local.settings.json` ファイルに格納されていた Twilio 資格情報が使用されていました。 名前が示すように、このファイルは Azure 設定ではなく、ローカル設定用です。 Azure 内で Azure 関数を呼び出すには、`TwilioAccountSid` 設定と `TwilioAuthToken` 設定を構成する必要があります。

1. [発行] タブから **[アプリケーション設定の管理]** オプションをクリックします。

    ![[アプリケーション設定の管理] オプション](../media/8-application-settings-option.png)

1. **[アプリケーション設定]** ダイアログには、ローカル値とリモート値を両方とも含むアプリケーション設定が表示されます。ローカル値はご利用の `local.settings.json` ファイルからのものです。リモート値は Azure でホストされている場合にご利用の関数で使用される値です。 **[TwilioAccountSid]** 値および **[TwilioAuthToken]** 値について、*[ローカル]* ボックスから *[リモート]* ボックスに値をコピーします。

    ![アプリケーション設定で Twilio 資格情報を設定する](../media/8-set-creds-in-app-settings.png)

1. **[OK]** をクリックします。 これにより値が Azure Functions アプリに発行されます。

## <a name="pointing-the-mobile-app-to-azure"></a>モバイル アプリを Azure にポイントする

1. [発行] タブから、値の横にある **[クリップボードにコピー]** ボタンを使用して **[サイト URL]** をコピーします。

    ![[発行] タブからサイトの URL をコピーする](../media/8-copy-site-url.png)

1. `ImHere` プロジェクトから `MainViewModel` を開く

1. `baseUrl` フィールドの値を、[発行] タブからコピーしたサイト URL に変更します。

## <a name="test-it-out"></a>テストする

1. `ImHere.UWP` アプリをスタートアップ アプリとして設定し、実行します。

1. 電話番号を入力し、**[場所の送信]** ボタンをクリックします。

1. 場所は SMS メッセージとして受信してください。

1. "*サービス利用不可*" エラーが返される場合は、ご利用の Functions アプリで使用されている "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージのバージョンを確認してください。それは 3.0.0 ではなく 3.0.0-rc1 である必要があります。
1. "*サービス利用不可*" エラーが返される場合は、ご利用の Functions アプリで使用されている "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージのバージョンを確認してください。それは 3.0.0 **ではなく** 3.0.0-rc1 である必要があります。

## <a name="summary"></a>まとめ

この演習では、Azure Functions プロジェクトを Visual Studio 内から Azure に発行し、アプリケーション設定を構成する方法について学習しました。