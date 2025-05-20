# IoTConnect Mobile App Gateway Repository

This repository contains integration guides and dashboard templates for the IoTConnect Mobile App Gateway, supporting various development boards from Microchip and STMicroelectronics.

## Mobile App as a Gateway

A mobile phone or tablet can act as a gateway for Bluetooth-connected edge devices. Both iOS and Android versions of the IoTConnect Mobile App ("IoT Bridge") are available. Download using the QR codes below:

### iOS App
<img src="apps/avnet.me_IoTC-bridge-ios-WHITE.png" alt="IoTConnect iOS App" width="200"/>

### Android App
<img src="apps/avnet.me_IoTC-bridge-android-WHITE.png" alt="IoTConnect Android App" width="200"/>

---

## Cloud Account Setup

An IoTConnect cloud account (AWS backend) is required:

- **Option #1:** [IoTConnect via AWS Marketplace](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/iotconnect_aws_marketplace.md) *(Recommended; 60-day trial)*
- **Option #2:** [IoTConnect via iotconnect.io](https://subscription.iotconnect.io/subscribe?cloud=aws) *(30-day trial; no credit card required)*

For subscription details, see the [IoTConnect Subscription Information](https://github.com/avnet-iotconnect/avnet-iotconnect.github.io/blob/main/documentation/iotconnect/subscription/subscription.md).

---

## Supported Edge Devices

### Microchip Reference Designs
- [Secure Telehealth Reference Design](targets/microchip/secure-telehealth-reference-design/README.md)
- [EV19J06A (PIC32CX-BZ3 and WBZ35x Curiosity Development Board)](targets/microchip/EV19J06A-curiosity-dev-board/README.md)

### STMicroelectronics Reference Designs
- [ST PROTEUS (STEVAL-PROTEUS1)](https://www.st.com/en/evaluation-tools/steval-proteus1.html)
- [ST SensorTile.box PRO (STEVAL-MKBOXPRO)](https://www.st.com/en/evaluation-tools/steval-mkboxpro.html)
- [STEVAL-ASTRA1B](https://www.st.com/en/evaluation-tools/steval-astra1b.html)
- [STEVAL-STLKT01V1](https://www.st.com/en/evaluation-tools/steval-stlkt01v1.html)

---

## Walkthroughs & Guides

- [Microchip Secure Telehealth Reference Design Guide](targets/microchip/secure-telehealth-reference-design/README.md)
- [Microchip EV96B94A Development Board Guide](targets/microchip/wbz451-curiosity-dev-board/README.md)
- [ST SensorTile.box PRO Mobile App Guide](targets/stmicro/README.md)

---

## Dashboard Templates

Dashboard JSON templates to quickly visualize telemetry data are available in each device's respective directory:

- **Microchip Dashboards**: [targets/microchip](targets/microchip/)
- **STMicroelectronics Dashboards**: [targets/stmicro](targets/stmicro/)

---

For additional support or to request new device integrations, please open an issue in this repository.
