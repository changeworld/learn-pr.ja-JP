[!include[](../../../includes/azure-sandbox-activate.md)]

実際には、Azure IoT Central を物理デバイス (対応のコーヒー メーカー) に接続します。 ここでは、Node.js アプリケーションを使ってデバイスをシミュレートして、Azure IoT Central アプリケーションに接続します。 シミュレートしたコーヒー メーカーからのテレメトリの測定は、監視と分析のために IoT Central に送信されます。

![コーヒー メーカーを示すイラスト。](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a>IoT Central でコーヒー メーカーを追加する 
コーヒー マシンをアプリケーションに追加するには、前のユニットで作成した **Connected Coffee Maker** デバイス テンプレートを使用します。

1. 新しいデバイスを追加するには、左側のナビゲーション メニューで **[Device Explorer]** を選択します。

    コーヒー メーカーの接続を開始するには、**[+ 新規]**、**[Real]\(リアル\)**、**[作成]** の順に選択します。 完了すると、同じ接続されているコーヒー メーカーのテンプレートを使用して作成したデバイスの一覧が表示されます。
   
    *   接続されているコーヒー メーカーは、**[+ 新規]**、**[Real]\(リアル\)** の順に選択すると一覧に追加されます。 
    *   接続されているコーヒー メーカー (シミュレート) は、テスト目的で IoT Central によって自動的に作成されます。 

1.  必要に応じて、名前に "Real" を追加することで、新しく追加されたコーヒー マシンを区別できます。 新しいデバイスの名前を変更するには、デバイスを選択し、[名前] フィールドの名前を編集します。 

    ![コーヒー マシン](../media/3-connect-device-a.png) 

    次のセクションでお使いのコーヒー マシンを接続するために、**[Connect this device]\(このデバイスを接続する\)** の場所を書き留めてください。 今はコーヒー マシンに接続していないため、画面には、"データが見つかりません" と表示されます。 接続が確立されると、画面に実際のテレメトリが作成されます。 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a>アプリケーションからコーヒー メーカーに対する接続文字列を生成する
ご使用の実際のコーヒー メーカーに対する接続文字列を、デバイス上で実行されるコードに埋め込みます。 接続文字列により、コーヒー マシンが Azure IoT Central アプリケーションに安全に接続できます。 どのデバイス インスタンスにも一意の接続文字列があります。 次の手順では、ご自分の Node.js アプリケーションを作成する一環として、接続文字列を生成します。

## <a name="create-a-nodejs-application"></a>Node.js アプリケーションを作成する
次の手順では、アプリケーションに追加したコーヒー メーカーを実装するクライアント アプリケーションを作成する方法を示します。

> [!TIP]
> この演習では、Azure Cloud Shell でアプリを作成するため、ご利用のローカル マシン上に何もインストールする必要はありません。 

1. Azure Cloud Shell で次のコマンドを実行し、`coffee-maker` フォルダーを作成してそこに移動します。

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. Cloud Shell で `coffee-maker` フォルダーから、次のコマンドを実行して DPS キー ジェネレーターをインストールします。 

   ```azurecli
    npm install dps-keygen
   ```
    このコマンドによって、dps-keygen パッケージがローカル フォルダーの `coffee-maker` にインストールされます。 グローバル パッケージとしてインストールするアクセス許可がないため、`-g` オプションは省略します。

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. Cloud Shell で次のコマンドを実行して、GitHub から DPS 接続文字列ユーティリティをダウンロードします。 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > Cloud Shell で実行しているため、Linux 版の **dps_cstr** がダウンロードされました。

1. 次のコマンドを実行して、`dps_cstr` に実行のアクセス許可を付与します。

    ```azurecli
    chmod +x dps_cstr
    ```

    ここでのデバイス用の接続文字列を生成するには、Azure IoT Central ポータルから次の 3 つの情報が必要です。
    - **スコープ ID**
    - **デバイス ID**
    - **主キー**

1. IoT Central ポータルに戻ります。 *Connected Coffee Maker - Real* デバイス用のデバイス画面で、画面の右上にある **[接続]** を選択します。

1. 表示された [デバイスの接続] ダイアログ上で、この演習の後で使用するため、**スコープ ID**、**デバイス ID**、**主キー**の値を任意の場所に保存します。 

1. **<scope_id>**、**<device_id>**、**<primary_key>** を最後の手順で保存した値に置き換えて、Cloud Shell で次のコマンドを実行します。 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    このコマンドにより、指定した値に基づいて接続文字列が生成され、**connection.txt** と名付けたファイルに書き込まれます。

    > [!IMPORTANT]
    > コマンド `dps_cstr` は、シェル内のご自分の PATH にあるものではありません。 そのため、必ず `./dps_cstr` を使って呼び出してください。

1. 次のコマンドを実行することによって、Cloud Shell で統合コード エディターを開きます。 

    ```azurecli
    code
    ```
1. エディターの **[FILES]** メニューで、ファイルの一覧から **connection.txt** を接続します。

1. **connection.txt** に ``HostName=`` から始まる接続文字列が含まれていることを確認します。

1. エディターの右上にあるメニュー (*...*) から **[エディターを閉じる]** を選択して、エディターを閉じます。 

1. Cloud Shell で次のコマンドを実行し、`coffee-maker` フォルダー内の Node.js プロジェクトを初期化します。

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > init スクリプトで、プロジェクトのプロパティを入力するように求められます。 この演習では、Enter キーを押してすべて既定値を使用します。

1. 必要なパッケージをインストールするには、`coffee-maker` フォルダーで次のコマンドを実行します。

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Cloud Shell で次のコマンドを実行して、新しいファイルを作成します。

    ```azurecli
    touch coffeeMaker.js
    ```
1. Cloud Shell のコマンド ラインで次を実行することによって、統合コード エディターを開きます。 

     ```azurecli
    code
    ```
    
1. コード エディターが表示されたら、**[FILES]** 一覧の更新ボタンを選択し、新しい **coffeeMaker.js** ファイルを選択します。 

1. 次のコードをコピーして、空の編集ウィンドウに貼り付けます。

    ```js
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;

    // Connection string (TODO: CHANGE HERE)
    const connectionString = `{your device connection string}`;

    // Global variables
    var client = clientFromConnectionString(connectionString);
    var optimalTemperature = 96;
    var cupState = true;
    var brewingState = false;
    var cupTimer = 20;
    var brewingTimer = 0;
    var maintenanceState = false;
    var warrantyState = Math.random() < 0.5?0:1;

    // Helper function to produce nice numbers (##.#)
    function niceNumber(value) 
    {
        var number = (Math.round(value * 10.0)).toString();
        return number.substr(0, 2) + '.' + number.substr(2, 1);
    }

    // Send device simulated telemetry measurements
    function sendTelemetry() 
    {   
        // Simulate the telemetry values
        var temperature = optimalTemperature + (Math.random() * 4) - 2;
        var humidity = 20 + (Math.random() * 80);
        
        // Cup timer - every 20 seconds randomly decide if the cup is present or not
        cupTimer--;
        
        if (cupTimer == 0)
        {
            cupTimer = 20;
            cupState = Math.random() < 0.5?false:true;
        }
        
        // Brewing timer
        if (brewingTimer > 0)
        {
            brewingTimer--;
            
            // Finished brewing
            if (brewingTimer == 0)
            {
                brewingState = false;
            }
        }
    
        // Create the data JSON package
        var data = JSON.stringify(
        { 
            waterTemperature: temperature, 
            airHumidity: humidity, 
        });

        // Create the message with the above defined data
        var message = new Message(data);
        
        // Set the state flags
        message.properties.add('stateCupDetected', cupState);
        message.properties.add('stateBrewing', brewingState);
        
        // Show the information in console
        var infoTemperature = niceNumber(temperature);
        var infoHumidity = niceNumber(humidity);
        var infoCup = cupState ? 'Y' :'N';
        var infoBrewing = brewingState ? 'Y':'N';
        var infoMaintenance = maintenanceState ? 'Y':'N';
        
        console.log('Telemetry send: Temperature: ' + infoTemperature + 
                    ' Humidity: ' + infoHumidity + '%' + 
                    ' Cup Detected: ' + infoCup + 
                    ' Brewing: ' + infoBrewing + 
                    ' Maintenance Mode: ' + infoMaintenance);
        
        // Send the message
        client.sendEvent(message, function (errorMessage) 
        {
            // Error
            if (errorMessage) 
            {
                console.log('Failed to send message to Azure IoT Hub: ${err.toString()}');
            }
        });
    }

    // Send device properties
    function sendDeviceProperties(deviceTwin) 
    {
        var properties = 
        {
            propertyWarrantyExpired: warrantyState
        };
        
        console.log(' * Property - Warranty State: ' + warrantyState.toString());
        
        deviceTwin.properties.reported.update(properties, (errorMessage) => 
            console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }

    // Optimal temperature setting
    var settings = 
    {
        'setTemperature': (newValue, callback) => 
        {
            setTimeout(() => 
            {
                optimalTemperature = newValue;
                callback(optimalTemperature, 'completed');
            }, 1000);
        }
    };

    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(deviceTwin) 
    {
        deviceTwin.on('properties.desired', function (desiredChange) 
        {
            // Iterate all settings looking for the defined one
            for (let setting in desiredChange) 
            {
                // Setting we defined
                if (settings[setting]) 
                {
                    // Console info
                    console.log(` * Received setting: ${setting}: ${desiredChange[setting].value}`);
                    
                    // Update 
                    settings[setting](desiredChange[setting].value, (newValue, status, message) => 
                    {
                        var patch = 
                        {
                            [setting]: 
                            {
                                value: newValue,
                                status: status,
                                desiredVersion: desiredChange.$version,
                                message: message
                            }
                        }
                        deviceTwin.properties.reported.update(patch, (err) => console.log(` * Sent setting update for ${setting} ` +
                        (err ? `error: ${err.toString()}` : `(success)`)));
                    });
                }
            }
        });
    }

    // Maintenance mode command
    function onCommandMaintenance(request, response) 
    {
        // Display console info
        console.log(' * Maintenance command received');

        // Console warning
        if (maintenanceState)
        {
            console.log(' - Warning: The device is already in the maintenance mode.');
        }
        
        // Set state
        maintenanceState = true;

        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    function onCommandStartBrewing(request, response) 
    {
        // Display console info
        console.log(' * Brewing command received');

        // Console warning
        if (brewingState == true)
        {
            console.log(' - Warning: The device is already brewing.');
        }
        
        if (cupState == false)
        {
            console.log(' - Warning: The cup has not been detected.');
        }
        
        if (maintenanceState == true)
        {
            console.log(' - Warning: The device is in maintenance state.');
        }
        
        // Set state - brew for 30 seconds
        if ((cupState == true) && (brewingState == false) && (maintenanceState == false))
        {
            brewingState = true;
            brewingTimer = 30;
        }
        
        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    // Handle device connection to Azure IoT Central
    var connectCallback = (errorMessage) => 
    {
        // Connection error
        if (errorMessage) 
        {
            console.log(`Device could not connect to Azure IoT Central: ${errorMessage.toString()}`);
        } 
        // Successfully connected
        else 
        {
            // Notify the user
            console.log('Device successfully connected to Azure IoT Central');

            // Send telemetry measurements to Azure IoT Central every 1 second.
            setInterval(sendTelemetry, 1000);
            
            // Set up device command callbacks
            client.onDeviceMethod('cmdSetMaintenance', onCommandMaintenance);
            client.onDeviceMethod('cmdStartBrewing', onCommandStartBrewing);
            
            // Get device twin from Azure IoT Central
            client.getTwin((errorMessage, deviceTwin) => 
            {
                // Failed to retrieve device twin
                if (errorMessage) 
                {
                    console.log(`Error getting device twin: ${errorMessage.toString()}`);
                } 
                // Success
                else 
                {
                    // Notify the user
                    console.log('Device Twin successfully retrieved from Azure IoT Central');
                
                    // Send device properties once on device startup
                    sendDeviceProperties(deviceTwin);
                    
                    // Apply device settings and handle changes to device settings
                    handleSettings(deviceTwin);
                }
            });
        }
    };

    // Start the device (connect it to Azure IoT Central)
    client.open(connectCallback);

    ```

    ここでのコーヒー メーカーは Node.js で記述されています。 まず、Azure IoT Central に接続されます。 次に、アプリによって初期プロパティが Azure IoT Central に送信され、設定の同期、Maintenance と Brewing の 2 つのコマンド ハンドラーの登録が行われ、最後に 1 秒ごとにテレメトリ情報を送信するためのタイマーが開始されます。

1.  このコードの上部にあるプレースホルダー `{your device connection string}` を、前に作成して **connection.txt** に保存した接続文字列に更新します。 接続文字列は `HostName=` から始まります。

1. エディターの右上で 3 つのドット `...` を選択し、エディター メニューを展開します。 次に、**[保存]** を選択し、"coffeeMaker.js" にした編集を保存します。

1. Cloud Shell ウィンドウで次のコマンドを実行し、アプリを開始します。

    ```azurecli
    node coffeeMaker.js
    ```
1. Cloud Shell ウィンドウの "*デバイスが Azure IoT Central に正常に接続されました*" というメッセージと "*テレメトリの送信:*" のメッセージで、アプリが開始されたことを確認します。 お疲れさまでした。 ご利用のアプリが起動して実行され、IoT Central と通信されています。