Raspberry Pi ボードには、理論またはクール物事イベントをテストするための遅延が大きな関心を集めています。 このボード上のコストは非常に低いですが、1 つに投資する前に、Raspberry Pi の機能をテストでいくつか知りたい場合があります。

Microsoft がオンラインで構築された[Raspberry Pi Azure IoT シミュレーター](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true)コードを使用して、エミュレートされたハードウェアを制御できるようにします。 エミュレーターでは、温度、湿度、圧力センサー、および回路を許可する実験用回路板を使用してまとめるを赤色の LED に接続されている Raspberry Pi のグラフィックを報告します。 表示されている側のパネルでは、LED を制御し、シミュレートされたセンサーをダミー データを収集する Node.js の JavaScript コードを入力することができます。

![Raspberry Pi シミュレーター](../media-draft/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Raspberry Pi Azure IoT のオンライン シミュレーター

初回実行時は、シミュレーターは、コマンドラインを使用して表示されるサンプル温度キャプチャ プログラムを動作します。 シミュレーターが実際のデバイスに転送する前にコードをテストできるようにするように設計として、同じサンプル アプリケーションを実際の Pi の実行もできます。

Web シミュレーターでは、3 つの領域があります。

1. **アセンブリ領域**します。 これは、デバイスの状態を確認できます。 既定では、これは、Pi が BME280 センサーと LED ライトとの接続です。 この構成は、この時点で、カスタマイズ可能なはありません。
2. **コーディング領域**します。 Node.js を使用した Raspberry Pi でアプリを作成するためのオンライン コード エディター。 既定のサンプル アプリケーションは、BME280 センサーからセンサー データを収集するのに役立つし、Azure IoT Hub に送信します。
3. **統合されたコンソール ウィンドウ**します。 これは、アプリケーションの出力を確認できます。 コンソール内では、3 つの関数があります。
    - `run` -サンプル コードを実行する (サンプルの実行中は、コードは読み取り専用)。
    - `Stop` -実行されているサンプル コードを停止します。
    - `Reset` -コードをリセットします。

Raspberry Pi シミュレーターの概要を把握するようになりましたことについて説明します、IoT Hub を Azure にシミュレーターからのデータをキャプチャする新しいリソースを作成します。

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->