[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="6f2e3-101">実際には、Azure IoT Central を物理デバイス (対応のコーヒー メーカー) に接続します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-101">In practice, you will connect Azure IoT Central to a physical device, i.e. an IoT enabled coffee machine.</span></span> <span data-ttu-id="6f2e3-102">ここでは、Node.js アプリケーションを使ってデバイスをシミュレートして、Azure IoT Central アプリケーションに接続します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-102">Here, you'll simualate a device with a Node.js applicatio and connect it to the Azure IoT Central application.</span></span> <span data-ttu-id="6f2e3-103">シミュレートしたコーヒー メーカーからのテレメトリの測定は、監視と分析のために IoT Central に送信されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-103">Telemetry measurements from the simulated coffee machine are sent to IoT Central for monitoring and analysis.</span></span>

![コーヒー メーカーを示すイラスト。](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="6f2e3-105">IoT Central でコーヒー メーカーを追加する</span><span class="sxs-lookup"><span data-stu-id="6f2e3-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="6f2e3-106">コーヒー マシンをアプリケーションに追加するには、前のユニットで作成した **Connected Coffee Maker** デバイス テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="6f2e3-107">新しいデバイスを追加するには、左側のナビゲーション メニューで **[Device Explorer]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="6f2e3-108">コーヒー メーカーの接続を開始するには、**[+ 新規]**、**[Real]\(リアル\)**、**[作成]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-108">To start connecting your coffee machine, choose **+ New**, **Real**, and then **Create**.</span></span> <span data-ttu-id="6f2e3-109">完了すると、同じ接続されているコーヒー メーカーのテンプレートを使用して作成したデバイスの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="6f2e3-110">接続されているコーヒー メーカーは、**[+ 新規]**、**[Real]\(リアル\)** の順に選択すると一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-110">The Connected Coffee Maker is added to the list when you choose **+ New** and then **Real**.</span></span> 
    *   <span data-ttu-id="6f2e3-111">接続されているコーヒー メーカー (シミュレート) は、テスト目的で IoT Central によって自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="6f2e3-112">必要に応じて、名前に "Real" を追加することで、新しく追加されたコーヒー マシンを区別できます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="6f2e3-113">新しいデバイスの名前を変更するには、デバイスを選択し、[名前] フィールドの名前を編集します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![コーヒー マシン](../media/3-connect-device-a.png) 

    <span data-ttu-id="6f2e3-115">次のセクションでお使いのコーヒー マシンを接続するために、**[Connect this device]\(このデバイスを接続する\)** の場所を書き留めてください。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="6f2e3-116">今はコーヒー マシンに接続していないため、画面には、"データが見つかりません" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-116">For now, the screen displays "Missing Data" because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="6f2e3-117">接続が確立されると、画面に実際のテレメトリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="6f2e3-118">アプリケーションからコーヒー メーカーに対する接続文字列を生成する</span><span class="sxs-lookup"><span data-stu-id="6f2e3-118">Generate connection string for the coffee machine from your application</span></span>
<span data-ttu-id="6f2e3-119">ご使用の実際のコーヒー メーカーに対する接続文字列を、デバイス上で実行されるコードに埋め込みます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="6f2e3-120">接続文字列により、コーヒー マシンが Azure IoT Central アプリケーションに安全に接続できます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="6f2e3-121">どのデバイス インスタンスにも一意の接続文字列があります。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="6f2e3-122">次の手順では、ご自分の Node.js アプリケーションを作成する一環として、接続文字列を生成します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-122">In the next steps, you generate the connection string as part of creating your node.js application.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="6f2e3-123">Node.js アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="6f2e3-123">Create a Node.js application</span></span>
<span data-ttu-id="6f2e3-124">次の手順では、アプリケーションに追加したコーヒー メーカーを実装するクライアント アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-124">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>

> [!TIP]
> <span data-ttu-id="6f2e3-125">この演習では、Azure Cloud Shell でアプリを作成するため、ご利用のローカル マシン上に何もインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-125">In this exercise, we'll create the app in the Azure Cloud Shell so that you don't have to install anything on your local machine.</span></span> 

1. <span data-ttu-id="6f2e3-126">Azure Cloud Shell で次のコマンドを実行し、`coffee-maker` フォルダーを作成してそこに移動します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-126">Execute the following command in the Azure Cloud Shell to create a `coffee-maker` folder and navigate to it:</span></span>

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. <span data-ttu-id="6f2e3-127">Cloud Shell で `coffee-maker` フォルダーから、次のコマンドを実行して DPS キー ジェネレーターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-127">From the `coffee-maker` folder in Cloud Shell, execute the following command to install the DPS key generator:</span></span> 

   ```azurecli
    npm install dps-keygen
   ```
    <span data-ttu-id="6f2e3-128">このコマンドによって、dps-keygen パッケージがローカル フォルダーの `coffee-maker` にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-128">This command installs the dps-keygen package to our local folder, `coffee-maker`.</span></span> <span data-ttu-id="6f2e3-129">グローバル パッケージとしてインストールするアクセス許可がないため、`-g` オプションは省略します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-129">We leave out the `-g` option because we don't have permissions to install as a global package.</span></span>

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. <span data-ttu-id="6f2e3-130">Cloud Shell で次のコマンドを実行して、GitHub から DPS 接続文字列ユーティリティをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-130">Execute the following command in the Cloud Shell to download the DPS connection string utility from GitHub:</span></span> 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > <span data-ttu-id="6f2e3-131">Cloud Shell で実行しているため、Linux 版の **dps_cstr** がダウンロードされました。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-131">We downloaded the Linux version of **dps_cstr** because we're running in the Cloud Shell.</span></span>

1. <span data-ttu-id="6f2e3-132">次のコマンドを実行して、`dps_cstr` に実行のアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-132">Execute the following command to give `dps_cstr` execute permissions:</span></span>

    ```azurecli
    chmod +x dps_cstr
    ```

    <span data-ttu-id="6f2e3-133">ここでのデバイス用の接続文字列を生成するには、Azure IoT Central ポータルから次の 3 つの情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-133">To generate a connection string for our device, we need three pieces of information from the Azure IoT Central portal:</span></span>
    - <span data-ttu-id="6f2e3-134">**スコープ ID**</span><span class="sxs-lookup"><span data-stu-id="6f2e3-134">**Scope ID**</span></span>
    - <span data-ttu-id="6f2e3-135">**デバイス ID**</span><span class="sxs-lookup"><span data-stu-id="6f2e3-135">**Device ID**</span></span>
    - <span data-ttu-id="6f2e3-136">**主キー**</span><span class="sxs-lookup"><span data-stu-id="6f2e3-136">**Primary Key**</span></span>

1. <span data-ttu-id="6f2e3-137">IoT Central ポータルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-137">Return to the IoT Central portal.</span></span> <span data-ttu-id="6f2e3-138">*Connected Coffee Maker - Real* デバイス用のデバイス画面で、画面の右上にある **[接続]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-138">On the device screen for the *Connected Coffee Maker - Real* device, select **Connect** in the top right of the screen.</span></span>

1. <span data-ttu-id="6f2e3-139">表示された [デバイスの接続] ダイアログ上で、この演習の後で使用するため、**スコープ ID**、**デバイス ID**、**主キー**の値を任意の場所に保存します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-139">On the Device Connection dialog that opens, save the values **Scope ID**, **Device ID** and **Primary Key** somewhere, because we'll use them later in this exercise.</span></span> 

1. <span data-ttu-id="6f2e3-140">**<scope_id>**、**<device_id>**、**<primary_key>** を最後の手順で保存した値に置き換えて、Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-140">Execute the following command in the Cloud Shell, replacing **<scope_id>**, **<device_id>**, and **<primary_key>** with the values you saved in the last step.</span></span> 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    <span data-ttu-id="6f2e3-141">このコマンドにより、指定した値に基づいて接続文字列が生成され、**connection.txt** と名付けたファイルに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-141">This command generates a connection string based on the values you gave it and writes them to a file that we've named **connection.txt**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6f2e3-142">コマンド `dps_cstr` は、シェル内のご自分の PATH にあるものではありません。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-142">The command `dps_cstr` is not in your PATH in the shell.</span></span> <span data-ttu-id="6f2e3-143">そのため、必ず `./dps_cstr` を使って呼び出してください。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-143">So, make sure to call it with `./dps_cstr`</span></span>

1. <span data-ttu-id="6f2e3-144">次のコマンドを実行することによって、Cloud Shell で統合コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-144">Open the integrated code editor in the Cloud Shell by running the following command:</span></span> 

    ```azurecli
    code
    ```
1. <span data-ttu-id="6f2e3-145">エディターの **[FILES]** メニューで、ファイルの一覧から **connection.txt** を接続します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-145">Select **connection.txt** from the list of files in the **FILES** menu of the editor.</span></span>

1. <span data-ttu-id="6f2e3-146">**connection.txt** に ``HostName=`` から始まる接続文字列が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-146">Verify that **connection.txt** contains a connection string that starts with ``HostName=``.</span></span>

1. <span data-ttu-id="6f2e3-147">エディターの右上にあるメニュー (*...*) から **[エディターを閉じる]** を選択して、エディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-147">Close the editor by selecting **Close Editor** from the menu (*...*) at the top right of the editor.</span></span> 

1. <span data-ttu-id="6f2e3-148">Cloud Shell で次のコマンドを実行し、`coffee-maker` フォルダー内の Node.js プロジェクトを初期化します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-148">Execute the following command in the Cloud Shell to initialize a Node.js project in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="6f2e3-149">init スクリプトで、プロジェクトのプロパティを入力するように求められます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-149">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="6f2e3-150">この演習では、Enter キーを押してすべて既定値を使用します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-150">For this exercise, press ENTER to use all default values.</span></span>

1. <span data-ttu-id="6f2e3-151">必要なパッケージをインストールするには、`coffee-maker` フォルダーで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-151">To install the necessary packages, run the following command in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="6f2e3-152">Cloud Shell で次のコマンドを実行して、新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-152">Execute the following command to create a new file in the Cloud Shell:</span></span>

    ```azurecli
    touch coffeeMaker.js
    ```
1. <span data-ttu-id="6f2e3-153">Cloud Shell のコマンド ラインで次を実行することによって、統合コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-153">Open the integrated code editor by executing the following at the command line in the Cloud Shell:</span></span> 

     ```azurecli
    code
    ```
    
1. <span data-ttu-id="6f2e3-154">コード エディターが表示されたら、**[FILES]** 一覧の更新ボタンを選択し、新しい **coffeeMaker.js** ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-154">When the code editor opens, select the refresh button on the **FILES** list and select our new file **coffeeMaker.js**.</span></span> 

1. <span data-ttu-id="6f2e3-155">次のコードをコピーして、空の編集ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-155">Copy and paste the following code into the empty editor window:</span></span>

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

    <span data-ttu-id="6f2e3-156">ここでのコーヒー メーカーは Node.js で記述されています。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-156">Our coffee machine is written in Node.js.</span></span> <span data-ttu-id="6f2e3-157">まず、Azure IoT Central に接続されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-157">It first connects to Azure IoT Central.</span></span> <span data-ttu-id="6f2e3-158">次に、アプリによって初期プロパティが Azure IoT Central に送信され、設定の同期、Maintenance と Brewing の 2 つのコマンド ハンドラーの登録が行われ、最後に 1 秒ごとにテレメトリ情報を送信するためのタイマーが開始されます。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-158">Then the app sends initial properties to Azure IoT Central, synchronizes settings, registers two command handlers for maintenance and brewing, and finally starts the timer for sending the telemetry information every second.</span></span>

1.  <span data-ttu-id="6f2e3-159">このコードの上部にあるプレースホルダー `{your device connection string}` を、前に作成して **connection.txt** に保存した接続文字列に更新します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-159">Update the placeholder `{your device connection string}` at the top of this code with the connection string you created earlier and saved in **connection.txt**.</span></span> <span data-ttu-id="6f2e3-160">接続文字列は `HostName=` から始まります。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-160">The connection string begins with `HostName=`.</span></span>

1. <span data-ttu-id="6f2e3-161">エディターの右上で 3 つのドット `...` を選択し、エディター メニューを展開します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-161">Select the three dots `...` to the top right of the editor to expand the editor menu.</span></span> <span data-ttu-id="6f2e3-162">次に、**[保存]** を選択し、"coffeeMaker.js" にした編集を保存します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-162">Then select **Save** to save the edits we made to \`coffeeMaker.js'</span></span>

1. <span data-ttu-id="6f2e3-163">Cloud Shell ウィンドウで次のコマンドを実行し、アプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-163">Execute the following command in the Cloud Shell window to start the app:</span></span>

    ```azurecli
    node coffeeMaker.js
    ```
1. <span data-ttu-id="6f2e3-164">Cloud Shell ウィンドウの "*デバイスが Azure IoT Central に正常に接続されました*" というメッセージと "*テレメトリの送信:*" のメッセージで、アプリが開始されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-164">Verify that the app starts in the Cloud Shell window with the message *Device successfully connected to Azure IoT Central* along with *Telemetry send:* messages.</span></span> <span data-ttu-id="6f2e3-165">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-165">Congratulations!</span></span> <span data-ttu-id="6f2e3-166">ご利用のアプリが起動して実行され、IoT Central と通信されています。</span><span class="sxs-lookup"><span data-stu-id="6f2e3-166">Your app is up and running and communicating with IoT Central!</span></span>