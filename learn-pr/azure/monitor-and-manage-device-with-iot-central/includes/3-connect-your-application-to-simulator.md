In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine. But for the purpose of this unit, you connect a Node.js application, representing a physical/real coffee machine, to the Azure IoT Central application. As a result of this connection, telemetry measurements from the coffee machine is sent to IoT Central for monitoring and analysis.

![Coffee machine](../images/3-coffee-machine.png) 

## Add the coffee machine in IoT Central 
To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.

1. To add a new device, choose **Device Explorer** in the left navigation menu.

	To start connecting your coffee machine, choose **+ New**, then **Real**. When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.
   
    *	The Connected Coffee Maker is added to the list when you choose + New, then Real. 
    *	The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes. 

1.	Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name. To rename your new device, choose the device and edit the name in the name field. 

    ![Coffee machine](../images/3-connect-device-a.png) 

    Note the location of **Connect this device** for connecting your coffee machine in the next section. For now the screen displays "Missing Data", this is because you haven't connected to the coffee machine. The real telemetry begins to populate the screen once the connection is made. 
 
## Get connection string for the coffee machine from your application
You embed the connection string for your real coffee machine in the code that runs on the device. The connection string enables the coffee machine to connect securely to your Azure IoT Central application. Every device instance has a unique connection string. Here is how to find the connection string for a device instance in your application:

1.	On the device screen for your Connected Coffee Machine Real, choose **Connect this device**.

1.	On the **Connect** page, copy the **Primary connection string**, and save it. You use this value in the client application that runs on the device.

## Create a Node.js application
The following steps show you how to create a client application that implements the coffee machine you added to the application.
1. Install [Node.js](https://nodejs.org/) version 4.0.x or later on your machine. Node.js is available for a wide variety of operating systems.

1. Create a folder called coffee-maker on your machine. Navigate to that folder in your command-line environment.

1. To initialize your Node.js project, run the following commands:
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > The init script prompts you to enter project properties. For this exercise, using the default values is sufficient and recommended. 
1. To install the necessary packages, run the following command:
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.

1. Copy and paste the code into your coffeeMaker.js file and **Save**.

    The code, representing the real coffee machine in this unit, is written in Node.js. You begin by establishing connection with the application, then you send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.

    > [!NOTE]
    > Update the placeholder `{your device connection string}`. 


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

1.  Update the placeholder {your device connection string} with your device connection string. You copied this value from the connection details page when you added your real device. 

## Run your Node.js application
```cmd/sh
node coffeeMaker.js
```

## Summary
In this unit, you connected your coffee machine to Azure IoT Central and began sending data for monitoring and analysis. You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.
