Raspberry Pi boards have garnered much interest of late for testing theories or
event making cool things. While the cost on this boards are quite low, some may
be interested in testing out the Raspberry Pi functionality before investing in
one.

Microsoft has built an online Raspberry Pi simulator allowing users to control
the emulated hardware via code. The emulator portrays a graphic of a Raspberry
Pi connected to a temperature, humidity, pressure sensor, and a red LED via
breadboard allowing circuits to be wired together. The displayed side panel
allows users to enter Node.js JavaScript code to control the LED and collect
dummy data from the simulated sensor.

At first run, the simulator operates a sample temperature capture program which
is displayed via the command line. The same sample application can also be run
on a real Pi as the simulator is designed to allow people to test code before
transferring it to a real device.

## Web simulator

There are three areas in the web simulator:

1.  Assembly area - The default circuit is that a Pi connects with a BME280
    sensor and an LED. The area is locked in preview version so currently you
    cannot do customization.

2.  Coding area - An online code editor for you to code with Raspberry Pi. The
    default sample application helps to collect sensor data from BME280 sensor
    and sends to your Azure IoT Hub. The application is fully compatible with
    real Pi devices.

3.  Integrated console window - It shows the output of your code. At the top of
    this window, there are three buttons.

    -   **Run** - Run the application in the coding area.

    -   **Reset** - Reset the coding area to the default sample application.

    -   **Fold/Expand** - On the right side there is a button for you to
        fold/expand the console window.

[Overview image of Pi online simulator](./../media-draft/image1.png)

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>

-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->

