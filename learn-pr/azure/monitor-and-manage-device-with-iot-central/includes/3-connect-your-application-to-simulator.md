現実の環境では、Azure IoT Central を実際のデバイス (この例でいえば実際のコーヒー マシン) に接続するものと考えられます。 しかし、このユニットでは、物理的な/実際のコーヒー マシンを表す Node.js アプリケーションと Azure IoT Central アプリケーションを接続します。 この接続の結果として、コーヒー マシンからのテレメトリの測定は、監視と分析のために IoT Central に送信されます。

![コーヒー マシン](../images/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a>IoT Central でコーヒー マシンを追加する 
コーヒー マシンをアプリケーションに追加するには、前のユニットで作成した **Connected Coffee Maker** デバイス テンプレートを使用します。

1. 新しいデバイスを追加するには、左側のナビゲーション メニューで **[Device Explorer]** を選択します。

    コーヒー マシンの接続を開始するには、**[+ 新規]**、**[Real]\(リアル\)** の順に選択します。 完了すると、同じ接続されているコーヒー メーカーのテンプレートを使用して作成したデバイスの一覧が表示されます。
   
    *   接続されているコーヒー メーカーは、[+ 新規]、[Real]\(リアル\) の順に選択すると一覧に追加されます。 
    *   接続されているコーヒー メーカー (シミュレート) は、テスト目的で IoT Central によって自動的に作成されます。 

1.  必要に応じて、名前に "Real" を追加することで、新しく追加されたコーヒー マシンを区別できます。 新しいデバイスの名前を変更するには、デバイスを選択し、[名前] フィールドの名前を編集します。 

    ![コーヒー マシン](../images/3-connect-device-a.png) 

    次のセクションでお使いのコーヒー マシンを接続するために、**[Connect this device]\(このデバイスを接続する\)** の場所を書き留めてください。 以降、画面には、"データが見つかりません" と表示されます。これは、コーヒー マシンに接続していないためです。 接続が確立されると、画面に実際のテレメトリが設定されます。 
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a>アプリケーションからコーヒー マシンに対する接続文字列を取得する
実際のコーヒー マシンに対する接続文字列を、デバイス上で実行されるコードに埋め込みます。 接続文字列により、コーヒー マシンが Azure IoT Central アプリケーションに安全に接続できます。 どのデバイス インスタンスにも一意の接続文字列があります。 ここでは、アプリケーションのデバイス インスタンスの接続文字列を見つける方法を示します。

1.  実際に接続されたコーヒー マシンのデバイス画面で、**[Connect this device]\(このデバイスを接続する\)** を選択します。

1.  **[接続]** ページで、**[プライマリ接続文字列]** をコピーして保存します。 デバイス上で実行されるクライアント アプリケーションで、この値を使用します。

## <a name="create-a-nodejs-application"></a>Node.js アプリケーションを作成する
次の手順では、アプリケーションに追加したコーヒー マシンを実装するクライアント アプリケーションを作成する方法を示します。
1. マシンに [Node.js](https://nodejs.org/) バージョン 4.0.x 以降をインストールします。 Node.js は、さまざまなオペレーティング システムで使用できます。

1. マシン上に coffee-maker という名前のフォルダーを作成します。 コマンドライン環境でそのフォルダーに移動します。

1. Node.js プロジェクトを初期化するには、次のコマンドを実行します。
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > init スクリプトで、プロジェクトのプロパティを入力するように求められます。 この演習では、既定値の使用で十分であり、これをお勧めします。 
1. 必要なパッケージをインストールするには、次のコマンドを実行します。
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. テキスト エディターを使用して、coffee-maker フォルダーに coffeeMaker.js という名前のファイルを作成します。

1. コードをコピーして coffeeMaker.js ファイルに貼り付け、**[保存]** を選択します。

    このユニットで実際のコーヒー マシンを表すコードが、Node.js に書き込まれます。 最初にアプリケーションとの接続を確立し、初期プロパティを Azure IoT Central に送信し、設定を同期し、Maintenance と Brewing の 2 つのコマンド ハンドラーを登録し、最後に 1 秒ごとにテレメトリ情報を送信するためのタイマーを開始します。

    > [!NOTE]
    > プレース ホルダー `{your device connection string}` を更新します。 


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
        
        // Cup timer - every 20 second randomly decide if the cup is present or not
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
            
            // Setup device command callbacks
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
                
                    // Send device properties once on device start up
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

1.  プレースホルダー {your device connection string} を、デバイスの接続文字列に更新します。 この値は、実際のデバイスを追加したときに接続の詳細ページからコピーしたものです。 

## <a name="run-your-nodejs-application"></a>Node.js アプリケーションを実行する
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a>まとめ
このユニットでは、接続されたコーヒー マシンを Azure IoT Central に接続し、監視と分析のためのデータの送信を開始しました。 最初に IoT Central から接続文字列を取得した後、コーヒー マシンで文字列を構成することによって、接続を実現しました。
