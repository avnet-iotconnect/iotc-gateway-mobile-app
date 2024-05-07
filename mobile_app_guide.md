# Mobile App Guide

## 1. Download the Application  

### iOS
Scan the QR Code below to download the iOS app.  
![avnet me_IoTC-bridge-ios - WHITE](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/c7c29253-04a8-4387-a836-54c5e593cfcf)

### Android
Scan the QR Code below to download the Android app.  
![avnet me_IoTC-bridge-android - WHITE](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/71b4dc44-9647-4266-af91-df44a99f7a3e)

Launch the "IoTConnect" application and proceed to the next section.  

## 2. Create an IoTConnect Account  
Upon opening the application, you will be prompted to enter your IoTConnect account credentials.  
If you need to create a 2-month free trial account folow these steps:
1. Press the **Sign Up** button at the bottom
2. Complete the registration form
3. Create a Password
4. Finish. You now have an IoTConnect account on the AWS cloud.

* If prompted, return to the login screen.  
* Ensure that `console.iotconnect.io (AWS)` is selected as the environment to use and press **Login**.

## 3. Power On Edge Device  
Connect power to one of the supported edge devices (e.g. "ST SensorTile.box PRO") using an appropriate USB cable.  
An updated list is maintained in the [README](README.md).

```
Firmware Update Section Placeholder
ST App?  CubeProgrammer?
Version? FP-SNS-xxx_BLESensorsPnPL v1.60?
```

## 4. Connect Device and Push Data 
The Device List screen will display any supported devices which are in Bluetooth range of your phone/tablet.  
* If after a short time no devices are displayed, press the green menu button in the lower-right corner and select "**Scan Device**".
* Identify the device to connect to (e.g. "STbox...") and click on it.

> Note:
> Clicking on the device will automatically perform the following steps:  
1. Obtain the corresponding device template  
2. Create the device template in the IoTConnect platform  
3. Create a device in the IoTConnect platform using the identified device template
4. Display a list of the detected sensors  

* Choose the **Select All** option at the top to select all available data and press **Push Data**

* After a few seconds, the sensor values will be updated in the mobile app and subsequently sent to IoTConnect.  

## 5. View Live Data on IoTConnect  
Using your credentials, login to IoTConnect in a browser:  
* IoTConnect on AWS - [https://console.iotconnect.io](https://console.iotconnect.io)  

Once logged in, select the Devices icon from the left-hand menu and click "Device"  
<img width="207" alt="select_device" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/0e9f2f60-b023-4abf-a88c-324c65cb2614">


The automatically created device will be visible.  
* Click on the **Unique ID** of the device to display the Device Info  
* Click on the **Live Data** to view the data from the Mobile App  
<img width="207" alt="live_data" src="https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/66835231-02c5-4b7c-bb97-5c9ffdfacd73">

## 6. Visualize the Data  
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
