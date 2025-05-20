# IoTConnect Mobile App with Microchip EV19J06A Development Board

This guide provides instructions for setting up the Avnet IoTConnect Mobile App as a Bluetooth gateway to stream telemetry data from the Microchip EV19J06A Development Board. Captured data is securely uploaded to the IoTConnect cloud for visualization.

![Microchip EV19J06A Development Board](../../images/WBZ35X_couriosity_board.PNG)

### Telemetry Supported:
- **Temperature**
- **LED Status**

---

## Prerequisites

Before starting, ensure you have:

- An [/IOTCONNECT](https://iotconnect.io) account (see [Cloud Account Setup](#cloud-account-setup))
- Android or iOS smartphone (*Bluetooth Gateway*)
- [Microchip EV19J06A Development Board](https://www.microchip.com/en-us/development-tool/EV19J06A)
- Latest firmware HEX file from [Microchip website](https://www.microchip.com/en-us/development-tool/EV19J06A)
- [MPLAB® X IDE](https://www.microchip.com/en-us/tools-resources/develop/mplab-x-ide) or [MPLAB IPE](https://www.microchip.com/en-us/tools-resources/production/mplab-integrated-programming-environment)
- [PICkit™ 5 Programmer](https://www.microchip.com/en-us/development-tool/pg164150)
- [AC102015 Adapter Board](https://www.microchip.com/en-us/development-tool/AC102015)
- USB-C cable
- PC (Windows, macOS, Linux)

![PICkit 5 Programmer](../../pick5.PNG)

![AC102015 Adapter Board](../../AC102015-Adapter-Board.PNG)

---

## Step-by-Step Guide

### 1. Download IoTConnect Mobile App  

Use the QR codes below:

| iOS App | Android App |
|---------|-------------|
| ![iOS App](apps/avnet.me_IoTC-bridge-ios-WHITE.png) | ![Android App](apps/avnet.me_IoTC-bridge-android-WHITE.png) |

Launch the app after installation.

---

## Cloud Account Setup

An /IOTCONNECT account with AWS backend is required. A free trial subscription is available through either of the following options:

- **Option #1:** [/IOTCONNECT via AWS Marketplace](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/iotconnect_aws_marketplace.md) **(Recommended)** - 60-day trial; AWS account creation required
- **Option #2:** [/IOTCONNECT via iotconnect.io](https://subscription.iotconnect.io/subscribe?cloud=aws) - 30-day trial; no credit card required

> **NOTE:**  
> Check your SPAM folder for the temporary password after registering.

For more information, refer to the [/IOTCONNECT Subscription Details](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/subscription.md).

---

### 3. Log into IoTConnect Mobile App  

- Enter your IoTConnect credentials.
- Select environment: `console.iotconnect.io (AWS)`
- Tap **Login**

![Login Screen](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/641162e4-e4c3-410f-b1a4-48bc62dafe9b)

---

### 4. Program Firmware onto EV19J06A Board  

Choose **one** of the following methods:

#### Option A: MPLAB® X IDE  

- Connect PICkit 5 and Adapter to the EV19J06A board and your PC.
- Launch MPLAB® X IDE, select device and programmer (PICkit 5).
- Load the firmware project, then **Make and Program Device**.

![MPLAB X IDE](https://www.microchip.com/content/dam/mchp/mrt-dam/media-block-images/development-tools/IDE/MPLABX_IDE_v6.0_Main.jpg)

#### Option B: MPLAB IPE *(recommended for HEX file only)*

- Connect PICkit 5 and Adapter to your EV19J06A board and PC.
- Launch MPLAB IPE, select your device, choose PICkit 5.
- Load HEX firmware file using **Browse**, and click **Program**.

![MPLAB IPE](https://www.microchip.com/content/dam/mchp/mrt-dam/media-block-images/tools-resources/programming-and-debugging/MPLAB%20IPE%20v6.0.png)

---

### 5. Connect Device & Publish Data  

- Power the EV19J06A board.
- Open IoTConnect Mobile App:
  - Tap the green menu button → **Scan Device** if device not visible.
  
  ![Scan Device](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/d85cb0b6-42bc-4894-9352-b11e891d9a8c)

- Select your device from the list (e.g., "Microchip EV19J06A").

The app will automatically:

- Create IoTConnect template and register your device.
- Show available sensors (Temperature, LED Status).
- Select **Select All** → **Push Data**.

---

### 6. View Live Data in IoTConnect Cloud  

- Log into [IoTConnect Console](https://console.iotconnect.io).
- Navigate to **Devices → Device**.
- Select your device’s **Unique ID**, click on **Live Data**.

![IoTConnect Live Data](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/66835231-02c5-4b7c-bb97-5c9ffdfacd73)

---

### 7. Visualize Data with Dashboards  

- Download the EV19J06A dashboard template (link TBD).
- In IoTConnect: **Create Dashboard → Import Dashboard**.
- Enter a dashboard name, upload `.json` file, select your device, and **Save**.

![Import Dashboard](https://github.com/avnet-iotconnect/iotc-gateway-mobile-app/assets/40640041/a3a2ceef-0d8d-46c5-8db3-971a119d0026)

Your telemetry data will now display visually on your custom dashboard.

---

## Optional Next Steps  

- Configure IoTConnect analytics and alerts.
- Explore expanding your setup by adding additional sensors or modules.

---

## Additional Resources  

- [Microchip EV19J06A Product Page](https://www.microchip.com/en-us/development-tool/EV19J06A)
- [PICkit 5 Programmer](https://www.microchip.com/en-us/development-tool/pg164150)
- [AC102015 Adapter Board](https://www.microchip.com/en-us/development-tool/AC102015)

Your Microchip EV19J06A Development Board is now successfully connected to IoTConnect!
