<span data-ttu-id="4ccc0-101">現実の環境では、Azure IoT Central を実際のデバイス (この例でいえば実際のコーヒー マシン) に接続するものと考えられます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-101">In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine.</span></span> <span data-ttu-id="4ccc0-102">しかし、このユニットでは、物理的な/実際のコーヒー マシンを表す Node.js アプリケーションと Azure IoT Central アプリケーションを接続します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-102">But for the purpose of this unit, you connect a Node.js application, representing a physical/real coffee machine, to the Azure IoT Central application.</span></span> <span data-ttu-id="4ccc0-103">この接続の結果として、コーヒー マシンからのテレメトリの測定は、監視と分析のために IoT Central に送信されます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-103">As a result of this connection, telemetry measurements from the coffee machine is sent to IoT Central for monitoring and analysis.</span></span>

![コーヒー マシン](../images/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="4ccc0-105">IoT Central でコーヒー マシンを追加する</span><span class="sxs-lookup"><span data-stu-id="4ccc0-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="4ccc0-106">コーヒー マシンをアプリケーションに追加するには、前のユニットで作成した **Connected Coffee Maker** デバイス テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="4ccc0-107">新しいデバイスを追加するには、左側のナビゲーション メニューで **[Device Explorer]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="4ccc0-108">コーヒー マシンの接続を開始するには、**[+ 新規]**、**[Real]\(リアル\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-108">To start connecting your coffee machine, choose **+ New**, then **Real**.</span></span> <span data-ttu-id="4ccc0-109">完了すると、同じ接続されているコーヒー メーカーのテンプレートを使用して作成したデバイスの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="4ccc0-110">接続されているコーヒー メーカーは、[+ 新規]、[Real]\(リアル\) の順に選択すると一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-110">The Connected Coffee Maker is added to the list when you choose + New, then Real.</span></span> 
    *   <span data-ttu-id="4ccc0-111">接続されているコーヒー メーカー (シミュレート) は、テスト目的で IoT Central によって自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="4ccc0-112">必要に応じて、名前に "Real" を追加することで、新しく追加されたコーヒー マシンを区別できます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="4ccc0-113">新しいデバイスの名前を変更するには、デバイスを選択し、[名前] フィールドの名前を編集します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![コーヒー マシン](../images/3-connect-device-a.png) 

    <span data-ttu-id="4ccc0-115">次のセクションでお使いのコーヒー マシンを接続するために、**[Connect this device]\(このデバイスを接続する\)** の場所を書き留めてください。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="4ccc0-116">以降、画面には、"データが見つかりません" と表示されます。これは、コーヒー マシンに接続していないためです。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-116">For now the screen displays "Missing Data", this is because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="4ccc0-117">接続が確立されると、画面に実際のテレメトリが設定されます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="4ccc0-118">アプリケーションからコーヒー マシンに対する接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="4ccc0-118">Get connection string for the coffee machine from your application</span></span>
<span data-ttu-id="4ccc0-119">実際のコーヒー マシンに対する接続文字列を、デバイス上で実行されるコードに埋め込みます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="4ccc0-120">接続文字列により、コーヒー マシンが Azure IoT Central アプリケーションに安全に接続できます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="4ccc0-121">どのデバイス インスタンスにも一意の接続文字列があります。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="4ccc0-122">ここでは、アプリケーションのデバイス インスタンスの接続文字列を見つける方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-122">Here is how to find the connection string for a device instance in your application:</span></span>

1.  <span data-ttu-id="4ccc0-123">実際に接続されたコーヒー マシンのデバイス画面で、**[Connect this device]\(このデバイスを接続する\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-123">On the device screen for your Connected Coffee Machine Real, choose **Connect this device**.</span></span>

1.  <span data-ttu-id="4ccc0-124">**[接続]** ページで、**[プライマリ接続文字列]** をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-124">On the **Connect** page, copy the **Primary connection string**, and save it.</span></span> <span data-ttu-id="4ccc0-125">デバイス上で実行されるクライアント アプリケーションで、この値を使用します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-125">You use this value in the client application that runs on the device.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="4ccc0-126">Node.js アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="4ccc0-126">Create a Node.js application</span></span>
<span data-ttu-id="4ccc0-127">次の手順では、アプリケーションに追加したコーヒー マシンを実装するクライアント アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-127">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>
1. <span data-ttu-id="4ccc0-128">マシンに [Node.js](https://nodejs.org/) バージョン 4.0.x 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-128">Install [Node.js](https://nodejs.org/) version 4.0.x or later on your machine.</span></span> <span data-ttu-id="4ccc0-129">Node.js は、さまざまなオペレーティング システムで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-129">Node.js is available for a wide variety of operating systems.</span></span>

1. <span data-ttu-id="4ccc0-130">マシン上に coffee-maker という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-130">Create a folder called coffee-maker on your machine.</span></span> <span data-ttu-id="4ccc0-131">コマンドライン環境でそのフォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-131">Navigate to that folder in your command-line environment.</span></span>

1. <span data-ttu-id="4ccc0-132">Node.js プロジェクトを初期化するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-132">To initialize your Node.js project, run the following commands:</span></span>
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="4ccc0-133">init スクリプトで、プロジェクトのプロパティを入力するように求められます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-133">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="4ccc0-134">この演習では、既定値の使用で十分であり、これをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-134">For this exercise, using the default values is sufficient and recommended.</span></span> 
1. <span data-ttu-id="4ccc0-135">必要なパッケージをインストールするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-135">To install the necessary packages, run the following command:</span></span>
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="4ccc0-136">テキスト エディターを使用して、coffee-maker フォルダーに coffeeMaker.js という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-136">Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.</span></span>

1. <span data-ttu-id="4ccc0-137">コードをコピーして coffeeMaker.js ファイルに貼り付け、**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-137">Copy and paste the code into your coffeeMaker.js file and **Save**.</span></span>

    <span data-ttu-id="4ccc0-138">このユニットで実際のコーヒー マシンを表すコードが、Node.js に書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-138">The code, representing the real coffee machine in this unit, is written in Node.js.</span></span> <span data-ttu-id="4ccc0-139">最初にアプリケーションとの接続を確立し、初期プロパティを Azure IoT Central に送信し、設定を同期し、Maintenance と Brewing の 2 つのコマンド ハンドラーを登録し、最後に 1 秒ごとにテレメトリ情報を送信するためのタイマーを開始します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-139">You begin by establishing connection with the application, then you send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ccc0-140">プレース ホルダー `{your device connection string}` を更新します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-140">Update the placeholder `{your device connection string}`.</span></span> 


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

1.  <span data-ttu-id="4ccc0-141">プレースホルダー {your device connection string} を、デバイスの接続文字列に更新します。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-141">Update the placeholder {your device connection string} with your device connection string.</span></span> <span data-ttu-id="4ccc0-142">この値は、実際のデバイスを追加したときに接続の詳細ページからコピーしたものです。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-142">You copied this value from the connection details page when you added your real device.</span></span> 

## <a name="run-your-nodejs-application"></a><span data-ttu-id="4ccc0-143">Node.js アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="4ccc0-143">Run your Node.js application</span></span>
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a><span data-ttu-id="4ccc0-144">まとめ</span><span class="sxs-lookup"><span data-stu-id="4ccc0-144">Summary</span></span>
<span data-ttu-id="4ccc0-145">このユニットでは、接続されたコーヒー マシンを Azure IoT Central に接続し、監視と分析のためのデータの送信を開始しました。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-145">In this unit, you connected your coffee machine to Azure IoT Central and began sending data for monitoring and analysis.</span></span> <span data-ttu-id="4ccc0-146">最初に IoT Central から接続文字列を取得した後、コーヒー マシンで文字列を構成することによって、接続を実現しました。</span><span class="sxs-lookup"><span data-stu-id="4ccc0-146">You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.</span></span>
