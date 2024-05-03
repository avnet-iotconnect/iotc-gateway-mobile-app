# Mobile App Guide

## 1. Download the Application  

### iOS
View the application in the [Apple Store]() or just the QR Code below.  
```
:
: QR Code
:
```

### Android
View the application in the [Play Store]() or just the QR Code below.  
```
:
: QR Code
:
```

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

## 4. Connect Device and Push Data 
The Device List screen will display any supported devices which are in Bluetooth range of your phone/tablet.  
* If after a short time no devices are displayed, press the green "hamburger" menu button in the lower-right corner and select "**Scan Device**".
* Identify the device to connect to (e.g. "STbox...") and click on it.

Clicking on the device will automatically perform the following steps:
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
[screenshot]  

* The automatically created device will be visible.  
* Click on the **Unique ID** of the device to display the Device Info  
* Click on the **Live Data** to view the data from the Mobile App

## 6. Visualize the Data  
Dashboards are used to easily visualize data.  
Some pre-made dashboards are available for the supported edge devices.  
Download the dashboard template which corresponds to your device:
* Dashboard 1
* Dashboard 2
* Dashboard 3

Import the template...
