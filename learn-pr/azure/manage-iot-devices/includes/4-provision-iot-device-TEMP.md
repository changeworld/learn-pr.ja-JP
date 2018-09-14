Azure IoT Hub が Raspberry Pi などの IoT デバイスからデータを収集する方法の理解になったようになりました転送もそのデータを送信するには、その他の IoT デバイスをプロビジョニングする移動できます。 小規模なプロジェクト (1 ~ 5 デバイス) での IoT デバイスのセットアップの完了は、1 対 1 とできます。 大規模な展開では、プロビジョニングするためより優れたメソッドを必要があります。

Azure IoT Hub Device Provisioning Service により、お客様は Azure IoT hub にゼロタッチのデバイスのプロビジョニングを構成して、時間のかかる時間での 1 つのプロセスは 1 回、クラウドのスケーラビリティが揃っています。 プロセスは、セキュリティで保護されたスケーラブルな方法で何百万というデバイスのプロビジョニングに必要なインフラストラクチャを提供することに注意してください、サプライ チェーンの課題に設計されています。

デバイスの自動プロビジョニングと Device Provisioning Service を今すぐには、IoT Hub でサポートされている HTTP、AMQP、MQTT、AMQP over websockets、および MQTT websocket 経由ですべてのプロトコル サポートしています。 このリリースは、デバイスとクライアントの両方の側の拡張 SDK の言語サポートにも対応します。

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Device Provisioning Service を IoT ハブにリンクする

次に、IoT Hub Device Provisioning Service と IoT ハブをリンクして、Device Provisioning Service がデバイスをそのハブに登録できるようにします。 Device Provisioning Service は、このサービスにリンクされている IoT ハブにのみデバイスをプロビジョニングできます。 次の手順に従います。

1.  **すべてのリソース**Azure portal でのページで、以前に作成した Device Provisioning Service インスタンスをクリックします。

2.  Device Provisioning Service ページで、**[Linked IoT hubs]\(リンクされた IoT ハブ\)** をクリックします。

3.  **[追加]** をクリックします。

4.  **[IoT Hub へのリンクを追加します]** ページで、以下の情報を入力して、**[保存]** をクリックします。

    - **[サブスクリプション]:** IoT ハブを含むサブスクリプションが選択されていることを確認してください。 別のサブスクリプション内にある IoT ハブにリンクすることもできます。

    - **[IoT Hub]:** この Device Provisioning Service インスタンスとリンクする IoT ハブの名前を選択します。

    - **[アクセス ポリシー]:** IoT ハブとのリンクを確立するために使用する資格情報として **[iothubowner]** を選択します。

![ポータルでハブ名を DPS にリンクする](../media-draft/ee6e78754a1d39d86de71fb6872723f3.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Device Provisioning Service で割り当てポリシーを設定する

割り当てポリシーは、デバイスを IoT ハブに割り当てる方法を決定する IoT Hub Device Provisioning Service 設定です。 次の 3 つの割り当てポリシーがサポートされています。

1. **Lowest latency\(最も短い待機時間\)**: デバイスに対する待機時間が最も短いハブに基づいて、デバイスを IoT ハブにプロビジョニングします。

2. **Evenly weighted distribution\(均等に重み付けされた分散\)** (既定値): リンクされた各 IoT ハブにデバイスが同程度にプロビジョニングされます。 これは、既定の設定です。 デバイスを 1 つの IoT ハブにのみプロビジョニングする場合は、この設定のままでかまいません。

3. **Static configuration via the enrollment list\(登録リストによる静的構成\)**: 登録リストの目的の IoT ハブの仕様が、Device Provisioning Service レベルの割り当てポリシーよりも優先されます。

割り当てポリシーを設定するには、Device Provisioning Service ページで **[Manage allocation policy]\(割り当てポリシーの管理\)** をクリックします。 割り当てポリシーが **[Evenly weighted distribution]\(均等に重み付けされた分散\)** (既定値) に設定されていることを確認します。 設定を変更した場合は、作業が終わったら **[保存]** をクリックします。

![割り当てポリシーを管理する](../media-draft/0c5fa5193156f17b4f5d64aab65a414d.png)

## <a name="enroll-the-device"></a>デバイスを登録する

この手順では、Device Provisioning Service にデバイスの一意のセキュリティ アーティファクトを追加する必要があります。 これらのセキュリティ アーティファクトは、デバイスの[構成証明メカニズム](https://docs.microsoft.com/azure/iot-dps/concepts-device#attestation-mechanism)をベースとしています。

TPM ベースのデバイスの場合、次のものが必要となります。

- 各 TPM チップまたはシミュレーションに固有の "*保証キー*"。保証キーは TPM チップの製造元から取得します。 詳細については、「[TPM 保証キーとは](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation#terminology)」をご覧ください。

- 名前空間/スコープ内でデバイスを一意に識別するために使用する "*登録 ID*"。 この ID は、デバイス ID と同じである場合もあれば、異なる場合もあります。 この ID はすべてのデバイスで必須です。 TPM ベースのデバイスでは、登録 ID が TPM 自体から派生している場合があります (TPM 保証キーの SHA-256 ハッシュなど)。

![ポータルに表示された TPM の登録情報](../media-draft/11db90b7128e1cf222a4da45de7cbac8.png)

X.509 ベースのデバイスの場合、次のものが必要となります。

- *.pem* ファイルまたは *.cer* ファイルの形式で、[X.509 チップまたはシミュレーションに発行された証明書](https://docs.microsoft.com/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates)。 個別登録でのデバイスを使用する必要があります。*署名者証明書*、X.509 システムの登録グループを使用する必要、*ルート証明書*します。

   ![ポータルで X.509 構成証明の個々 の登録を追加します。](../media-draft/8d56752f453f27e55dd15b7c894ae406.png)

デバイスを Device Provisioning Service に登録するには、次の 2 つの方法があります。

- **Enrollment Groups\(登録グループ\)**: これは、特定の構成証明メカニズムを共有するデバイスのグループを表します。 必要な初期構成を共有する多数のデバイスがある場合や、すべてのデバイスが同じテナントに配置される場合は、登録グループを使用することをお勧めします。 登録グループの ID 構成証明の詳細については、[セキュリティ](https://docs.microsoft.com/azure/iot-dps/concepts-security#controlling-device-access-to-the-provisioning-service-with-x509-certificates)に関するページを参照してください。

   ![X.509 構成証明のグループ登録ポータルで追加します。](../media-draft/4a9d9ea822887c70f1ff1e4b64b138f1.png)

- **Individual Enrollments\(個別登録\)**: Device Provisioning Service に登録できる 1 つのデバイスのエントリを表します。 個別登録では、構成証明メカニズムとして X.509 証明書または (実際の TPM または仮想 TPM の) SAS トークンを使用できます。 固有の初期構成を必要とするデバイスや、TPM または仮想 TPM を介した SAS トークンのみを構成証明メカニズムとして使用できるデバイスには、個別加入を使用することをお勧めします。 個別登録では、必要な IoT ハブ デバイス ID が指定されている場合があります。

今度は、デバイスの構成証明メカニズムに基づく必要なセキュリティ アーティファクトを使って、Device Provisioning Service インスタンスにデバイスを登録します。

1. Azure Portal にサインインし、左側のメニューの **[すべてのリソース]** をクリックして、Device Provisioning Service を開きます。

2. Device Provisioning Service の概要ブレードで、**[Manage enrollments]\(登録の管理\)** を選択します。 デバイスの設定に従って、**[Individual Enrollments]/(個別登録\)** タブまたは **[Enrollment Groups]\(登録グループ\)** タブを選択します。 上部にある **[追加]** をクリックします。 選択**TPM**または**X.509** id 構成証明として*メカニズム*、既に説明したとおりに適切なセキュリティ アーティファクトを入力します。 新しい **IoT ハブ デバイス ID** を入力できます。 作業が完了したら、**[保存]** をクリックします。

3. デバイスが正常に登録されると、ポータルに次のように表示されます。

![ポータルで正常に完了した TPM 登録](../media-draft/cb277b2e5bc21cd02669775d536e89c0.png)

登録後、プロビジョニング サービスは、デバイスが起動し、後でサービスに接続するまで待機します。 デバイスの初回起動時に、クライアント SDK ライブラリはチップと対話してデバイスからセキュリティ アーティファクトを抽出し、Device Provisioning Service への登録を確認します。

## <a name="start-the-iot-device"></a>IoT デバイスを起動する

IoT デバイスは、実際のデバイスにすることも、シミュレートされたデバイスにすることもできます。 IoT デバイスが Device Provisioning Service インスタンスに登録されたため、デバイスを起動し、プロビジョニング サービスを呼び出して、構成証明メカニズムを使用して認識されるようにすることができます。 デバイスは、プロビジョニング サービスに認識されると、IoT ハブに割り当てられます。

TPM および X.509 の両方の構成証明を使用して、シミュレートされたデバイスの例では、C、Java、C の\#Node.js、および Python。 たとえば、TPM と [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) を使用するシミュレートされたデバイスは、「[デバイスの初回ブート シーケンスのシミュレーション](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device#simulate-first-boot-sequence-for-the-device)」セクションで説明されているプロセスに従います。 X.509 証明書構成証明を使用する同じデバイスの場合は、この[ブート シーケンス](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509#simulate-first-boot-sequence-for-the-device)についてのセクションを参照してください。

実際のデバイスの例については、[MXChip Iot DevKit のハウツー ガイド](https://docs.microsoft.com/azure/iot-dps/how-to-connect-mxchip-iot-devkit)を参照してください。

デバイスを起動して、デバイスのクライアント アプリケーションが Device Provisioning Service への登録を開始できるようにします。

## <a name="verify-the-device-is-registered"></a>デバイスが登録されていることを確認する

デバイスが起動すると、次のアクションが実行されます。

1. デバイスが Device Provisioning Service に登録要求を送信します。

2. TPM デバイスの場合、Device Provisioning Service が登録チャレンジを送信し、デバイスがこれに応答します。

3. 登録が成功すると、Device Provisioning Service は、IoT ハブの URI、デバイス ID、暗号化されたキーをデバイスに送信します。

4. デバイス上の IoT Hub クライアント アプリケーションがハブに接続します。

5. ハブに正常に接続されると、IoT ハブの **[IoT デバイス]** エクスプローラーにデバイスが表示されます。

![ポータルに表示されたハブへの成功した接続](../media-draft/12ea6da6eef9bf96be6bd80aa1721173.png)

<!--Reference links

-   <https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-cloud>

-   <https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-provision-device-to-hub>-->