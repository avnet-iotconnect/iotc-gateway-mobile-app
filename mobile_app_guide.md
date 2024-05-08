# IoTConnect Mobile App with ST SensorTile.box PRO
This guide will walk through setting up the Avnet IoTConnect mobile app to be leveraged as a Bluetooth gateway.  
Acting as a gateway, the IoTConnect app will capture Bluetooth telemetry data and transport and transport it to the IoTConnect cloud to visualization.

## Prerequisites
In addition to the ST SensorTile.box PRO device, you will need the following:  
1. A free [MyST](https://my.st.com/cas/login) account  
2. An free "Test Drive" [IoTConnect on AWS](https://subscription.iotconnect.io/subscribe?cloud=aws) account  
3. ST SensorTile.box PRO firmware `FP-SNS-xxx_BLESensorsPnPL` from [here](https://www.st.com/en/embedded-software/fp-sns-stbox1.html) (select v1.60 from the drop-down)  
4. An Android or iOS device
5. A PC with the [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) installed
6. A USB-A to USB-C cable
7. Desire to learn!

## 1. Download the IoTConnect Application  

### iOS
Scan the QR Code below to download the iOS app.  
![avnet me_IoTC-bridge-ios - WHITE](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/c7c29253-04a8-4387-a836-54c5e593cfcf)

### Android
Scan the QR Code below to download the Android app.  
![avnet me_IoTC-bridge-android - WHITE](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/71b4dc44-9647-4266-af91-df44a99f7a3e)

Launch the "IoT Bridge" application and proceed to the next section.  

## 2. Create an IoTConnect Account  
> **Note:**  
> If you've already created an account, you may skip to the next section.  

If you need to create an account, follow one of the methods below:  

<details>
  <summary>Method 1: Use the App [Recommended]</summary>

1. Press the **Sign Up** button at the bottom<br>
<img width="254" alt="login" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/641162e4-e4c3-410f-b1a4-48bc62dafe9b"><br>

2. Ensure "AWS" is selected as the Service Provider and click **Subscribe Now** for the "Test Drive"<br>
<img width="263" alt="subscribe" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/1a43f6ca-d13d-4a33-83f9-fb91ba0fda6e"><br>

3. Complete the registration form.<br>

4. Expect two emails from IoTConnect, one of which contains a temporary password.<br>
</details>

<details>
  <summary>Method 2: Use a Browser</summary>
  
  1. Navigate to the [IoTConnect Subscription](https://subscription.iotconnect.io/subscribe?cloud=aws) page<br>
  
  2. Ensure "AWS" is selected as the Service Provider and click **Subscribe Now** for the "Test Drive"<br>
  <img width="263" alt="subscribe" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/1a43f6ca-d13d-4a33-83f9-fb91ba0fda6e"><br>
  
  3. Complete the registration form.<br>
  
  4. Expect two emails from IoTConnect, one of which contains a temporary password.<br>
</details>

> **TIP:**
> Check your email SPAM folder if you don't receive the emails after a couple minutes.

## 3. Enter Credentials and Choose Environment  
* Return the mobile app home screen and enter your credentials.  
* Ensure that `console.iotconnect.io (AWS)` is selected as the environment to use and press **Login**.  

## 4. Update Firmware  
A firmware version that exposes the sensor data via Bluetooth is required to be loaded on the device.  

1. Connect power to the device (e.g. "ST SensorTile.box PRO") using a USB cable.  
2. Open the STM32CubeProgrammer  
3. Select **USB** for the connect type and select **Connect**  
4. Click the "Erasing & Programming" icon and browse to the previously downloaded file:  
`FP-SNS-xxx_BLESensorsPnPL` v1.60  
5. Click **Start Programming**
6. Once completed sucessfully, close the application and power cycle the device.

> For additional information and troubleshooting, please refer to Chapter 2.2 of the [ST SensorTile.box PRO Getting Started Guide](https://www.st.com/resource/en/user_manual/um3133-getting-started-with-sensortilebox-pro-multisensors-and-wireless-connectivity-development-kit-for-any-intelligent-iot-node-stmicroelectronics.pdf)  

## 5. Connect Device and Push Data 
With the IoTConnect mobile app open and the SensorTile device powered, it will be displayed in the device list.  
* If after a short time no devices are displayed, press the green menu button in the lower-right corner and select "**Scan Device**".
<img width="48" alt="menu_button" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/d85cb0b6-42bc-4894-9352-b11e891d9a8c">  

* Identify the device to connect to (e.g. "STbox...") and click on it.

Clicking on the device will automatically perform the following steps:  
1. Create the device template in the IoTConnect platform  
2. Create a device in the IoTConnect platform using the identified device template
3. Display a list of the detected sensors  

* Choose the **Select All** option at the top to select all available data and press **Push Data**

* After a few seconds, the sensor values will be updated in the mobile app and subsequently sent to IoTConnect.  

## 6. View Live Data on IoTConnect  
Using your credentials, login to IoTConnect in a browser:  
* IoTConnect on AWS - [https://console.iotconnect.io](https://console.iotconnect.io)  

Once logged in, select the Devices icon from the left-hand menu and click "Device"  
<img width="207" alt="select_device" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/0e9f2f60-b023-4abf-a88c-324c65cb2614">


The automatically created device will be visible.  
* Click on the **Unique ID** of the device to display the Device Info  
* Click on the **Live Data** to view the data from the Mobile App  
<img width="207" alt="live_data" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/66835231-02c5-4b7c-bb97-5c9ffdfacd73">

## 7. Visualize the Data  
Dashboards are used to easily visualize data.  
Some pre-made dashboards are available for the supported edge devices.  
Download the dashboard template which corresponds to your device:  
* [ST SensorTile.box PRO](dashboards/SensorTileBoxPRO_dashboard_export.json)

* Once downloaded, select "Create Dashboard" from the top of the IoTConnect portal and then choose the "Import Dashboard" option.
<img width="461" alt="create_dashboard" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/a852f939-340a-4f71-931b-f0ce70c63f34">

* Enter a name for the dashboard (e.g. "My SensorTile Dashboard")  
* Click "Browse" and select the dashboard template (.json) file downloaded previously which will cause two new fields to appear.  
* Select "AvnetSTaws" from the **Template** drop-down  
* Select the unique device name from the **Device** drop-down  
* Click **Save**  
<img width="454" alt="save_device" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/a3a2ceef-0d8d-46c5-8db3-971a119d0026">  

* Feel free to edit the dashboard or just click the blue **Save** button in the top right.
<img width="257" alt="save_dashboard" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/37062068-6472-4c02-9016-3d606584893a">
