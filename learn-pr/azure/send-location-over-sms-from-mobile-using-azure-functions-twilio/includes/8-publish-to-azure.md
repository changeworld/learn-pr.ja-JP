アプリと Azure 関数が完成し、ローカルで実行されています。 このユニットでは、関数を Azure に発行してクラウドで実行します。

> このユニットでは、関数を Visual Studio から発行します。 これは概念実証、プロトタイプ、学習を始める方法として優れていますが、運用品質のアプリにはこの手法を利用**しないで**ください。 何らかの形式の CI ベースのデプロイを利用してください。 この方法に関する詳細は、[Azure Functions のデプロイに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment)を参照してください。
>
## <a name="publishing-your-app-to-azure"></a>アプリを Azure に発行する

Azure 関数は Visual Studio 内から Azure に発行できます。

1. ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。

2. ソリューション エクスプローラーで `ImHere.Functions` アプリを右クリックし、*[発行]* を選択します。

    ![Functions アプリで右クリック発行する](../media-drafts/8-right-click-publish.png)

3. **[発行先の選択]** ダイアログから *[Azure 関数アプリ]* を選択し、**[Azure App Service]** には *[新規作成]* を選択します。 **[発行]** をクリックします。

    ![発行先の Azure App Service を新しく作成する](../media-drafts/8-pick-publish-target.png)

4. Azure アカウントを複数お持ちのとき、必要なアカウントが選択されていない場合、右上隅のドロップダウンから Azure アカウントを選択します。

5. Function App に名前を付けます。 この名前は、Azure 全体のすべての Functions アプリで一意にする必要があります。"ImHere-\<YourName\>" のような名前を使用してください。

6. この Functions アプリを作成するサブスクリプションを選択します。

7. **[リソース グループ]** ドロップダウンの横にある **[新規]** ボタンをクリックし、"ImHere" のような名前を付けることで、この Functions アプリに新しいリソース グループを作成します。 リソース グループ名は、Azure 全体で一意にするのではなく、サブスクリプションに対して一意にする必要があります。 次に、 **[OK]** をクリックします

    ![新しいリソース グループを作成する](../media-drafts/8-create-new-resource-group.png)

   新しいリソース グループを作成すると、後のクリーンアップが簡単になります。 リソース グループを削除すると、この Functions アプリに作成したすべてが同時に削除されます。

8. **[ホスティング プラン]** ドロップダウンの横にある **[新規]** ボタンをクリックし、新しいホスティング プランを作成します。 App Service プランの名前は、既定でアプリ名の末尾に "Plan" が付く形式になります。 **[場所]** をお住まいの地域に最も近い場所に設定し、**[サイズ]** を消費に設定します。 次に、 **[OK]** をクリックします

    ![ホスティング プランを構成する](../media-drafts/8-configure-hosting-plan.png)

9. **[ストレージ アカウント]** ドロップダウンの横にある **[新規]** ボタンをクリックし、新しいストレージ アカウントを作成します。 既定の名前が与えられます。既定値をすべてそのまま選択し、**[OK]** をクリックします。

    ![ストレージ アカウントの作成](../media-drafts/8-create-storage-account.png)

10. **[作成]** をクリックし、Azure にすべてのリソースをプロビジョニングし、Azure Functions アプリを発行します。

    ![App Service を作成する](../media-drafts/8-create-app-service.png)

プロビジョニングの実行には 2、3 分かかります。 次のリソースがプロビジョニングされます。

* Azure Functions アプリに必要なファイルを格納するためのストレージ アカウント
* Azure Functions アプリに必要なコンピューティング リソースを管理する App Service プラン
* Azure 関数を実行する App Service

関数が発行され、 https://<your-app-name>.azurewebsites.net/api/SendLocation で呼び出せるようになります。

## <a name="configuring-your-app"></a>アプリの構成

Azure 関数がローカルで実行されていたとき、`local.settings.json` ファイルに格納されていた Twilio 資格情報が使用されていました。 名前が示すように、このファイルは Azure 設定ではなく、ローカル設定用です。 Azure 内で Azure 関数を呼び出すには、`TwilioAccountSid` 設定と `TwilioAuthToken` 設定を構成する必要があります。

1. [発行] タブから **[アプリケーション設定の管理]** オプションをクリックします。

    ![[アプリケーション設定の管理] オプション](../media-drafts/8-application-settings-option.png)

2. **[追加]** ボタンをクリックして、新しい設定を追加します。 これに "TwilioAccountSid" という名前を付け、値を Twilio アカウント SID に設定します。 "TwilioAuthToken" という名前を使用し、認証トークンに対してこの手順を繰り返します。

    ![アプリケーション設定で Twilio 資格情報を設定する](../media-drafts/8-set-creds-in-app-settings.png)

3. Click **OK**.

4. **[発行]** をクリックし、新しいアプリケーション設定で Azure Functions アプリを再発行します。

    ![[発行] ボタン](../media-drafts/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a>モバイル アプリを Azure にポイントする

1. [発行] タブから、値の横にあるコピー ボタンを使用して **[サイト URL]** をコピーします。

    ![[発行] タブからサイトの URL をコピーする](../media-drafts/8-copy-site-url.png)

2. `ImHere` プロジェクトから `MainViewModel` を開く

3. [発行] タブからコピーしたサイト URL に `baseUrl` フィールドの値を変更します。

4. この値のプロトコルを `http` から `https` に変更します。 サイト URL は常に HTTP の使用で与えられますが、Azure 関数を呼び出すには HTTPS を使用する必要があります。

## <a name="test-it-out"></a>テストする

1. `ImHere.UWP` アプリをスタートアップ アプリとして設定し、実行します。

2. 電話番号を入力し、**[場所の送信]** ボタンをクリックします。

3. 場所は SMS メッセージとして受信してください。

## <a name="summary"></a>まとめ

この演習では、Azure Functions プロジェクトを Visual Studio 内から Azure に発行し、アプリケーション設定を構成する方法について学習しました。