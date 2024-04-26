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

## 2. Create an IoTConnect Account  
An  IoTConnect account is required.  If you need to create an account, a free, 2-month subscription is available on either AWS or Azure

Please see the  
[Creating a New IoTConnect Account](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/subscription.md)
page for more information.
* [IoTConnect - AWS version](https://subscription.iotconnect.io/subscribe?cloud=aws)  
* [IoTConnect - Azure version](https://subscription.iotconnect.io/subscribe?cloud=azure)  

## 3. Power On Edge Device  
Connect power to one of the supported edge devices.  An updated list is available in the [README](README.md).

## 4. Login to IoTConnect  
Enter your credentials and select the IoTConnect instance which matches the version for which you registered.  

[screenshot]  
  
## 5. Connect Device  
The Device List screen will display any supported devices which are in Bluetooth range of your phone/tablet.  
[Device List Picture]  

Clicking on any device will automatically perform the following steps:
* Obtain the corresponding device template  
* Create the device template in the IoTConnect platform  
* Create a device in the IoTConnect platform
* Display a list of the detected sensors

## 6. Push Data  
Confirm the list of Sensors is as expected and press  **Push Data** to begin sending live data to the IoTConnect platform.  
[screenshot]  

After a few seconds, the sensor values will be updated in the mobile app and subsequently sent to IoTConnect.  
[screenshot]  


## 7. View Live Data on IoTConnect  
Using your credentials, login to the corresponding version of IoTConnect in a browser:  
* IoTConnect on AWS - [https://console.iotconnect.io](https://console.iotconnect.io)  
* IoTConnect on Azure - [https://portal.iotconnect.io](https://portal.iotconnect.io)

Once logged in, select the Devices icon from the left-hand menu and click "Device" 
[screenshot]  

* The automatically created device will be visible.  
* Click on the **Unique ID** of the device to display the Device Info  
* Click on the **Live Data** to view the data from the Mobile App

## 8. Visualize the Data  
Dashboards are used to easily visualize data.  
Some pre-made dashboards are available for the supported edge devices.  
Download the dashboard template which corresponds to your device:
* Dashboard 1
* Dashboard 2
* Dashboard 3

Import the template...
