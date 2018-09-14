現実の環境では、Azure IoT Central を実際のデバイス (この例でいえば実際のコーヒー マシン) に接続するものと考えられます。 このユニットでは、するために接続する Node.js アプリケーションでは、Azure IoT Central アプリケーションに、物理または real コーヒー マシンを表します。 この接続は、結果として、コーヒー マシンからのテレメトリの測定値は、監視と分析の IoT Central に送信されます。

![コーヒー マシン](../images/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a>IoT Central のコーヒー マシンを追加します。 
使用する、コーヒー マシンをアプリケーションを追加する、**コーヒー メーカーの接続されている**前の単位で作成したデバイス テンプレート。

1. 新しいデバイスを追加する**Device Explorer**左側のナビゲーション メニューでします。

    コーヒー マシンの接続を開始する **+ 新規**、し**実際**します。 完了したら、同じコーヒー メーカーの接続されているテンプレートを使用して作成したデバイスの一覧を参照してください。
   
    *   接続のコーヒー メーカーは、リストを選択すると + 新規、その後、実際に追加されます。 
    *   接続されているコーヒー メーカー (シミュレート) は、テスト目的で IoT Central によって自動的に作成されます。 

1.  必要に応じて、その名前で word"の Real"を追加して、新しく追加されたコーヒー マシンを区別できます。 新しいデバイスの名前を変更するには、デバイスを選択し、[名前] フィールドに名前を編集します。 

    ![コーヒー マシン](../images/3-connect-device-a.png) 

    場所をメモ**このデバイスを接続**次のセクションで、コーヒー マシンを接続するためです。 これで、画面には、「見つからないデータの」が表示されます、コーヒー マシンに接続していないためにです。 実際のテレメトリは、接続が確立されると、画面の設定を開始します。 
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a>アプリケーションからコーヒー マシンに対する接続文字列を取得する
実際のコーヒー マシンに対する接続文字列を、デバイス上で実行されるコードに埋め込みます。 接続文字列により、コーヒー マシンが Azure IoT Central アプリケーションに安全に接続できます。 どのデバイス インスタンスにも一意の接続文字列があります。 ここでは、アプリケーションのデバイス インスタンスの接続文字列を見つける方法を示します。

1.  接続されているコーヒー マシン実際のデバイスの画面で選択**このデバイスを接続**します。

1.  **[接続]** ページで、**[プライマリ接続文字列]** をコピーして保存します。 デバイス上で実行されるクライアント アプリケーションで、この値を使用します。

## <a name="create-a-nodejs-application"></a>Node.js アプリケーションを作成する
次の手順では、アプリケーションに追加したコーヒー マシンを実装するクライアント アプリケーションを作成する方法を示します。
1. インストール[Node.js](https://nodejs.org/)バージョン 4.0.x または後で、コンピューター。 Node.js は、さまざまなオペレーティング システムで使用できます。

1. マシン上に coffee-maker という名前のフォルダーを作成します。 コマンドライン環境でそのフォルダーに移動します。

1. Node.js プロジェクトを初期化するには、次のコマンドを実行します。
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > Init スクリプトでは、プロジェクトのプロパティを入力するように求められます。 この演習では、既定値を使用して十分なとお勧めします。 
1. 必要なパッケージをインストールするには、次のコマンドを実行します。
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. テキスト エディターを使用して、coffee-maker フォルダーに coffeeMaker.js という名前のファイルを作成します。

1. コードをコピーして coffeeMaker.js ファイルに貼り付け、**[保存]** を選択します。

    このユニットで、実際のコーヒー マシンを表す、コードは、Node.js で記述されます。 アプリケーションで、接続を確立することから始める、Azure IoT Central の初期プロパティへの送信、設定を同期、メンテナンスと Brewing、2 つのコマンド ハンドラーを登録および最後に、テレメトリを送信するためのタイマーを開始します。1 秒ごとの情報。

    > [!NOTE]
    > プレース ホルダーを更新`{your device connection string}`します。 


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

1.  プレースホルダー {your device connection string} を、デバイスの接続文字列に置き換えます。 この値は、実際のデバイスを追加したときに接続の詳細ページからコピーしたものです。 

## <a name="run-your-nodejs-application"></a>Node.js アプリケーションを実行する
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a>まとめ
このユニットでは、Azure IoT Central にコーヒー マシンを接続し、監視、分析するデータの送信を開始します。 最初に IoT Central から接続文字列を取得した後、コーヒー マシンで文字列を構成することによって、接続を実現しました。
