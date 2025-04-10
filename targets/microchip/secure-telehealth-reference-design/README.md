# IoTConnect Mobile App with Microchip Secure Telehealth Reference Design  

This guide explains how to configure the Avnet IoTConnect Mobile App as a Bluetooth gateway for the Microchip Secure Telehealth Reference Design. The app captures telemetry data via Bluetooth and securely uploads it to IoTConnect for cloud visualization.

![Microchip Telehealth Reference Design](https://ww1.microchip.com/downloads/aemDocuments/images/MCHP-Secure-Telehealth-Main_1120x840.jpg)

### Telemetry Supported:
- **Quality of Measurement**
- **Temperature**
- **Heart Rate**
- **SPO₂ (Blood Oxygen Level)**

---

## Prerequisites  

Ensure you have:

- An [/IOTCONNECT](https://iotconnect.io) account (see [Cloud Account Setup](#7-cloud-account-setup))
- Android or iOS smartphone (*Bluetooth Gateway*)
- [Microchip Secure Telehealth Reference Design](https://www.microchip.com/en-us/tools-resources/reference-designs/secure-telehealth-reference-design)  
- Firmware HEX file from [Microchip website](https://www.microchip.com/en-us/tools-resources/reference-designs/secu`re-telehealth-reference-design)
- [MPLAB® X IDE](https://www.microchip.com/en-us/tools-resources/develop/mplab-x-ide) or [MPLAB IPE](https://www.microchip.com/en-us/tools-resources/production/mplab-integrated-programming-environment)  
- [PICkit™ 5 Programmer](https://www.microchip.com/en-us/development-tool/pg164150)  
  <img width="200" alt="PICkit" src="https://www.waveshare.com/media/catalog/product/cache/1/image/800x800/9df78eab33525d08d6e5fb8d27136e95/p/i/pickit-5-2.jpg">
- [AC102015 Adapter Board](https://www.microchip.com/en-us/development-tool/AC102015)   
  <img width="200" alt="Adapter" src="https://akizukidenshi.com/img/goods/L/114442.jpg">
- USB-C cable  
- PC (Windows, macOS, Linux)

---

## Step-by-Step Guide

### 1. Download IoTConnect Mobile App  

Use QR codes below:

| iOS App                                                                           | Android App                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| <img width="75" alt="ios" src="../../../apps/avnet.me_IoTC-bridge-ios-WHITE.png"> | <img width="75" alt="android" src="../../../apps/avnet.me_IoTC-bridge-android-WHITE.png" > |

Launch the app once installed.

---

### 2. Cloud Account Setup

An /IOTCONNECT account with AWS backend is required. If you need to create an account, a free trial subscription is available.  
The free subscription may be obtained directly from iotconnect.io or through the AWS Marketplace.

- **Option #1:** [/IOTCONNECT via AWS Marketplace](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/iotconnect_aws_marketplace.md) **(Recommended)** - 60-day trial; AWS account creation required
- **Option #2:** [/IOTCONNECT via iotconnect.io](https://subscription.iotconnect.io/subscribe?cloud=aws) - 30-day trial; no credit card required

> **NOTE:**  
> Be sure to check your SPAM folder for the temporary password after registering.

See the [/IOTCONNECT Subscription Information](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/subscription.md) for more details.

---

### 3. Log into IoTConnect Mobile App  

- Enter your credentials  
- Select environment: `console.iotconnect.io (AWS)`  
- Tap **Login**

![Login](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/641162e4-e4c3-410f-b1a4-48bc62dafe9b)

---

### 4. Program Firmware onto Telehealth Device  

Choose **one** programming method:

#### Option A: MPLAB® X IDE  

- Connect PICkit 5 and Adapter to your Telehealth device and PC.
- Open MPLAB® X IDE, select your device and PICkit 5, load project, then **Make and Program Device**.

  <img width="180" alt="mplabxide" src="https://cc.sj-cdn.net/instructor/kt1ejklh2ngf-microchip-technology/courses/13vwh8sh89nwy/promo-image.1610634123.png">

#### Option B: MPLAB IPE *(HEX file only)*

- Connect PICkit 5 and Adapter to your Telehealth device and PC.
- Open MPLAB IPE, select your device, PICkit 5, load HEX file, and press **Program**.

  <img width="180" alt="mplabipe" src="https://img.informer.com/icons_mac/png/128/681/681285.png">

---

### 5. Connect Device & Publish Data  

- Power the device.
- Open IoTConnect Mobile App:
  - Tap the green menu button, select **Scan Device** if needed.
  
  ![Scan Device](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/d85cb0b6-42bc-4894-9352-b11e891d9a8c)

- Select your device from the list (e.g., "Microchip Telehealth").

The app automatically:

- Creates IoTConnect template and registers your device.
- Lists available sensors (Quality, Temperature, Heart Rate, SPO₂).
- Choose **Select All** → **Push Data**.

---

### 6. View Live Data in IoTConnect Cloud  

- Log into [IoTConnect Console](https://console.iotconnect.io).
- Navigate to **Devices → Device**.
- Click your device’s **Unique ID**, then **Live Data**.

![IoTConnect Live Data](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/66835231-02c5-4b7c-bb97-5c9ffdfacd73)

---

### 7. Visualize Data with Dashboards  

- Download the dashboard template (link TBD).
- In IoTConnect, select **Create Dashboard → Import Dashboard**.
- Name dashboard, upload `.json` file, select your device, and **Save**.

![Import Dashboard](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/a3a2ceef-0d8d-46c5-8db3-971a119d0026)

Your telemetry data is now visualized on a custom dashboard.

---

### Optional Next Steps  

- Configure IoTConnect alerts for critical thresholds.
- Expand system capabilities via additional sensors or modules.

---

## Additional Resources  

- [Microchip Telehealth Reference Design Page](https://www.microchip.com/en-us/tools-resources/reference-designs/secure-telehealth-reference-design)
- [PICkit 5 Programmer](https://www.microchip.com/en-us/development-tool/pg164150)
- [AC102015 Adapter Board](https://www.microchip.com/en-us/development-tool/AC102015)

Your Microchip Secure Telehealth Reference Design is now successfully integrated with IoTConnect!
