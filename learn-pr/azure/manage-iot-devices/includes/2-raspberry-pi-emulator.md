Raspberry Pi ボードは、最近、テスティング セオリーや、優れたものを作るイベントで、多くの関心を集めています。 このボードは非常に低コストですが、投資する前に、Raspberry Pi の機能をテストしてみたいと思われている方もいるでしょう。

Microsoft は、エミュレートされたハードウェアをユーザーがコードを使用して制御することができる、オンラインの [Raspberry Pi Azure IoT シミュレーター](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) を構築しました。 エミュレーターでは、回線を 1 つにつなぐことができるブレッドボードを介して、温度、湿度、圧力センサー、および赤色 LED に接続された Raspberry Pi のグラフィックが描かれます。 表示されるサイド パネルで、ユーザーは Node.js JavaScript コードを入力して、LED を制御し、シミュレートされたセンサーからダミー データを収集することができます。

![Raspberry Pi シミュレーター](../media/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Raspberry Pi Azure IoT オンライン シミュレーター

シミュレーターでは、最初の実行時に、コマンド ラインを介して表示されるサンプル温度キャプチャ プログラムが操作されます。 シミュレーターは実際のデバイスに転送する前にコードをテストできるように設計されているため、同じサンプル アプリケーションを実際の Pi でも実行することができます。

Web シミュレーターには、次の 3 つの領域があります。

1. **アセンブリ領域**。 ここでは、ご利用のデバイスの状態を確認できます。 既定では、これは BME280 センサーおよび LED ライトに接続されている Pi です。 この構成は、この時点ではカスタマイズできません。
2. **コーディング領域**。 Raspberry Pi 上で Node.js を使用してアプリを作成するためのオンライン コード エディターです。 既定のサンプル アプリケーションでは、BME280 センサーからセンサー データを容易に収集することができ、そのデータがご利用に Azure IoT Hub に送信されます。
3. **統合されたコンソール ウィンドウ**。 ここでは、ご利用のアプリケーションの出力を確認できます。 コンソール内には、3 つの関数があります。
    - `run` -サンプル コードが実行されます (サンプルの実行時は、コードは読み取り専用です)。
    - `Stop` - サンプル コードの実行が停止されます。
    - `Reset` - コードがリセットされます。

これで Raspberry Pi シミュレーターの概要の説明が終了しましたので、次に、Azure 内の IoT Hub を探索します。ここでは、シミュレーターからデータをキャプチャするための新しいリソースを作成します。

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->